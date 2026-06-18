---
title: 분할 결제 POC - Commerce 모듈 AI 프롬프트
description: 이 프롬프트를 사용하여 Client_SplitPayment를 생성하는 방법을 알아봅니다. REST, 플러그인, JavaScript 체크아웃, I/O 이벤트 및 명령 활성화, 컴파일 및 배포
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# 분할 결제 POC: Commerce 모듈 AI 프롬프트

분할 결제 개념 증명에 대한 `Client_SplitPayment` 처리 중인 모듈(REST, 세션 처리, **[!UICONTROL Checkout]** 및 **[!UICONTROL Admin]** 표시)을 생성하는 전체 프롬프트를 복사할 수 있습니다. 연산자 워크플로우는 App Builder에 있습니다.

## 이 프롬프트를 사용하는 방법

**PROMPT START**&#x200B;에서 **PROMPT END**&#x200B;까지의 모든 내용을 Cursor(Claude 포함)로 복사하거나 Claude로 직접 복사합니다. Commerce 프로젝트의 루트 또는 AI가 파일을 만들 수 있는 디렉터리에서 실행합니다.

## 실행하기 전에 사용자 지정

* `Client`을(를) 실제 공급업체 이름으로 바꾸십시오.
* 다른 모듈 이름을 사용하려면 `SplitPayment`을(를) 변경하십시오.
* 사이트에서 사용자 지정 테마를 사용하는 경우 레이아웃 XML 및 RequireJS 경로를 변경해야 할 수 있습니다.
* **[!UICONTROL Cash on delivery]** 메서드가 `cashondelivery`과(와) 다른 코드를 사용하는 경우 `payment-method-helper.js`을(를) 업데이트합니다.


## 프롬프트

**프롬프트 시작**

분할 결제 기능을 위해 프로덕션 준비가 완료된 완전한 Adobe Commerce 2.4.5 이상 in-process 모듈을 생성하고 있습니다. 이 모듈은 올바른 REST 표면을 노출하고 Commerce 라이프사이클에서 올바른 순간에 올바른 데이터를 연결하는 얇은 PHP 어댑터입니다. 모든 연산자 워크플로우 논리는 Adobe App Builder(이 모듈에는 없음)에 있습니다.

**모듈 ID:**
* 공급업체: `Client`
* 모듈: `SplitPayment`
* 전체 이름: `Client_SplitPayment`
* 네임스페이스: `Client\SplitPayment`
* 위치: `app/code/Client/SplitPayment/`
* 종속성: `Magento_Checkout`, `Magento_CustomerBalance`, `Magento_Sales`, `Magento_Quote`, `Magento_WebApi`, `Magento_AdobeCommerceEventsClient`

아래 파일 구조에 나열된 모든 파일을 생성합니다. 파일을 생략하지 마십시오. 모든 PHP 파일에서 `declare(strict_types=1)`을(를) 사용합니다.


### 생성할 파일 구조

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### 동작 사양

#### &#x200B;1. 데이터베이스 스키마(`etc/db_schema.xml`)

`sales_order`에 다음 열 추가(리소스: `sales`):

| 열 | 유형 | Null 허용 | 댓글 |
|---|---|---|---|
| `split_store_credit_amount` | 십진수(12,4) | 예 | 저장소 크레딧 비율 |
| `split_cash_amount` | 십진수(12,4) | 예 | 현금 반제 금액 |
| `split_cash_status` | varchar(32) | 예 | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | int 부호 없음 | 예 | 신용 청구서 엔티티 ID 저장 |
| `split_cash_invoice_id` | int 부호 없음 | 예 | 현금 송장 엔티티 ID |

이 열에 대한 `db_schema_whitelist.json`도 생성합니다.

#### &#x200B;2. 확장 특성(`etc/extension_attributes.xml`)

`split_store_credit_amount`(부동 소수점), `split_cash_amount`(부동 소수점), `split_cash_status`(문자열)을(를) 추가할 위치:
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. REST 끝점(`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

`Client\SplitPayment\Api\SplitPaymentManagementInterface`에 대한 세 가지 맵 모두.

#### &#x200B;4. I/O 이벤트(`etc/io_events.xml`)

`observer.sales_order_place_before`에 가입하고 필드 포함:
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — 세션 래퍼

선언된 분할 금액을 체크아웃 세션에 저장합니다. 다음을 노출해야 합니다.
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — `['store_credit' => float, 'cash' => float]` 반환
* `clear(): void`
* `beginBalanceApply(): void` — 스토어 크레딧 적용 중 총 수정 플러그인을 제외하는 플래그를 설정합니다.
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — REST 컨트롤러

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* 숫자 ID와 마스크된 견적 ID를 비교하여 카트가 현재 세션에 속하는지 확인합니다.
* `SplitPaymentSession`에 금액 저장
* 성공 시 `true`을(를) 반환합니다. 실패 시 일반 메시지와 함께 `LocalizedException`을(를) throw합니다.

**`markCashReceived(int $orderId): bool`**
* `entity_id`(으)로 주문 로드
* `split_cash_status === 'pending'`의 유효성을 검사합니다.
* 상태를 `received`(으)로 설정하고 상태를 `processing`(으)로 설정합니다.
* 기록 주석을 추가합니다. `"Cash payment of $X.XX received."`
* `SplitInvoiceService::createCashPortionInvoice($orderId)` 호출
* 현금 송장 증가 ID로 주석 추가
* 호출 `createShipmentIfPossible($orderId)` — 배송 가능한 항목이 있는 경우 `ShipOrder::execute()`을(를) 사용하여 발송을 만듭니다.
* `true`을(를) 반환합니다. 오류 발생 시 일반 `LocalizedException`을(를) throw합니다.

**`markCashDeclined(int $orderId): bool`**
* 주문 로드
* `split_cash_status === 'pending'`의 유효성을 검사합니다.
* `$order->canCancel()`의 유효성을 검사합니다.
* 상태를 `declined`(으)로 설정하고 주석을 추가합니다. `"Cash payment declined (simulated fraud check)."`
* 주문 저장
* `OrderManagement::cancel($orderId)` 호출
* `true`을(를) 반환합니다. 실패 시 일반 `LocalizedException`을(를) throw합니다.

#### &#x200B;7. `PlaceOrderPlugin` — `QuoteManagement`의 aroundPlaceOrder

가장 중요한 플러그인입니다. `aroundPlaceOrder` 실행:

1. 견적을 로드합니다. 활성화되지 않은 경우 `$proceed()`을(를) 즉시 호출하십시오.
2. `SplitPaymentSession::getAmounts()` 읽기
3. 견적에 `customer_balance_amount_used > 0`이(가) 있으면 `BalanceManagementInterface::remove($cartId)`을(를) 호출합니다(`LocalizedException` 핸들 — 일반 메시지로 기록하고 재throw).
4. 호출 `recollectQuoteForBalanceOperation()` — 인용 부호, 호출 `collectTotals()`을(를) 로드하고 저장합니다(`beginBalanceApply()`/`endBalanceApply()` 이내에 총 수정 플러그인을 표시하지 않음).
5. `$storeCredit > 0`이면 `BalanceManagementInterface::apply($cartId, $storeCredit)`을(를) 호출합니다(처리 실패).
6. 견적 다시 로드, 확장 특성 설정: `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'`
7. `payment->additional_information['client_split_payment']`을(를) `['store_credit' => x, 'cash' => y]`(으)로 저장
8. 견적 저장
9. `$proceed()` 호출, 주문 ID 캡처
10. `SplitPaymentSession::clear()` 호출
11. 반품 주문 ID

#### &#x200B;8. `CopySplitPaymentToOrder` — `sales_model_service_quote_submit_before`의 관찰자

세션 → 결제 추가 정보 → 견적 확장 속성(해당 우선순위 순서에서)에서 분할 금액을 읽습니다. 주문 개체에 `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'`을(를) 씁니다.

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — `sales_order_place_after`의 관찰자

주문 배치 후 주문에 스토어 크레딧 금액(`split_store_credit_amount > 0`)이 있는 경우 `SplitInvoiceService::createStoreCreditPortionInvoice($orderId)`을(를) 호출하여 스토어 크레딧 부분에 대해 즉시 송장을 발행하십시오.

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
스토어 신용 부분에 대해서만 송장을 생성합니다. 인보이스의 `customer_balance_amount`을(를) 스토어 크레딧 금액으로 설정합니다. 송장을 등록 및 저장합니다. 실패 시 송장 엔티티 ID 또는 null 반환(로그, 다시 던지지 않음).

**`createCashPortionInvoice(int $orderId): ?int`**
현금 부분(SC 송장이 적용되지 않는 나머지 주문 품목/금액)에 대한 송장을 생성합니다. 등록 및 저장. 실패 시 엔티티 ID 또는 null 반환

#### &#x200B;11. `CheckoutPlugin` — `PaymentInformationManagement`

`Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`의 플러그 인입니다. 계속하기 전에:
* 체크아웃 세션에서 견적 로드
* 구성된 임계값 가져오기: `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`, 기본값 `100`
* `$quote->getGrandTotal() > $threshold`이면 `LocalizedException('Payment could not be processed. Please try again or contact support.')` throw

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — 총 `Customerbalance`개

기본 고객 잔액 총 수집 실행 후 `customer_balance_amount_used`에서 `SplitPaymentSession::getAmounts()['store_credit']`(으)로 상한. 이렇게 하면 고객이 더 작은 매장 크레딧 부분을 선언한 경우 Commerce에서 전체 고객 잔액을 과도하게 적용할 수 없습니다.

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — `Quote\Address\Total\Grand`

총 수집 후: 분할 결제 세션이 있고 `isBalanceApplyInProgress()`이(가) false인 경우 견적 총계를 세션 현금 금액으로 설정하십시오. 이렇게 하면 체크아웃 UI에 현금으로만 표시됩니다.

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — `Sales\Model\Order\Invoice`

송장 합계가 수집된 후 송장의 연결된 주문에 `split_sc_invoice_id`이(가) 있는 경우 스토어 크레딧이 두 번 적용되지 않도록 송장의 `customer_balance_amount`을(를) 수정하십시오.

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — `Payment\Model\Checks\ZeroTotal`

견적 총계가 $0인 경우에도 `SplitPaymentSession::getAmounts()['cash'] > 0`에 COD 결제를 허용합니다(스토어 크레딧이 전체 주문을 포함하기 때문).

#### &#x200B;16. `LayoutProcessorPlugin` — `Checkout\Block\Checkout\LayoutProcessor`

레이아웃이 처리된 후:
* `components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`에 있는 `cashondelivery` 결제 방법 구성 요소의 `additional` 하위 항목에 `Client_SplitPayment/js/view/payment/split-payment` 구성 요소를 삽입하십시오.
* 결제 단계에서 기본 스토어 신용 UI 구성 요소(`customerBalance`, `useStoreCredit`)를 제거합니다. 분할 결제 구성 요소는 스토어 신용 표시/응용 프로그램을 소유합니다.

#### &#x200B;17. `OrderRepositoryPlugin` — `OrderRepositoryInterface`

`get()`과(와) `getList()` 후에 플랫 `sales_order` 열(`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`)에서 주문의 확장 특성을 하이드레이션합니다.

#### &#x200B;18. `AdminSplitPaymentTitlePlugin` — `Payment\Block\Info`

`getTitle()`이(가) 반환된 후 결제 방법이 `cashondelivery`이고 주문에 분할 결제가 있는 경우 제목에 `" (Split: Cash $X.XX + Store Credit $Y.YY)"`을(를) 추가합니다.

#### &#x200B;19. `OrderPaymentPlugin` — `Sales\Block\Adminhtml\Order\Payment`

`_beforeToHtml` 또는 afterToHtml을 통해 Commerce 관리 주문 보기에서 분할 결제 세부 사항(현금 금액, 스토어 신용 금액, 상태)을 결제 블록 HTML에 추가합니다.

#### &#x200B;20. Storefront Store 크레딧 잔고 컨트롤러

`Controller/Checkout/StoreCreditBalance.php` — 경로: `GET /splitpayment/checkout/storecreditbalance`

JSON 반환: `{"balance": float, "logged_in": bool}`. 고객이 로그인하지 않은 경우 `{"balance": 0, "logged_in": false}`을(를) 반환합니다. `Magento\CustomerBalance\Model\Balance`에서 잔액을 읽습니다.

`etc/frontend/routes.xml`에서 `frontName="splitpayment"`(으)로 프론트엔드 경로를 등록합니다.

#### &#x200B;21. 체크아웃 KnockoutJS 구성 요소 — `split-payment.js`

다음을 포함하는 `uiComponent`:
* `cashondelivery` 결제 방법이 선택된 경우(`quote.paymentMethod.subscribe`)를 검색합니다.
* `GET /splitpayment/checkout/storecreditbalance`을(를) 통해 고객의 스토어 크레딧 잔액을 로드합니다.
* 현금 금액 필드를 전체 주문 합계로 미리 채웁니다(`grand_total` 및 `customerbalance`을(를) 제외하고 `total_segments`에서 계산됨. `grand_total`은(는) 현금 잔액으로 설정될 수 있으므로 직접 사용하지 않음).
* 고객이 현금 금액 입력을 변경할 때: 필요한 상점 크레딧 계산 = 총 주문 − 현금; 유효성 검사; `POST /V1/split-payment/set`에 게시
* 다음에 대한 유효성 검사 메시지 표시: 현금 > 주문 총액, 부족한 스토어 크레딧, 로그인되지 않음
* 올바른 분할을 입력했을 때 성공 메시지를 표시합니다. `"The remaining $X.XX will automatically be applied from your store credit."`
* 다른 결제 방법을 선택하면 재설정됩니다(세션을 지우려면 `{storeCreditAmount: 0, cashAmount: 0}` 게시물).

#### 22. `cashondelivery-method.js`

`Magento_OfflinePayments/js/view/payment/offline-payments`을(를) 확장합니다. `payment-method-helper.js`을(를) 사용하여 Cash 메서드 코드를 검색합니다. `additional` 영역에 `split-payment` 구성 요소를 등록합니다.

#### 23. `payment-method-helper.js`

`getCashMethodCode()`을(를) 반환하는 유틸리티 — `cashondelivery`에 대해 `window.checkoutConfig.paymentMethods`을(를) 확인합니다. 필요한 경우 `checkmo`(으)로 돌아갑니다.

#### &#x200B;24. `cashondelivery.html` 템플릿

표준 COD 템플릿이지만 `<!-- ko foreach: getRegion('additional') -->` 영역을 포함하므로 분할 결제 하위 구성 요소를 렌더링할 수 있습니다.

#### &#x200B;25. `split-payment.html` 템플릿

분할 결제 필드에 대한 녹아웃JS 템플릿:
* 사용 가능한 스토어 신용 잔액 표시
* 현금 금액 입력(숫자, 단계 0.01)
* 크레딧 부분 표시 저장(읽기 전용)
* 스토어 크레딧 메시지 자동 적용(분할이 유효하고 스토어 크레딧이 0일 때 표시됨)
* 유효성 검사 오류 메시지

#### 26. `requirejs-config.js`

맵:
* 구성 요소→ `Client_SplitPayment/js/view/payment/split-payment`
* COD 재정의→ `Client_SplitPayment/js/view/payment/cashondelivery-method`
* `Client_SplitPayment/js/model/payment-method-helper` → 도우미

#### 27. `etc/config.xml`

기본 시스템 구성 값:

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### 중요 구현 정보

**스토어 신용 응용 프로그램에서 직접 모델 조작이 아닌 `BalanceManagementInterface`을(를) 사용해야 합니다.** `BalanceManagementInterface::apply()`이(가) 세션, 유효성 검사 및 장바구니 다시 계산을 올바르게 처리합니다.

**`PlaceOrderPlugin`은(는) `beforePlaceOrder`이(가) 아닌 `aroundPlaceOrder`을(를) 사용해야 합니다.** 장바구니가 활성 상태인 동안 저장소 크레딧을 적용해야 하며 `$proceed()`을(를) 호출하기 전에 이를 보장해야 합니다.

**`beginBalanceApply`/`endBalanceApply`에 대한 세션 플래그 패턴이 중요합니다.** `FixSplitPaymentGrandTotalPlugin`은(는) `collectTotals()` 동안 잔고 작업 내에서 실행되고 총계를 나머지 현금으로 설정하여 `BalanceManagementInterface::apply()`이(가) 실패하거나 크레딧을 제한합니다.

**내부 오류 세부 정보를 고객에게 표시하지 않습니다.** REST 응답으로 표시되는 모든 `catch` 블록은 `LocalizedException('Payment could not be processed. Please try again or contact support.')`을(를) throw해야 합니다.

**`entity_id`은(는) 숫자 데이터베이스 ID입니다.** App Builder의 REST 호출은 항상 `increment_id`이(가) 아닌 `entity_id`을(를) 사용합니다.

**`SplitInvoiceService`은(는) 오류를 전파하지 않고 오류를 포착하고 기록해야 합니다.** 송장 생성 실패는 이미 주문한 주문을 취소해서는 안 됩니다. 실패를 로그하여 관리자가 수동으로 처리하도록 하십시오.


### 파일 생성 후

Commerce 프로젝트 루트에서 다음 명령을 실행합니다.

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### 프롬프트 끝


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
