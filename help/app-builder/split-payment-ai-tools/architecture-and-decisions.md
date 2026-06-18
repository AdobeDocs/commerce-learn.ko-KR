---
title: 분할 결제 POC — 아키텍처 및 설계 결정
description: 분할 결제 POC가 Commerce에 동기화 체크아웃을 매핑하고 I/O 기반 단계를 App Builder에 동기화하는 방법과 확장 속성, REST 및 5개의 플러그인 에지 사례를 알아봅니다.
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# 분할 결제 POC: 아키텍처 및 디자인 결정

이 페이지에서는 분할 지급 개념 증명 이면의 아키텍처 선택 사항에 대해 설명합니다. 이 시리즈에서 빌드 프롬프트를 사용하기 전에 각 구성 요소가 구성되는 방식과 자체 프로젝트에서 패턴을 조정하는 방법을 이해할 수 있습니다.

## 핵심 원칙

개념증명은 가장 우아한 분할지급 이행에 관한 것이 아니다. 빅뱅 다시 작성 없이 Commerce 논리를 App Builder으로 이동하는 방법&#x200B;**을**&#x200B;보여주는 것입니다.

전체에 적용되는 규칙은 다음과 같습니다.

> **Commerce 요청 주기에서 동기적으로 실행해야 하거나, 깨끗한 외부 표면이 없는 Commerce 내부 API를 호출해야 하는 경우 PHP로 유지됩니다. 다른 모든 항목은 App Builder으로 이동합니다.**

## Commerce(PHP)에서 생활하는 내용과 그 이유

### &#x200B;1. 크레딧 응용 프로그램 저장: `PlaceOrderPlugin`

스토어 크레딧이 `Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`을(를) 사용하여 장바구니에 적용됩니다. 이 메서드는 **active** 장바구니에서만 작동합니다. 주문이 진행되는 순간 장바구니가 비활성화됩니다. App Builder은 주문이 실행된 *후*&#x200B;에 I/O 이벤트를 수신하므로 App Builder에서 스토어 크레딧을 적용할 수 없습니다.

**단원:** 주문 배치 전에 장바구니 상태를 변경해야 하는 모든 항목은 Commerce에서 실행해야 합니다. 해결 방법이 없습니다.

### &#x200B;2. 동기 임계값 보호기: `CheckoutPlugin`

$100 주문 임계값 검사는 고객이 **[!UICONTROL Place Order]**&#x200B;을(를) 선택하기 전에 결제 단계에서 고객을 차단해야 합니다. Commerce 요청 주기에서 응답이 동기화되어야 합니다. App Builder은 이벤트 기반이며 비동기이므로 해당 순간에 즉각적인 오류를 반환할 수 없습니다.

App Builder *also*&#x200B;은(는) 감사로서 임계값의 유효성을 검사하지만, 고객 경험은 먼저 실행되는 Commerce 검사에 따라 달라집니다.

### &#x200B;3. 사용자 지정 REST 끝점: `webapi.xml` 및 `SplitPaymentManagement`

다음 끝점은 다음과 같아야 합니다.

* `SplitInvoiceService`(Commerce 내부 송장 서비스를 사용하는 송장) 호출
* `ShipOrder::execute()` 호출(Commerce 내부 배송 서비스)
* Commerce의 주문 상태 컴퓨터로 주문 상태 및 상태 업데이트

`/V1/split-payment/orders/:id/cash-received` 및 `/V1/split-payment/orders/:id/cash-decline`

해당 비헤이비어에 대한 깨끗한 공개 REST 레이어는 없으므로 Commerce은 끝점을 노출합니다. App Builder이 호출합니다.

### &#x200B;4. 견적 및 주문에 대한 금액 분할: 관찰자 및 플러그인

Commerce은 App Builder이 읽는 REST 응답과 **[!UICONTROL Admin]** 주문 보기에 대해 모두 주문에서 분할 금액(`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`)이 필요합니다. 금액은 확장 특성과 함께 첨부되며 견적의 주문에서 `sales_model_service_quote_submit_before`의 관찰자의 주문으로 복사됩니다.

## App Builder에 서식하는 이유

### &#x200B;1. 이벤트 기반 순서 처리: `payment-orchestrator`

`sales_order_place_before`이(가) 실행되면 App Builder에서 이벤트를 수신합니다. 임계값의 유효성을 다시 검사하고(감사로서) 주문에 대해 *현금 보류 중* 댓글을 기록하고 주문 상태를 업데이트합니다. 그것들 중 어떤 것도 새로운 PHP를 필요로 하지 않으며, Commerce에 다시 REST만 필요합니다.

### &#x200B;2. 현금 수락: `payment-accept`

ERP(또는 대시보드의 연산자)에서 현금을 받았음을 확인하면 `payment-accept`에서 `POST /V1/split-payment/orders/:id/cash-received`을(를) 호출합니다. 송장, 선적 및 주문 상태는 Commerce에서 처리됩니다. App Builder이 트리거입니다.

### &#x200B;3. 현금 감소: `payment-decline`

`payment-decline`에서 `POST /V1/split-payment/orders/:id/cash-decline`을(를) 호출하면 Commerce에서 주문을 취소합니다. 현금 수락과 동일한 패턴.

### &#x200B;4. 연산자 대시보드: `demo-dashboard`

App Builder 웹 작업에서 제공되는 자체 포함된 HTML 대시보드. Commerce REST에서 현금 대기 중인 주문을 가져오고 위의 App Builder 작업을 호출하는 **[!UICONTROL Accept]**/**[!UICONTROL Decline]** 작업을 제공합니다. Commerce **[!UICONTROL Admin]**&#x200B;은(는) 필요하지 않습니다.

## 임계값: 의도적으로 두 번 적용됨

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce은 사용자 대면 가드를 소유하고, App Builder은 배치 후 감사를 소유합니다.** 그것은 의도적인 것이다.

## 상점 크레딧: PHP에 머무르는 이유

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

Orchestrator의 `store-credit.js` 파일은 이를 문서화합니다. 사용되지 않는 이유를 설명하는 주석이 포함된 no-op 스텁입니다.

## 확장 속성: 접착제

분할 금액은 확장 속성에 대해 시스템을 통해 이동합니다.

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## 데이터 모델

이 모듈에서 추가하는 플랫 열 **`sales_order`개**

| 열 | 유형 | 목적 |
| --- | --- | --- |
| `split_store_credit_amount` | 부동 | 적용된 스토어 크레딧 |
| `split_cash_amount` | 부동 | 현금 미수금 |
| `split_cash_status` | varchar | `pending`, `received` 또는 `declined` |
| `split_sc_invoice_id` | int | 스토어 신용 송장의 엔티티 ID |
| `split_cash_invoice_id` | int | 현금 송장의 엔티티 ID |

**확장 특성**(`CartInterface`, `OrderInterface` 및 `OrderPaymentInterface`에 있음)

* `split_store_credit_amount`(부동 소수점)
* `split_cash_amount`(부동 소수점)
* `split_cash_status`(문자열)

## I/O 이벤트 페이로드 필드

`observer.sales_order_place_before`이(가) 이벤트에 다음 항목을 포함하도록 `io_events.xml`에 구성되어 있습니다.

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder에서는 임계값 유효성 검사를 위해 `entity_id`을(를) 주문 ID로 사용하고 `split_store_credit_amount` 및 `split_cash_amount`을(를) 사용합니다.

## 개념 증명이 다루는 다섯 가지 핵심 사례

### 1. `CapCustomerBalanceCollectPlugin`

Commerce의 기본 **[!UICONTROL Customer balance]** 합계 수집기는 초과 적용할 수 있습니다(세션에서 선언한 분할 금액이 아니라 사용 가능한 전체 잔액을 볼 수 있음). 이 플러그인은 금액을 세션에서 선언된 값으로 제한합니다.

### 2. `FixSplitPaymentGrandTotalPlugin`

스토어 크레딧이 적용되면 **[!UICONTROL Grand Total]** 견적이 현금 전용 금액으로 떨어질 수 있습니다. 체크아웃 JavaScript은 변경되는 분할 유효성 검사 *before*&#x200B;에 대한 주문 합계를 계산해야 합니다. 플러그인은 합계 수집 후에 실행되며 표시를 수정하는 반면, JavaScript은 `grand_total`만 신뢰하지 않고 소계 세그먼트에서 값을 다시 구성합니다.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

송장 합계가 회수되면 스토어 크레딧을 두 번 적용할 수 있습니다. 이 플러그인은 인보이스의 `customer_balance_amount`을(를) 수정합니다.

### 4. `SplitPaymentZeroTotalPlugin`

스토어 크레딧이 적용된 후 장바구니 **[!UICONTROL Grand Total]**&#x200B;은(는) $0(전체 스토어 크레딧 주문)일 수 있습니다. 이 경우 Commerce의 **[!UICONTROL Zero subtotal checkout]** 검사가 COD를 차단할 수 있습니다. 이 플러그인은 세션 현금 금액이 0보다 클 때 COD를 허용합니다.

### &#x200B;5. `BalanceManagementInterface::apply()` 이전 견적 회수

`apply()`이(가) 현재 **[!UICONTROL Grand Total]**&#x200B;에 대해 금액을 확인합니다. 합계가 이미 현금 부분인 경우 `apply()`이(가) 실패하거나 제한될 수 있습니다. `PlaceOrderPlugin`은(는) 세션 플래그(`beginBalanceApply` / `endBalanceApply`)를 사용하여 균형을 적용하는 동안 총계 수정 작업을 일시적으로 중단합니다.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
