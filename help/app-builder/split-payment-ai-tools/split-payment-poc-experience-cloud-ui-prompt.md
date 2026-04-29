---
title: '분할 결제 POC: Experience Cloud UI 확장 AI 프롬프트'
description: 관리자 UI SDK, IMS, OAuth, 수락 및 거부와 시뮬레이션 스크립트와 같은 선택적 프롬프트를 사용하여 Commerce 관리자에 분할 결제를 임베드하는 방법을 알아봅니다.
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 7ea8492b082fb3f6e9ed7794526b0f83cb0481b3
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# 분할 결제 POC: Experience Cloud UI 확장 AI 프롬프트

`commerce-checkout-starter-kit` 및 `commerce-backend-ui-1` 패턴을 사용하여 **[!UICONTROL Adobe Commerce]** 관리 셸(Experience Cloud)에 분할 결제 주문 패널을 임베드하는 선택적 단계입니다. App Builder orchestrator의 독립 실행형 [데모 대시보드](split-payment-poc-app-builder-orchestrator-prompt.md)는 Admin Shell 통합 없이 동일한 수락 및 거절 흐름을 다룹니다.

## 이 프롬프트를 사용하는 방법

**PROMPT START**&#x200B;에서 **PROMPT END**&#x200B;까지의 모든 내용을 Cursor 또는 Claude에 복사합니다. `commerce-checkout-starter-kit/` 디렉터리에서 프롬프트를 실행합니다.

## 실행하기 전에

* 이 경로에는 OAuth 값 외에 **IMS** 자격 증명이 필요합니다(`commerce-checkout-starter-kit` 변수의 경우 [분할 결제 POC: 환경 변수 참조](split-payment-poc-env-reference.md)).
* 동일한 `payment-accept` 및 `payment-decline` 동작을 비교하려면 먼저 [분할 결제 POC: App Builder orchestrator AI 프롬프트](split-payment-poc-app-builder-orchestrator-prompt.md)를 완료하십시오. UI 확장은 해당 논리를 `COMMERCE_INTEGRATION_*` 환경 이름으로 다시 사용합니다.


## 프롬프트

**프롬프트 시작**


분할 결제 PoC에 대한 `commerce-backend-ui-1` 관리 UI SDK 확장을 생성하고 있습니다. 이 확장은 Commerce 체크아웃 스타터 키트의 `@adobe/aio-app-dev-toolkit` 및 `@adobe/commerce-backend-ui-1` 패턴을 사용하여 분할 결제 주문 대시보드를 Adobe Commerce 관리 셸에 포함합니다.

**기본 프로젝트:** `commerce-checkout-starter-kit/`
**확장 디렉터리:** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### 이 확장에서 제공하는 사항

1. **관리 순서 격자 패널** — `split_cash_status = 'pending'`과(와) 함께 분할 결제 주문을 나열하는 Commerce 관리 셸의 사용자 지정 메뉴 항목
2. **주문 세부 사항 보기** - 주문과 함께 분할 결제 분류(현금 금액, 스토어 신용 금액, 상태)를 표시합니다.
3. **동작 수락/거부** - OAuth 1.0a를 통해 `payment-accept` 및 `payment-decline` App Builder 동작을 호출하는 단추


### 생성할 파일 구조

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### 백엔드 작업

**`actions/commerce/index.js`** — IMS 인증 Commerce REST
* 관리자 UI SDK 컨텍스트에서 제공한 IMS 토큰을 사용하여 Commerce REST를 호출합니다
* `split_cash_status` 필터를 사용하여 순서 목록을 가져옵니다.
* 주문 목록을 JSON으로 반환

**`actions/payment-accept/commerce-client.js`** — OAuth 1.0a 클라이언트
* `split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`과(와) 동일한 구현
* `COMMERCE_INTEGRATION_*` 접두사가 있는 env 변수를 사용합니다(IMS 자격 증명과 구분).

**`actions/payment-accept/index.js`** — 동작 수락
* `split-payment-orchestrator/actions/payment-accept/index.js`과(와) 동일한 논리
* OAuth 1.0a를 통해 `POST /V1/split-payment/orders/:orderId/cash-received` 호출

**`actions/payment-decline/index.js`** — 작업 거부
* `POST /V1/split-payment/orders/:orderId/cash-decline` 호출

**`actions/registration/index.js`** — 관리자 UI SDK 등록
* Commerce 관리 셸에 확장을 등록합니다.
* 분할 결제 대시보드의 주문 아래에 메뉴 항목 추가


### React 프론트엔드 구성 요소

**`SplitPaymentDashboard.jsx`**
* 스펙트럼 스타일의 테이블에 보류 중인 분할 지급 주문 나열
* 열: 주문 번호(increment_id), 일자, 고객, 현금 만기, 상점 대변, 상태
* 행당 수락 및 거절 단추
* `web-src/src/utils.js` 가져오기 도우미를 통해 백 엔드 작업을 호출합니다.
* 로드/오류 상태 표시, 작업 완료 시 새로 고침

**`SplitPaymentOrderDetail.jsx`**
* 단일 주문에 대한 분할 결제 세부 정보 표시
* 표시: 현금 금액, 상점 대변 금액, 현재 split_cash_status

**`useSplitPaymentOrders.js`** — React 후크
* `actions/commerce/index.js`에서 분할 결제 주문 가져오기
* `{ orders, loading, error, refresh }` 반환


### 시뮬레이션 스크립트

**`scripts/simulate-split-payment.mjs`**

App Builder을 거치지 않고 Commerce REST 호출을 직접 테스트하기 위한 Node.js ESM 스크립트. App Builder 작업과 동일한 OAuth 1.0a 서명을 사용합니다.

명령:
* `node simulate-split-payment.mjs help` — 사용량 표시
* `node simulate-split-payment.mjs list` — 분할 결제 데이터를 사용하여 최근 주문 나열
* `node simulate-split-payment.mjs show <orderId>` — 특정 주문에 대한 분할 결제 필드 표시(entity_id)
* `node simulate-split-payment.mjs accept <orderId>` — `cash-received` 끝점 호출
* `node simulate-split-payment.mjs decline <orderId>` — `cash-decline` 끝점 호출

`commerce-backend-ui-1/.env.simulation`에서 자격 증명을 읽습니다(`.env.simulation.example`에서 복사).

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

확장을 구성할 대상:
* `commerce-backend-ui-1` 확장 유형
* 네 가지 백엔드 작업(`commerce`, `payment-accept`, `payment-decline`, `registration`)
* `registration`을(를) 제외한 모든 작업에 대한 `require-adobe-auth: true`
* `COMMERCE_INTEGRATION_*` 자격 증명에 대한 환경 바인딩 입력


### 파일 생성 후

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### 프롬프트 끝


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
