---
title: 분할 결제 POC 단계별 구현 안내서
description: 설정 완료 후 모듈, 환경, Orchestrator, 확인 및 선택적 UI 순서로 분할 결제 POC를 구현하는 방법을 알아봅니다.
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# 분할 결제 POC 단계별 구현 안내서

이 페이지는 구현 체크리스트입니다. [개요](./overview.md) 및 [전체 데모](./full-demo.md)를 이미 보았으며, Commerce 사이트 및 App Builder 프로젝트가 모두 준비되었다고 가정합니다. 개별 튜토리얼 페이지에는 아래 각 주제에 대한 전체 세부 정보가 포함되어 있습니다. 이 페이지를 사용하여 수행할 작업과 순서를 알아보십시오.

## 시작하기 전에

프롬프트 또는 명령을 실행하기 전에 다음 사항을 모두 확인하십시오. [필수 구성 요소 및 설정](./prerequisites-and-setup.md) 페이지는 각 항목을 자세히 다룹니다.

**Commerce 측**

* [ ] Adobe Commerce 2.4.5 이상
* [ ] I/O 이벤트가 활성화되고 배포됨(Commerce Cloud만 해당 - `.magento.env.yaml`에 `ENABLE_EVENTING: true`이(가) 필요)
* [ ] **배달 현금**&#x200B;이(가) 책임자에서 활성화되었으며 제목은 정확히 `Cash`(으)로 설정됨
* [ Admin에서 ] 스토어 크레딧이 활성화됨
* [ ] 테스트 고객에게 $50 이상의 스토어 크레딧이 있습니다.
* [ ] Commerce 통합이 만들어지고 활성화되었으며 네 개의 OAuth 값이 모두 안전하게 저장되었습니다.

**App Builder 측**

* [ ] App Builder 프로젝트가 Adobe Developer Console에 있습니다.
* [ 작업 영역에 ] Adobe I/O Events 서비스가 추가됨
* [ 이벤트 공급자로 연결된 ] Commerce
* [ ] `aio login` 완료, `aio app use`(으)로 올바른 작업 영역 선택됨
* [ ] Node.js 18 이상 설치, `aio` CLI 설치(`npm install -g @adobe/aio-cli`)

위의 내용이 누락된 경우 여기에서 중지하고 먼저 완료하십시오.

## 1단계 — Commerce 모듈 구축

**기능:** 체크아웃 UI, 저장소 신용 응용 프로그램, REST 끝점 및 I/O 이벤트 구독을 처리하는 `Client_SplitPayment` PHP 모듈을 생성합니다. 얇은 Commerce 내 어댑터입니다. 모든 연산자 워크플로는 App Builder에 있습니다.

### 1.1단계 — 프로젝트의 프롬프트 사용자 정의

[Commerce 모듈 AI 프롬프트](./commerce-module-prompt.md) 페이지를 열고 프롬프트를 복사하기 전에 다음을 참고하십시오.

* 프롬프트에서 `Client`을(를) 실제 공급업체 이름으로 바꾸기
* 다른 모듈 이름을 사용하려면 `SplitPayment`을(를) 바꾸십시오.
* 테마가 Luma가 아닌 경우 `LayoutProcessorPlugin` 삽입 경로 및 RequireJS 매핑을 조정해야 할 수 있습니다.
* Cash on Delivery 메서드가 `cashondelivery` 이외의 코드를 사용하는 경우 `payment-method-helper.js`을(를) 업데이트합니다.

### 1.2단계 — 프롬프트를 실행합니다.

전체 프롬프트를 **PROMPT START**&#x200B;에서 **프롬프트 끝**&#x200B;으로 복사한 다음 Cursor(Claude 사용)에 붙여넣거나 Claude에 바로 붙여넣습니다. AI가 `app/code/` 아래에 파일을 만들 수 있도록 Commerce 프로젝트의 루트에서 실행하십시오.

### 1.3단계 — 모듈 활성화 및 설치

Commerce 프로젝트 루트에서 다음 명령을 실행합니다.

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 1.4단계 — 모듈이 올바르게 설치되었는지 확인

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`, `split_sc_invoice_id` 및 `split_cash_invoice_id`의 5개 열이 표시됩니다.

그런 다음, 매장 크레딧이 있는 로그인한 고객이 결제 방법으로 **현금**&#x200B;을(를) 선택할 때 분할 결제 필드가 체크아웃 시 표시되는지 브라우저에서 확인합니다.

## 2단계 — 환경 변수 구성

**수행할 작업:** App Builder orchestrator, 시뮬레이션 스크립트 및 Experience Cloud UI 확장(선택 사항)에 사용되는 자격 증명 파일을 준비합니다. 모든 `.env` 파일에서 Commerce 통합의 동일한 4개의 OAuth 값이 사용됩니다.

구성 요소별 전체 변수 목록은 [환경 변수 참조](./env-reference.md)를 참조하십시오.

### 2.1단계 — Orchestrator `.env` 만들기

`split-payment-orchestrator/` 디렉터리에서:

```bash
cp .env.example .env
```

모든 값 입력:

| 변수 | 어디에서 구입할 수 있습니까 |
|---|---|
| `COMMERCE_BASE_URL` | 스토어 URL — 뒤쪽 슬래시가 없음 |
| `COMMERCE_CONSUMER_KEY` | Commerce 관리 > 시스템 > 통합 |
| `COMMERCE_CONSUMER_SECRET` | 동일 — 활성화 시에만 표시됨 |
| `COMMERCE_ACCESS_TOKEN` | 동일 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 동일 |
| `PAYMENT_THRESHOLD` | Commerce의 `split_payment/general/threshold`과(와) 일치해야 합니다(기본값: `100`). |
| `DEMO_UI_SECRET` | 선택 사항 — 설정된 경우 대시보드 URL에 `?secret=`(으)로 필요합니다. |

### 2.2단계 — 시뮬레이션 스크립트 `.env` 만들기

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

`COMMERCE_BASE_URL` 및 4개의 OAuth 값(위와 동일한 자격 증명)을 채웁니다.

## 3단계 - App Builder Orchestrator 구축

**기능:** `split-payment-orchestrator` App Builder 프로젝트: `payment-orchestrator` I/O 이벤트 소비자, `payment-accept` 및 `payment-decline` 웹 작업, `demo-dashboard` 연산자 UI를 생성합니다.

### 3.1단계 — Orchestrator 프롬프트 실행

[App Builder Orchestrator AI 프롬프트](./orchestrator-prompt.md) 페이지를 엽니다. 전체 프롬프트를 복사하고 `split-payment-orchestrator/` 디렉터리에서 실행하십시오.

### 3.2단계 — 종속성 설치 및 배포

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

배포 후 `aio app deploy`에서 작업 URL을 인쇄합니다. `demo-dashboard` URL을 기록해 두십시오. 4단계에서 필요합니다.

### 3.3단계 — 이벤트 등록 확인

Adobe Developer Console에서 App Builder 프로젝트 작업 영역을 엽니다. **이벤트**&#x200B;에서 `Split payment — sales order place before` 등록이 있으며 `payment-orchestrator` 작업에 바인딩되어 있는지 확인하십시오.

## 4단계 — 전체 플로우 테스트

확인 단계를 순서대로 진행합니다. [테스트 및 확인 안내서](./testing-and-verification.md)에는 아래 각 단계에 대한 전체 curl 명령, 시뮬레이션 스크립트 사용 및 문제 해결 참조가 있습니다.

### 4.1단계 — REST 엔드포인트가 라우팅 가능한지 확인

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### 4.2단계 — 체크아웃 UI 테스트

1. 상점 크레딧이 있는 테스트 고객으로 상점에 로그인합니다.
2. 배송 및 세금 공제 후 합계 $100 미만인 장바구니에 제품 추가
3. 결제 단계에서 **현금**&#x200B;을 선택하세요.
4. 분할 지급 필드가 표시되는지 확인합니다. 표시된 잔액, 선불 현금, $0에 있는 상점 대변
5. 현금 금액을 줄이고 매장 크레딧 부분 업데이트를 올바르게 확인

### 4.3단계 — 임계값 가드 테스트

총 $100 이상의 제품을 추가하고 체크아웃을 진행하고 현금을 선택한 다음 주문을 시도합니다. 오류 `"Payment could not be processed. Please try again or contact support."`이(가) 표시되어야 합니다.

### 4.4단계 — 테스트 분할 지급 주문

$100 미만으로 장바구니를 만들고, 현금/매장 크레딧 분할을 입력한 다음 주문합니다. Commerce 관리에서 다음을 확인합니다.

* 주문 상태는 `pending_payment`입니다.
* App Builder의 두 개의 주석이 순서대로 표시됩니다.
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

App Builder 댓글이 표시되지 않으면 계속하기 전에 `aio app logs`에 있는 로그를 확인하십시오.

### 4.5단계 — 시뮬레이션 스크립트를 통해 승인 테스트

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

Commerce 관리에서 다음을 확인합니다.

* 주문 상태가 `processing`(으)로 변경됨
* 기록 주석: `"Cash payment of $X.XX received."`
* 송장 탭에 표시되는 현금 송장
* 선적 탭에 표시되는 선적(해당되는 경우)

### 4.6단계 — 시뮬레이션 스크립트를 통해 감소 테스트

다른 테스트 순서를 지정한 다음

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

주문 상태가 `canceled`이고 `split_cash_status`이(가) `declined`인지 확인하십시오.

### 4.7단계 — 데모 대시보드 테스트

3.2단계에서 대시보드 URL을 엽니다. 시스템에 대기 중인 주문:

1. 대기 중 목록에 주문이 표시되는지 확인
2. **수락**&#x200B;을 클릭하고 Commerce에서 `processing`(으)로 순서가 이동하는지 확인하십시오.
3. 새 주문을 하고 대시보드로 돌아가서 **거부**&#x200B;를 클릭합니다. 취소를 확인합니다.

## 5단계(선택 사항) - Experience Cloud UI 확장 추가

이 단계에서는 독립 실행형 `demo-dashboard`을(를) 사용하지 않고 연산자 대시보드를 Commerce 관리 셸에 포함합니다. 독립 실행형 대시보드가 사용자의 요구 사항을 충족하면 이 단계를 건너뜁니다.

### 5.1단계 — 사전 요구 사항 검토

Experience Cloud UI 확장을 사용하려면 OAuth 통합 값 외에 IMS 자격 증명이 필요합니다. 프롬프트를 실행하기 전에 [Experience Cloud UI 확장 AI 프롬프트](./experience-cloud-ui-prompt.md) 페이지를 읽고 `OAUTH_CLIENT_ID` 및 관련 IMS 변수에 주의하십시오.

### 5.2단계 — UI 확장 프롬프트 실행

**PROMPT START**&#x200B;에서 **프롬프트 끝**(으)로 전체 프롬프트를 복사하고 `commerce-checkout-starter-kit/` 디렉터리에서 실행하십시오.

### 5.3단계 — 배포

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## 빠른 문제 해결 참조

| 증상 | 가장 먼저 확인해야 할 사항 |
|---|---|
| 분할 결제 필드가 체크아웃 시 표시되지 않음 | 브라우저 콘솔에서 RequireJS 오류가 발생했습니다. `bin/magento cache:flush`도 실행하십시오. |
| `"Payment could not be processed"`(주문) | `PlaceOrderPlugin` 또는 `BalanceManagementInterface` — `PlaceOrderPlugin`에서 임시 디버그 로깅을 추가합니다. |
| 주문에 대한 App Builder 주석 없음 | `aio app logs`을(를) 실행하여 이벤트가 실행되었는지 확인하고 작업이 반환되었는지 확인합니다. |
| App Builder의 Commerce REST에서 `403` | Fastly IP 허용 목록에 추가 — 먼저 시뮬레이션 스크립트로 테스트합니다. 제대로 작동하는 경우 문제는 이그레스 IP 차단입니다. |
| Commerce의 `"The signature is invalid"` | `COMMERCE_BASE_URL` 뒤에 슬래시가 없는지 확인합니다. 통합이 활성화되었는지 확인합니다. |
| 주문이 데모 대시보드에 표시되지 않음 | `extension_attributes.split_cash_status`이(가) 직접 `GET /rest/V1/orders` 응답에 표시되는지 확인 |

자세한 문제 해결 정보는 [테스트 및 확인 가이드](./testing-and-verification.md#common-issues-and-fixes)를 참조하세요.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
