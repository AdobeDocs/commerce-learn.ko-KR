---
title: '분할 결제 POC: 테스트 및 확인 가이드'
description: '분할 결제 POC를 확인하는 방법: Commerce 설치, REST, 체크아웃, 임계값, 시뮬레이션 수락 및 거부, 데모 대시보드 및 App Builder 로그를 알아봅니다.'
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# 분할 결제 POC: 테스트 및 확인 가이드

이 안내서에서는 테스트해야 하는 순서대로 모든 구성 요소가 제대로 작동하는지 확인하는 과정을 안내합니다. 맨 아래(Commerce 모듈)에서 시작하여 위로(App Builder) 이동합니다.


## 1단계 — Commerce 모듈 설치 확인

`bin/magento setup:upgrade` 및 `bin/magento setup:di:compile`을(를) 실행한 후:

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

예상 출력: `split_`(으)로 시작하는 4개의 열이 `sales_order`에 표시됩니다.


## 2단계 — Commerce 관리 구성 확인

Commerce 관리에서:
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** —**[!UICONTROL Cash On Delivery]**&#x200B;이(가) 제목 `Cash`을(를) 사용하여 활성화되었는지 확인
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — 활성화됨 확인
3. 테스트 고객에게 스토어 크레딧이 있는지 확인합니다. **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[고객]* > **[!UICONTROL Store Credit]**


## 3단계 — REST 엔드포인트에 액세스할 수 있는지 확인

curl을 사용하여 엔드포인트가 응답하는지 확인합니다(승인되지 않은 요청을 거부하지만 401에서 올바르게 라우팅되는지 확인).

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## 4단계 — 체크아웃 UI 테스트

1. 상점 크레딧을 받은 테스트 고객으로 상점 앞에 로그인합니다.
2. 장바구니에 제품 추가(배송 후 총 $100 미만 + 세금)
3. 체크아웃 진행
4. 결제 단계에서 **현금**(또는 배달 시 현금)을 선택합니다.
5. 분할 지급 필드가 표시되는지 확인합니다.
   * 스토어 크레딧 잔액이 표시됩니다.
   * 주문 합계로 미리 채워진 현금 금액 필드
   * $0.00를 표시하는 &quot;이 주문에 대한 스토어 크레딧&quot; 필드(현금 = 전체 주문 합계)
6. 현금 금액 감소(예: $50 주문에 $10 입력)
7. 스토어 크레딧 금액이 $40.00로 업데이트되었는지 확인
8. 메시지가 표시되는지 확인: `"The remaining $40.00 will automatically be applied from your store credit."`

**테스트 유효성 검사:**
* 주문 총액보다 큰 현금 금액 → 오류 메시지 입력
* 사용 가능한 → 오류 메시지보다 더 많은 상점 크레딧이 필요한 현금 금액을 입력하십시오.
* 현금 = 0 → 오류 입력(또는 스토어 크레딧이 전체 주문을 포함)


## 5단계 — 임계값 가드 테스트

1. 합계 $100 이상인 제품 추가(소계 + 배송 + 세금 > $100)
2. 체크아웃을 진행합니다. **현금**&#x200B;을(를) 선택하세요.
3. 주문 시도
4. 오류 메시지가 표시되는지 확인합니다. `"Payment could not be processed. Please try again or contact support."`
5. 장바구니가 보존되는지 확인합니다(고객은 여전히 장바구니를 조정하고 다시 시도할 수 있음).


## 6단계 — 테스트 분할 지급 주문

1. $100(스토어 크레딧으로 로그인한 고객) 미만의 장바구니 구축
2. 체크아웃 시 현금 선택
3. 주문 총액보다 적은 현금 금액(예: $45 주문의 $10) 입력
4. 스토어 크레딧 메시지가 표시되는지 확인
5. **주문** 클릭

주문 배치 후 Commerce 관리에서 다음을 확인하십시오.
* 주문은 `pending_payment` 상태입니다.
* Order에는 두 개의 내역 주석이 있습니다.
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`(App Builder `payment-orchestrator`에서)
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`(App Builder)
* 분할 결제 금액은 주문 결제 블록에 표시됩니다

> **App Builder 댓글이 표시되지 않는 경우:** `aio app logs`(으)로 App Builder 작업 로그를 확인하십시오. The event may not have fired or the action may have an error.


## Step 7 — Test Accept via Simulation Script

The simulation script is the fastest way to test the accept/decline flow without the full operator UI.

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

After accept, verify in Commerce Admin order view:
* Order status is `processing`
* History comment: `"Cash payment of $X.XX received."`
* Cash invoice created (visible in Invoices tab)
* Shipment created (visible in Shipments tab, if applicable)
* History comment: `"Split payment: cash portion invoiced #XXXXXXXX."`
* History comment: `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## Step 8 — Test Decline via Simulation Script

Place another test order (same setup as Step 6), then:

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

After decline, verify in Commerce Admin:
* Order status is `canceled`
* History comment: `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## Step 9 — Test the Demo Dashboard

After deploying the `split-payment-orchestrator`, `aio app deploy` prints the action URLs.

Open the `demo-dashboard` URL in a browser:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

If `DEMO_UI_SECRET` is set:

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

With a pending order:
1. The dashboard should show the order in the pending list
2. Click **Accept** → order should move to `processing` in Commerce
3. Place another order; click **Decline** → order should be `canceled` in Commerce


## Step 10 — Test App Builder Action Logs

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

For the `payment-orchestrator`, look for:

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

`payment-accept` 또는 `payment-decline`의 경우:

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## 일반적인 문제 및 수정 사항

### Commerce OAuth에서 &quot;서명이 잘못되었습니다.&quot;

**원인:** App Builder 런타임과 Commerce 사이의 클럭 스큐 또는 OAuth 서명 버그입니다.

**수정:**
* `COMMERCE_BASE_URL` 뒤에 슬래시가 없는지 확인
* 4개의 OAuth 자격 증명이 _활성화_ 통합에 대한 자격 증명인지 확인합니다.
* 먼저 시뮬레이션 스크립트를 사용하여 테스트합니다. 스크립트가 작동하지만 App Builder이 작동하지 않는 경우 환경 변수가 로드되지 않았을 수 있습니다(누락된 환경 변수에 대해 `aio app deploy` 출력 확인).

### 분할 결제 필드가 체크아웃에 표시되지 않음

**원인:** LayoutProcessorPlugin 삽입 경로가 체크아웃 레이아웃과 일치하지 않습니다.

**수정:**
* 브라우저 콘솔에서 `Client_SplitPayment/js/view/payment/split-payment`을(를) 로드하는 RequireJS 오류를 확인하십시오.
* `bin/magento setup:static-content:deploy` 확인이 완료되었습니다.
* 캐시 초기화: `bin/magento cache:flush`
* 테마에 사용자 지정 체크 아웃이 있는 경우 구성 요소를 삽입할 `LayoutProcessorPlugin` 경로를 조정해야 할 수 있습니다

### 스토어 크레딧이 적용되지 않음 / 주문 시 &quot;결제를 처리할 수 없음&quot;

**원인:** 일반적으로 Edge Case 플러그인 중 하나가 제대로 작동하지 않습니다.

**확인:**
1. `SplitPaymentSession`에서 올바른 금액을 반환하고 있습니까? `PlaceOrderPlugin`에 임시 디버그 로깅 추가
2. `BalanceManagementInterface::apply()`이(가) 호출되기 전에 `FixSplitPaymentGrandTotalPlugin`이(가) 실행 중이고 견적 합계에 영향을 주고 있습니까? `beginBalanceApply()` 플래그는 표시하지 않아야 합니다.
3. Commerce에서 고객 균형 모듈이 활성화되어 있습니까?

### App Builder 작업이 이벤트를 수신하지만 orderId가 누락되었습니다.

**원인:** `io_events.xml` 필드 목록에 `entity_id`이(가) 없거나 이벤트 페이로드 모양이 변경되었습니다.

**수정:**
* `io_events.xml`이(가) 필드 목록에 `entity_id`을(를) 포함하는지 확인
* 작업에서 `JSON.stringify(params)`을(를) 임시로 로그하여 전체 페이로드 모양을 확인합니다.
* `extractValue()` 함수가 올바른 중첩 수준을 찾고 있는지 확인합니다.

### 주문이 데모 대시보드에 표시되지 않음

**원인:** Commerce REST `orders` 검색 조건이 주문을 반환하지 않거나 `split_cash_status` 필드가 REST 응답에 없습니다.

**수정:**
* `OrderRepositoryPlugin`이(가) 확장 특성을 올바르게 로드하고 있는지 확인
* `GET /rest/V1/orders?searchCriteria[pageSize]=5`을(를) 직접 테스트하고 `extension_attributes.split_cash_status`이(가) 응답에 표시되는지 확인
* `extension_attributes.xml`이(가) `OrderInterface`에서 `split_cash_status` 특성을 올바르게 선언하고 있는지 확인하십시오.


## 확인 검사 목록

* [ `sales_order` 테이블에 ] `split_*` 열이 표시됨
* [ 인증 없이 호출하면 ] REST 끝점이 401(404가 아님)을 반환합니다.
* [ 현금을 선택하면 체크아웃 시 ] 분할 결제 UI 렌더링
* [ ]개의 유효성 검사 메시지가 작동함(과다 결제, 크레딧 부족)
* [ ] 임계값 보호기가 주문을 차단함 > $100
* [ ] 주문의 상태 및 App Builder 댓글은 `pending_payment`개입니다.
* [ ] `simulate-split-payment.mjs list`이(가) 분할 금액이 있는 테스트 순서를 표시합니다.
* [ ] `simulate-split-payment.mjs accept <id>`이(가) 주문 및 배송을 `processing`(으)로 이동
* [ ] `simulate-split-payment.mjs decline <id>`이(가) 순서를 취소합니다.
* [ ] 데모 대시보드에 보류 중인 주문 및 UI의 작업 수락/거부가 나열됩니다.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
