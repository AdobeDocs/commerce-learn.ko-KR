---
title: '분할 결제 POC: App Builder orchestrator AI 프롬프트'
description: 이 프롬프트를 사용하여 분할 결제-Orchestrator 앱을 빌드하는 방법을 알아봅니다. I/O 이벤트, 결제 Orchestrator, 웹 액션, 데모 대시보드 및 aio 앱 배포
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# 분할 결제 POC: App Builder orchestrator AI 프롬프트

**분할 결제-오케스트레이터** 프로젝트를 생성하는 전체 프롬프트, 즉 **결제-오케스트레이터** I/O 이벤트 소비자, **결제-수락** 및 **결제-거절** 웹 액션, **데모-대시보드** 및 Commerce REST 클라이언트를 복사할 수 있습니다.

## 이 프롬프트를 사용하는 방법

**PROMPT START**&#x200B;에서 **PROMPT END**&#x200B;까지의 모든 내용을 Cursor(Claude 포함)로 복사하거나 Claude로 직접 복사합니다. `split-payment-orchestrator/` 디렉터리(App Builder 프로젝트 루트)에서 프롬프트를 실행합니다.

## 실행하기 전에

* [분할 결제 POC: 필수 구성 요소 및 환경 설정](./prerequisites-and-setup.md)을 완료합니다.
* [분할 결제 POC: 환경 변수 참조](./env-reference.md) 및 `.env` 파일을 프로젝트에서 준비하십시오.


## 프롬프트

**프롬프트 시작**


Adobe Commerce에서 분할 지급을 조정하기 위해 전체 Adobe App Builder 애플리케이션을 생성하고 있습니다. 이 애플리케이션은 Commerce에서 I/O 이벤트를 수신하고 분할 지급 결정을 처리하며 REST를 통해 Commerce으로 다시 호출합니다.

**프로젝트:** `split-payment-orchestrator`
**런타임:** Node.js 18
**키 종속성:** `@adobe/aio-sdk ^6.0.0`, `got ^11.8.6`, `oauth-1.0a ^2.2.6`

아래 나열된 모든 파일을 생성합니다. 응용 프로그램은 Adobe I/O Runtime(`aio app deploy`)에서 작동해야 합니다.


### 생성할 파일 구조

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

`split_payment_orchestrator` 패키지에서 네 가지 작업을 정의합니다.

**`payment-orchestrator`**
* `web: "no"`(I/O 이벤트 트리거만 — HTTP를 통해 직접 호출할 수 없음)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 환경 입력: `LOG_LEVEL`, `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET`, `PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"`(대시보드 또는 ERP에서 호출할 수 있는 HTTP 웹 작업)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 동일한 Commerce 자격 증명 입력(`PAYMENT_THRESHOLD` 없음)

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* 동일한 Commerce 자격 증명 입력

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` ← 대시보드는 공개적으로 액세스할 수 있습니다(설정된 경우 `DEMO_UI_SECRET`에 의해서만 보호).
* 입력: 모든 Commerce 자격 증명 + `DEMO_UI_SECRET`, `DEMO_UI_BASE_URL`

**이벤트 등록**(`events.registrations` 아래):

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

Adobe Commerce용 OAuth 1.0a REST 클라이언트를 공유했습니다. 구현:

**`createCommerceClient(params, logger)`** — `{ request, baseUrl }` 반환

* `params`에서 `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET` 읽기
* 누락된 자격 증명이 있는 경우 throw됩니다.
* `HMAC-SHA256`과(와) 함께 `oauth-1.0a` 사용(Node.js `crypto.createHmac`)
* `prefixUrl = ${baseUrl}/rest/V1/`에서 `got@11`(`got@12+`이(가) 아님) 사용 - 프로젝트에서 CJS 사용
* `beforeRequest` 후크를 통해 `Authorization` 헤더 추가
* `request(method, path, options)` — 경로(`/` 앞에 있는 스트립)를 정규화하고 `{ statusCode, body }`을(를) 반환합니다.
* `throwHttpErrors: false` — 4xx/5xx에서 throw되지 않습니다. 항상 상태 코드를 반환합니다.


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — `{ pass: boolean, reason: string }` 반환

논리:
1. `params.PAYMENT_THRESHOLD`을(를) 읽고, float로 구문 분석합니다. 누락된 경우 기본적으로 `100`, NaN 또는 ≤ 0입니다.
2. `orderTotal > threshold`인 경우: `{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }` 반환
3. `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02`인 경우: `{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }` 반환
4. 그렇지 않으면 `{ pass: true, reason: '' }` 반환


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — `{ ok: boolean, error? }` 반환

* 다음으로 `POST orders/${orderId}/comments` 호출:

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* 2xx에 `{ ok: true }`을(를) 반환합니다. 그렇지 않으면 `{ ok: false, error: { code, message } }`을(를) 반환합니다.
* try/catch로 래핑, 오류 개체를 반환하고 throw하지 않음


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — `{ ok: boolean, error? }` 반환

* `success`: 기록 댓글을 게시하는 경우 `"Split payment orchestration completed. Order awaiting cash confirmation."`
* `!success`: 게시물 `"Payment could not be processed. Please try again or contact support."` — 고객이 볼 수 있는 댓글에 `detail`을(를) 포함하지 않음. 내부적으로만 `detail` 기록
* `{ ok: boolean, error? }`을(를) 반환합니다. throw하지 않습니다.


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** — 더 이상 사용되지 않는 no-op 구현

다음을 설명하는 JSDoc `@deprecated` 알림과 함께 이 파일을 포함합니다.
> 오케스트레이터는 더 이상 REST를 통해 스토어 크레딧을 적용하지 않습니다. 저장소 크레딧이 Commerce PHP 모듈(`PlaceOrderPlugin`)의 체크 아웃 시 `BalanceManagementInterface::apply()`을(를) 사용하여 적용되며, 이 경우 활성 장바구니가 필요합니다. App Builder이 I/O 이벤트를 수신할 때까지 장바구니는 비활성 상태입니다. 이 파일은 참조용으로 보관되거나 스토어 크레딧이 사후 적용되는 사용자 지정 플로우용으로 보관됩니다.

개발자가 패턴을 학습할 수 있도록 작동 중인 구현(다른 모듈과 동일한 모양)을 유지하지만 현재 흐름에서 사용되지 않는 것으로 명확하게 표시합니다.


### `actions/payment-orchestrator/index.js`

입출력 이벤트 진입점 `async function main(params)` 구현.

**페이로드 추출:**

Adobe Commerce I/O 이벤트는 페이로드를 여러 가지 모양으로 전달할 수 있습니다. 다음 경로를 순서대로 확인하여 순서 값 개체를 추출합니다.
1. `params.__ow_body`(필요한 경우 JSON 문자열 구문 분석) → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. `params` 자체로 폴백

**값에서 필드 추출:**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* `value.extension_attributes`에서 분할 금액(`extension_attributes` 및 `extensionAttributes` 모두 확인):
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**흐름:**
1. 값을 추출합니다. `orderId`이(가) 없으면 오류를 기록하고 `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`을(를) 반환합니다.
2. `evaluateThreshold(...)` 호출 — 실패하면 같은 200 응답을 로그하여 반환합니다.
3. `createCommerceClient(params, logger)` 호출 — 실패하면 200 오류를 반환합니다.
4. `storeCredit > 0`인 경우 체크아웃 시 해당 저장소 크레딧이 적용되었는지 기록합니다(REST 호출 필요 없음).
5. `recordCashPending(...)` 호출 — 실패하면 `updateOrderAfterOrchestration(..., success: false)`을(를) 호출하고 200 오류를 반환합니다.
6. `updateOrderAfterOrchestration(..., success: true)` 호출
7. `{ statusCode: 200, body: { ok: true, message: 'processed' } }` 반환

**중요:** 항상 `statusCode: 200`을(를) 반환합니다. I/O Runtime이 200개가 아닌 응답을 다시 시도하면 중복 주문 처리가 발생합니다. 본문에 오류가 보고됩니다.

**`PUBLIC_ERROR`상수:** `"Payment could not be processed. Please try again or contact support."` — 모든 외부 오류 메시지에 사용됩니다.


### `actions/payment-accept/index.js`

HTTP 웹 작업입니다. `POST /V1/split-payment/orders/:orderId/cash-received`을(를) 호출합니다.

**주문 ID 확인:** `params.orderId`, `params.payload.orderId`, `params.__ow_body`(구문 분석된 JSON)을 확인합니다. 누락된 경우 400을 반환합니다.

**흐름:**
1. `orderId` 해결, 누락된 경우 400 반환
2. 상거래 클라이언트 초기화, 실패 시 500 반환
3. 빈 JSON 본문으로 `POST split-payment/orders/${orderId}/cash-received` 호출
4. 2xx인 경우: `{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }` 반환
5. 오류인 경우: 로그하여 `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }` 반환


### `actions/payment-decline/index.js`

HTTP 웹 작업입니다. `payment-accept`과(와) 같은 패턴이지만 `POST /V1/split-payment/orders/:orderId/cash-decline`을(를) 호출합니다.

성공하면 `{ ok: true, orderId, message: 'declined' }`을(를) 반환합니다.


### `actions/demo-dashboard/index.js`

자체 포함된 데모 연산자 대시보드. 보류 중인 현금 주문을 나열하고 수락/거절 작업을 트리거하기 위한 HTML 대시보드를 제공합니다. HTML UI와 JSON API를 모두 제공하는 단일 웹 작업입니다.

**보안:**
* 선택적 `DEMO_UI_SECRET` 확인: 설정된 경우 모든 요청에 `?secret=<value>` 쿼리 매개 변수 또는 `x-demo-secret` 헤더가 필요합니다. 누락/잘못된 경우 401을 반환합니다.
* `DEMO_UI_SECRET`이(가) 설정되지 않은 경우 경고 기록(대시보드가 보호되지 않음)

**라우팅(HTTP 메서드 + 경로/본문 기반):**

이 작업은 모든 요청을 수신합니다. 다음에서 의도 결정:
* `params.__ow_method`(GET/POST) 및 `params.__ow_path` 또는 작업 매개 변수
* HTML 대시보드를 제공하는 작업이 → `GET`
* `action=list` 매개 변수가 있는 `GET`이(가) 보류 중인 주문의 JSON 목록을 반환할 → 있습니다.
* `action=accept` 및 `orderId`이(가) 있는 `POST`에서 `payment-accept` 논리를 →(또는 Commerce REST 호출을 인라인으로)
* `action=decline` 및 `orderId`을(를) 사용하는 `POST`에서 `payment-decline` 논리를 →.

**보류 중인 주문 가져오기:**
* Commerce REST에서 `GET orders?searchCriteria[pageSize]=50` 호출
* `extension_attributes.split_cash_status === 'pending'` 및 순서가 터미널 상태가 아닌 순서로 필터링합니다(`complete`, `closed`, `canceled`, `cancelled`).
* 메모리에서 가장 최근 항목 먼저 정렬
* 최대 20개 반환(`limit` 매개 변수를 통해 구성 가능)

**HTML 대시보드:**
대시보드는 `Content-Type: text/html`(으)로 반환된 HTML 문자열입니다. 이는 다음과 같아야 합니다.
* 보류 중인 현금 주문을 테이블에 나열합니다. 주문 번호(increment_id), entity_id, 고객명, 현금 금액, 상점 대변 금액, 일자
* 작업의 자체 API 끝점을 호출하는 각 순서에 대해 **Accept** 및 **Decline** 단추를 제공합니다.
* 인라인 성공/오류 응답 표시
* 새로 고침 단추 포함
* 사용할 수 있을 만큼 스타일링되어야 합니다(최소 CSS는 괜찮습니다. 런타임에 CORS 문제를 방지하기 위해 외부 CDN 종속성이 없음).
* `DEMO_UI_SECRET`이(가) 설정되지 않은 경우 경고 배너 표시

**UI에 표시되는 오류:**
Commerce REST 호출이 실패하면 HTTP 상태와 Commerce 오류 본문에 대한 간단한 설명을 포함합니다(sanitize — HTML 태그를 제거하고, 500자로 자름). 자격 증명을 노출하지 마십시오.

**응답 도우미:**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### Deploy 명령

`split-payment-orchestrator/` 디렉터리에서 모든 파일을 생성한 후:

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

배포 후 `aio app deploy`이(가) 인쇄한 작업 URL을 확인합니다. `demo-dashboard` URL에서 연산자 대시보드에 액세스합니다.


### 프롬프트 끝


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
