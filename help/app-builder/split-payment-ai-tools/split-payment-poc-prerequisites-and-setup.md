---
title: '분할 결제 POC: 사전 요구 사항 및 환경 설정'
description: 분할 결제 빌드를 묻기 전에 Commerce, COD 관리 및 크레딧 저장, OAuth 통합, I/O 이벤트, App Builder 및 aio CLI를 설정하는 방법에 대해 알아봅니다.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# 분할 결제 POC: 사전 요구 사항 및 환경 설정

빌드 프롬프트를 실행하기 전에 이 자습서의 모든 단계를 완료합니다. 단일 단계가 누락된 것은 흐름이 튜토리얼 중간을 중단하는 가장 일반적인 이유입니다.

## &#x200B;1. Adobe Commerce 요구 사항

* Adobe Commerce **2.4.5 이상**(온-프레미스 또는 Commerce Cloud)
* Commerce 프로젝트에 대한 Git 액세스(`app/code/` 아래에 모듈 추가)
* **[!UICONTROL Commerce Admin]**&#x200B;에 액세스

### Commerce Cloud만 해당: I/O 이벤트 활성화

모듈을 추가하기 전에 `.magento.env.yaml`에 다음을 추가하고 배포합니다.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **경고:** I/O 이벤트 모듈 종속성을 해결하려면 이 배포를 완료해야 합니다.


## &#x200B;2. Commerce 관리 구성

이 단계를 먼저 수행하십시오. 체크아웃 JavaScript은 정확한 문자열 일치 항목에 따라 다릅니다.

### 2a. 정확한 제목으로 배달 현금 사용

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]**: **예**
* **[!UICONTROL Title]**: **`Cash`**(이 정확한 문자열은 체크아웃 JavaScript과 일치함)

> **참고:** 스토어에서 다른 COD(Cash-on-Delivery) 구현이나 제목을 사용하는 경우 Commerce 모듈에서 `payment-method-helper.js`을(를) 조정하십시오.

### 2b. 스토어 크레딧 활성화

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]**: **예**

### 2c. 테스트 고객에게 스토어 크레딧 추가

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[테스트 고객]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

스토어 크레딧에 **$50** 이상을 추가하십시오. 총 $100 미만의 주문으로 테스트합니다.

### 2d. Commerce 통합 만들기

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]**: `Split Payment App Builder`(또는 원하는 이름)
* **[!UICONTROL API]** 탭에서 최소한 다음 권한 부여:
   * `Magento_Sales::actions`(`cash-received` 끝점에 필요)
   * `Magento_Sales::cancel`(`cash-decline` 끝점에 필요)
   * 견적 또는 장바구니 관리(전체 또는 관련 하위 집합)
   * **[!UICONTROL Customer balance]**(전체 또는 관련 하위 집합)

**[!UICONTROL Save]**&#x200B;을(를) 선택한 다음 **[!UICONTROL Activate]**&#x200B;을(를) 선택합니다.

**네 개의 값을 모두 복사하십시오. 한 번만 표시됩니다.**

| 값 | 환경 변수 |
| --- | --- |
| 소비자 키 | `COMMERCE_CONSUMER_KEY` |
| 소비자 암호 | `COMMERCE_CONSUMER_SECRET` |
| 액세스 토큰 | `COMMERCE_ACCESS_TOKEN` |
| 액세스 토큰 암호 | `COMMERCE_ACCESS_TOKEN_SECRET` |

이러한 값을 안전하게 저장하십시오. 모든 App Builder `.env` 파일에 필요합니다.


## &#x200B;3. Adobe Developer Console 및 App Builder

* Adobe Developer Console 조직에 대한 액세스
* **App Builder 프로젝트**(새 프로젝트 또는 다시 사용하는 프로젝트)
* A workspace configured; the prompts assume **[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]** added as a service to the workspace
* **Commerce** connected as an event provider for the workspace

The event code used in the proof of concept is: `com.adobe.commerce.observer.sales_order_place_before`

If you have not connected Commerce as an event provider, see [Configure Commerce to emit events to Adobe I/O](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"} in the Commerce Extensibility guide.


## 4. Local development environment

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## 5. Two project directories to know

This tutorial uses two separate directory roots. Keep them separate.

**Commerce project root** (your Magento git repository):

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder project root** (the `split-payment-orchestrator` folder from the source package, or a new project you create):

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## 6. entity_id compared to increment_id

> **Always use `entity_id` (the numeric database ID), not `increment_id` (for example `000000042`), in REST calls.**

Find `entity_id` from the **[!UICONTROL Commerce Admin]** order URL:

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

Or from the simulation script:

```bash
node scripts/simulate-split-payment.mjs show 42
```


## 7. The $100 threshold

The split payment UI and threshold guard target orders **$100.00 or less**. The flow behaves as follows:

* **At or below $100:** the split payment UI appears; the customer can set a cash plus store credit split
* **Above $100:** `CheckoutPlugin` throws an error at the payment step

To test, build a cart whose subtotal, shipping, and tax are **less than or equal to $100** (for example, a product under $90 so shipping and tax still fit under the cap).

The threshold is stored in:

* Commerce config: `split_payment/general/threshold` (default `100` in `etc/config.xml`)
* App Builder environment: `PAYMENT_THRESHOLD=100` (must match Commerce)


## 8. Fastly (Commerce Cloud only)

App Builder actions call Commerce over REST (`/rest/V1/split-payment/orders/...`). If your Commerce Cloud project uses Fastly with IP allowlisting, the App Builder runtime egress IP addresses must be allowlisted.

**How to check:** Run the simulation script first (direct curl with OAuth signing). If that works but the App Builder action returns `403`, Fastly is likely blocking the request.

Use Adobe’s current documentation for App Builder egress IP ranges and add them to your Fastly configuration as needed.


## Verification checklist

Before you start the build prompts, confirm the following:

* [ ] Commerce version is 2.4.5 or later
* [ ] I/O eventing is enabled (Commerce Cloud) and deployed
* [ ] Cash on delivery is enabled with the title set exactly to `Cash`
* [ ] Store credit is enabled
* [ ] The test customer has at least $50 store credit
* [ ] The Commerce integration is created and activated, and all four OAuth values are saved
* [ ] The App Builder project has the I/O Events service and the Commerce event provider configured
* [ ] `aio login` is complete and the correct workspace is selected with `aio app use`
* [ ] Node.js 18 or later is installed and the `aio` CLI is installed
* [ ] `.env` files are prepared per [Split payment POC: environment variables reference](split-payment-poc-env-reference.md) (and your source package, if you use one)


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
