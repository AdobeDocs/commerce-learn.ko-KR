---
title: '분할 결제 POC: 환경 변수 참조'
description: Commerce OAuth, 기본 URL, 결제 임계값 및 선택적 데모 설정을 Orchestrator, UI 확장 프로그램 및 시뮬레이션 환경 파일에 매핑하는 방법을 알아봅니다.
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 분할 결제 POC: 환경 변수 참조

모든 구성 요소에서 동일한 4개의 Commerce OAuth 자격 증명이 사용됩니다. **[!UICONTROL Commerce Admin]**&#x200B;에서 하나의 **[!UICONTROL Integration]**&#x200B;을(를) 만든 다음 아래 `.env`개 파일마다 네 개의 값을 다시 사용합니다. 활성화 단계는 [분할 결제 POC: 사전 요구 사항 및 환경 설정](split-payment-poc-prerequisites-and-setup.md)을 참조하십시오.

## 4개의 OAuth 자격 증명(모든 곳에서 사용됨)

| 변수 | 어디에서 구입할 수 있습니까 |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[통합]* |
| `COMMERCE_CONSUMER_SECRET` | 위와 동일 - 활성화 시에만 값이 표시됩니다. |
| `COMMERCE_ACCESS_TOKEN` | 위와 동일 |
| `COMMERCE_ACCESS_TOKEN_SECRET` | 위와 동일 |


## App Builder orchestrator

### `split-payment-orchestrator/.env`

Orchestrator 디렉터리의 `.env.example`에서 복사하십시오. 이 파일을 커밋하지 마십시오.

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Experience Cloud UI 확장(commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

이 구성 요소는 **[!UICONTROL Admin]** UI SDK이 포함된 주문 목록에 대해 IMS와 수락 및 거절 작업에 대해 OAuth 1.0a의 두 가지 자격 증명 집합을 사용합니다.

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## 시뮬레이션 스크립트

### `commerce-backend-ui-1/.env.simulation`

같은 디렉터리에 있는 `.env.simulation.example`에서 복사하십시오.

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## 메모

**`PAYMENT_THRESHOLD`** — **[!UICONTROL Commerce]** 시스템 구성의 `split_payment/general/threshold`과(와) 일치해야 합니다. 값이 없거나 숫자가 아니거나 `0`보다 작거나 같은 경우 양쪽의 기본값은 `100`입니다. **[!UICONTROL Commerce]**&#x200B;에서 임계값을 변경하는 경우 일치하도록 App Builder `.env`을(를) 업데이트하십시오.

**`DEMO_UI_SECRET`** — 선택 사항이지만 localhost가 아닌 배포에 권장됩니다. 대시보드 URL이 있는 모든 사용자는 주문을 나열하고 비어 있는 경우 수락 및 거부를 실행할 수 있습니다. 실제 스테이징 환경의 경우 공유 암호를 설정합니다.

**`COMMERCE_BASE_URL`** — 후행 슬래시를 포함하지 않습니다. Commerce REST 클라이언트가 `/rest/V1/`을(를) 자동으로 추가합니다.

**`AIO_CLI_ENV`** — **[!UICONTROL Stage]** 작업 영역에 대해 `stage`(으)로 설정합니다. **[!UICONTROL Production]**&#x200B;에 배포할 때 `prod`(으)로 변경합니다.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
