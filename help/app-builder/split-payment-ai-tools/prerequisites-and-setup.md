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
source-git-commit: d5f1e76c3a5127698f2933810fca218b79082571
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
* 작업 영역이 구성되었습니다. 프롬프트는 **[!UICONTROL Stage]**&#x200B;을(를) 가정합니다.
* **[!UICONTROL Adobe I/O Events]**&#x200B;이(가) 작업 영역에 서비스로 추가됨
* 작업 영역의 이벤트 공급자로 연결된 **Commerce**

개념 증명에 사용된 이벤트 코드는 `com.adobe.commerce.observer.sales_order_place_before`입니다.

Commerce을 이벤트 공급자로 연결하지 않은 경우 Commerce 확장성 안내서에서 [이벤트를 Adobe I/O에 내보내도록 Commerce 구성](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"}을 참조하십시오.


## &#x200B;4. 로컬 개발 환경

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


## &#x200B;5. 알아 두어야 할 두 개의 프로젝트 디렉토리

이 자습서에서는 두 개의 별도 디렉터리 루트를 사용합니다. 따로 보관하세요.

**Commerce 프로젝트 루트**(Magento git 저장소):

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**App Builder 프로젝트 루트**(원본 패키지의 `split-payment-orchestrator` 폴더 또는 사용자가 만든 새 프로젝트):

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id와 increment_id 비교

> **REST 호출에서 항상 `increment_id`(예: `000000042`)이 아닌 `entity_id`(숫자 데이터베이스 ID)을(를) 사용하십시오.**

**[!UICONTROL Commerce Admin]** 주문 URL에서 `entity_id` 찾기:

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

또는 시뮬레이션 스크립트에서 다음을 수행합니다.

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. $100 임계값

분할 결제 UI 및 임계값 보호 대상 주문 **$100.00 이하**. 흐름은 다음과 같이 작동합니다.

* **$100 이하:**&#x200B;에서 분할 결제 UI가 나타납니다. 고객은 현금+상점 크레딧 분할을 설정할 수 있습니다
* **$100 초과:** `CheckoutPlugin`에서 결제 단계에서 오류가 발생합니다.

테스트하려면 소계, 배송 및 세금이 **1$100**&#x200B;보다 작거나 같은 장바구니를 만듭니다(예: $90 미만이므로 배송과 세금은 여전히 상한선에 맞음).

임계값은 다음 위치에 저장됩니다.

* Commerce 구성: `split_payment/general/threshold`(`etc/config.xml`의 기본 `100`)
* App Builder 환경: `PAYMENT_THRESHOLD=100`(Commerce과 일치해야 함)


## &#x200B;8. Fastly(Commerce Cloud만 해당)

App Builder 작업에서 REST(`/rest/V1/split-payment/orders/...`)를 통해 Commerce을 호출합니다. Commerce Cloud 프로젝트에서 IP 허용 목록에 추가으로 Fastly를 사용하는 경우 App Builder 런타임 이그레스 IP 주소는 허용 목록에추가된이어야 합니다.

**확인하는 방법:** 먼저 시뮬레이션 스크립트를 실행합니다(OAuth 서명을 사용하여 직접 curl). 이것이 작동하지만 App Builder 작업이 `403`을(를) 반환하는 경우 Fastly가 요청을 차단할 수 있습니다.

App Builder 이그레스 IP 범위에 대한 Adobe의 현재 설명서를 사용하고 필요에 따라 Fastly 구성에 추가합니다.


## 확인 검사 목록

빌드 프롬프트를 시작하기 전에 다음을 확인하십시오.

* [ ] Commerce 버전은 2.4.5 이상입니다.
* [ ] I/O 이벤트가 사용(Commerce Cloud)되고 배포되었습니다.
* [ `Cash`(으)로 정확히 설정된 제목을 사용하여 ] 배달 현금을 사용할 수 있습니다.
* [ ] 스토어 크레딧이 활성화되었습니다.
* [ ] 테스트 고객에게 $50 이상의 스토어 크레딧이 있습니다.
* [ ] Commerce 통합이 생성 및 활성화되고 4개의 OAuth 값이 모두 저장됩니다
* [ ] App Builder 프로젝트에 I/O 이벤트 서비스와 Commerce 이벤트 공급자가 구성되어 있습니다.
* [ ] `aio login`이(가) 완료되었으며 `aio app use`과(와) 함께 올바른 작업 영역을 선택했습니다.
* [ ] Node.js 18 이상이 설치되어 있고 `aio` CLI가 설치되어 있습니다.
* [ [분할 결제 POC: 환경 변수 참조](./env-reference.md)에 따라 ] `.env`개의 파일이 준비되었습니다(소스 패키지를 사용하는 경우).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
