---
title: Adobe Commerce 개발자 에이전트 App Builder dry run
description: 이 실습형 App Builder dry run에서 Adobe Commerce 개발자 에이전트를 사용하여 3개의 Commerce 확장성 사용 사례를 구축, 배포 및 테스트하는 방법을 알아봅니다.
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 6ce75fe023cfb9c3be988787e8993db556cf3150
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 0%

---

# Adobe Commerce 개발자 에이전트 App Builder dry run

Adobe Commerce 개발자 에이전트(CDA)를 사용하여 Commerce 확장성 사용 사례를 구축, 배포 및 테스트하는 실습 워크플로우입니다. 이 드라이 실행은 블루프린트에서 기능 테스트에 이르기까지 장바구니 수량 제한 웹후크, 고가치 주문 보류 및 보류된 주문에 대한 이벤트 기반 보관 등 세 가지 사용 사례를 다룹니다.

## 시작

### 문제 및 피드백을 보고하는 방법

시험 실행 시 거친 모서리가 나타나며, 이는 새로운 기능을 사용하는 동안 예상되었습니다. 온보딩 중에 제공된 피드백 템플릿을 사용하여 Adobe 프로그램 담당자와 모든 문제를 캡처하고 공유합니다.

>[!TIP]
>
> 문제를 보고할 때:
>
> * `projectId`(브라우저 URL에 표시됨)을(를) 포함합니다.
> * 관련이 있을 때마다 스크린샷을 포함하십시오.

### 사전 요구 사항

**계정 및 액세스**

* 조기 액세스 IMS 조직에서 **개발자** 역할 이상입니다.
* 해당 조직 내의 Adobe Commerce ACCS(as a Cloud Service) 인스턴스에 대한 관리자 액세스 권한은 **Cloud Service 인스턴스**&#x200B;에서 experience.adobe.com에 있습니다.
* GitHub 계정.

**도구**

기능 유효성을 검사하려면 EDS(Edge Delivery Services) 상점이 필요합니다. 다음이 필요합니다.

* Node.js 22+
* Adobe I/O CLI: `npm install -g @adobe/aio-cli`
* AIO CLI Commerce 플러그인: `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

메시지가 표시되면 ACCS 인스턴스를 선택하여 빈 폴더에 Storefront 보일러플레이트를 설치합니다.

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

상점 시작:

```bash
cd storefront
npm run start
```

## Commerce 개발자 에이전트 열기

1. **개발자 에이전트** 아래의 experience.adobe.com에서 Commerce 개발자 에이전트로 이동합니다.
1. 조기 액세스 IMS 조직 자격 증명을 사용하여 로그인합니다.

## 사용 사례 1: 장바구니 최대 개수 webhook

이 사용 사례는 동기식 Commerce 웹후크를 사용하여 제품이 추가되기 전에 장바구니 수량 제한을 확인합니다.

### 블루프린트 단계

다음 프롬프트를 입력하고 **블루프린트 생성**&#x200B;을 클릭하세요.

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> 다음 항목을 살펴보십시오.
>
> * 요구 사항을 캡처하는 블루프린트(v1)가 생성됩니다.
> * 구현을 안내하는 작업이 만들어집니다.

채팅 상자에 세부 정보를 입력하거나 채팅 상자 위에 있는 알약 중 하나를 클릭하여 블루프린트를 세분화합니다(*가정 충원*, *디자인 간격 찾기* 등). 만족하면 **계획 승인**&#x200B;을 클릭하여 진행하십시오.

### 개발 단계

에이전트가 개발 단계로 전환하고 작업 공간 프로비저닝을 시작합니다.

>[!NOTE]
>
> Explorer 패널에서 다음 파일을 찾습니다.
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

프로비저닝되면 에이전트는 구현 작업 목록을 표시하고 빌드를 시작합니다.

>[!NOTE]
>
> 다음 항목을 살펴보십시오.
>
> * 생성된 코드는 요구 사항과 일치합니다.
> * `Validate` 스트리밍 화면에 작업 영역 유효성 검사(`aio app build`) 진행 상황이 표시됩니다.
> * 유효성 검사가 실패하면 에이전트는 생성된 코드를 자체 수정합니다.

코드가 만족스러우면 **통합** 탭을 클릭하여 앞으로 이동합니다.

### 통합 구성

**App Builder 작업 영역 연결 또는 만들기**

App Builder 프로젝트를 만들거나 연결하려면 화면에 표시되는 안내를 따르십시오.

기존 작업 공간에 연결하는 경우 다음과 같은 사항이 있는지 확인합니다.

* `Runtime` 서비스가 추가되었습니다.
* 추가된 API는 Adobe Commerce as a Cloud Service, I/O 관리 API, App Builder 데이터 서비스, I/O 이벤트, Adobe Commerce용 Adobe I/O Events입니다.

새 작업 영역을 만드는 경우 **Adobe Commerce as a Cloud Service** API를 수동으로 추가하십시오.

>[!IMPORTANT]
>
> 기존 App Builder 프로젝트에 연결되면 **고급 구성**&#x200B;을 확장하고 작업 영역 JSON을 붙여넣은 다음 **상태 다시 확인**&#x200B;을 클릭하여 필요한 모든 API가 설치되었는지 확인합니다.

계속하려면 **다음**&#x200B;을 클릭하세요.

**Commerce에 연결**

목록에서 ACCS 인스턴스를 선택하거나 **Commerce REST 기본 URL** 필드에 URL을 입력한 다음 **Commerce 인스턴스 연결**&#x200B;을 클릭합니다. 계속하려면 **다음**&#x200B;을 클릭하세요.

**GitHub에 연결**

저장소 URL을 입력하고 GitHub 앱 또는 개인 액세스 토큰을 사용하여 작업 영역을 GitHub 저장소에 연결합니다. 계속하려면 **다음**&#x200B;을 클릭하세요.

**환경 변수 구성**

프로젝트에 필요한 환경 변수를 입력합니다.

### 배포

**개발**&#x200B;을 클릭하여 개발 단계로 돌아간 다음 에이전트에게 프롬프트 필드에 배포하도록 요청하십시오.

>[!NOTE]
>
> 조직, 프로젝트, Workspace 및 런타임 네임스페이스를 표시하는 &quot;배포 확인&quot; 메시지를 찾습니다.

배포를 확인합니다.

>[!NOTE]
>
> 다음을 찾습니다.
>
> * 배포 전 유효성 검사 진행률을 보여 주는 `Validate` 스트리밍 화면입니다.
> * 유효성 검사가 실패할 경우 에이전트가 코드를 자체 수정합니다.
> * 배포 진행 상황(`aio app deploy`)을 보여 주는 `Deploy` 스트리밍 화면입니다.
> * 배포가 실패할 경우 에이전트가 코드를 자체 수정합니다.

### 앱 관리에서 앱 연결

1. ACCS 인스턴스 관리자 URL로 이동하여 로그인합니다.
1. 왼쪽 메뉴에서 **앱**&#x200B;을 선택한 다음 **앱 관리**&#x200B;를 선택합니다.
1. **+ 앱 연결**(오른쪽 상단)을 클릭합니다.
1. CDA가 배포한 프로젝트와 Workspace을 선택한 다음 **연결**&#x200B;을 클릭합니다.

>[!NOTE]
>
> 애플리케이션 이름과 버전, 구현된 기능(비즈니스 구성, 웹후크, 이벤트 등)을 보여주는 카드를 찾습니다.

### 앱 관리에서 설치 및 구성

1. 응용 프로그램의 행에서 **설치**&#x200B;를 클릭한 다음 **닫기**&#x200B;를 클릭합니다.
1. 같은 행에서 **구성**&#x200B;을 클릭하여 비즈니스 구성 값을 입력한 다음 **닫기**&#x200B;를 클릭합니다.

>[!NOTE]
>
> 블루프린트에서 지정한 모든 구성 필드를 표시하는 양식을 찾습니다. 이 필드는 지정한 기본값으로 미리 채워져 있습니다.

### 기능 테스트

1. 앱 관리 앱 구성에서 **최대 장바구니 수**&#x200B;을(를) 3으로 설정합니다(빠른 테스트의 경우 낮은 값).
1. 상점 앞에서 빈 장바구니로 시작하십시오.
1. PDP(Product Detail Page)에서 총 수량이 3을 초과할 때까지 제품을 추가합니다. 마지막 추가가 실패합니다.
1. PDP에서 *&quot;최대 항목 수에 도달했습니다.&quot;*&#x200B;이(가) 표시됩니다.
1. 제한보다 낮으면 가 계속 성공합니다.

>[!NOTE]
>
> PLP(제품 목록 페이지)에서 차단된 추가가 메시지 없이 자동으로 실패합니다. 이는 웹후크 오류가 아닌 상점 내 행동입니다. 확인을 위해 PDP를 선호합니다.

## 사용 사례 2: 고가치 주문 보류 및 확인 코드

이 사용 사례를 시작하려면 **블루프린트** 단계로 돌아가십시오.

### 블루프린트 단계

다음 프롬프트를 입력하고 **블루프린트 생성**&#x200B;을 클릭하세요.

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> 다음 항목을 살펴보십시오.
>
> * 요구 사항을 캡처하는 블루프린트(v2)가 생성됩니다.
> * 최초 계획 태스크는 유지됩니다.
> * 새로운 요구 사항에 해당하는 새로운 작업이 추가되었습니다.

필요에 따라 블루프린트를 세분화한 다음 **계획 승인**&#x200B;을 클릭하여 진행합니다.

### 개발, 배포, 연결 및 설치

사용 사례 1에서 사용한 것과 동일한 프로세스를 따라 요구 사항에서 설치된 애플리케이션으로 이동합니다. 통합을 재구성할 필요가 없습니다.

>[!IMPORTANT]
>
> 이미 연결된 앱에 대한 변경 사항을 선택하려면 앱 관리에서 **연결 해제**&#x200B;하고 **연결 해제**&#x200B;해야 합니다.

### 기능 테스트

1. 앱 관리 앱 구성에서 **주문 보류 임계값(USD)**&#x200B;을 50(테스트 장바구니에서 쉽게 초과)으로 설정합니다.
1. 순서 사용자 지정 특성이 있는지 확인합니다(기본값 `lab_verification_code`).
1. 총 50달러 이상으로 주문하세요.
1. 30초 동안 대기(이벤트는 비동기이며, 비우선 순위 게재는 최대 59초 정도 소요될 수 있습니다.)
1. Commerce Admin → Sales → Orders에서 주문을 엽니다. 상태는 **보류 중**(`holded`)입니다. 사용자 지정 특성에는 임의의 값이 있는 `lab_verification_code`이(가) 포함됩니다.
1. 선택 사항: 먼저 $50 미만에 주문을 합니다. 이 핸들러가 해당 주문을 보류하지 않습니다.

## 사용 사례 3: 보류 중인 주문에 대한 이벤트 기반 보관

이 사용 사례를 시작하려면 **블루프린트** 단계로 돌아가십시오.

### 블루프린트 단계

다음 프롬프트를 입력하고 **블루프린트 생성**&#x200B;을 클릭하세요.

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> 다음 항목을 살펴보십시오.
>
> * 요구 사항을 캡처하는 블루프린트(v3)가 생성됩니다.
> * 최초 계획 태스크는 유지됩니다.
> * 새로운 요구 사항에 해당하는 새로운 작업이 추가되었습니다.

필요에 따라 블루프린트를 세분화한 다음 **계획 승인**&#x200B;을 클릭하여 진행합니다.

### 개발, 배포, 연결 및 설치

이전 사용 사례에서 사용한 것과 동일한 프로세스를 따라 요구 사항에서 설치된 애플리케이션으로 이동합니다. 통합을 재구성할 필요가 없습니다.

>[!IMPORTANT]
>
> 이미 연결된 앱에 대한 변경 사항을 선택하려면 앱 관리에서 **연결 해제**&#x200B;하고 **연결 해제**&#x200B;해야 합니다.

### 기능 테스트

1. 사용 사례 2 임계값이 테스트에 충분히 낮은지 확인합니다(예: 앱 구성의 경우 $50).
1. 해당 임계값을 초과하여 주문하므로 사용 사례 2를 잠시 중단합니다(~30초).
1. Adobe Developer Console → Project → Stage → Events에서 보류 주문 아카이브 이벤트(설치 시 추가 또는 업데이트)에 대한 등록을 엽니다.
1. 주문이 보류로 이동한 후 이벤트가 해당 등록에 전달되었는지 확인합니다. `order-archive/archive-held-order`에 연결된 Commerce 이벤트에 대한 이벤트 추적 또는 모니터링을 사용합니다.

>[!NOTE]
>
> 이벤트는 비동기적입니다. 즉, 순서를 보류한 후 최대 30-59초까지 사용할 수 있습니다.

## 문제 해결

CDA에서 생성한 애플리케이션이 예상대로 작동하지 않거나 오류가 발생하는 경우 에이전트에게 개발 단계에서 문제를 해결하도록 요청하십시오.

>[!NOTE]
>
> CDA는 외부에서 발생하는 단계에 대한 가시성이 없습니다. 연결, 설치, 구성 및 기능 테스트는 모두 CDA가 아닌 Commerce 관리, 앱 관리 또는 상점 첫 화면에서 실행됩니다. 이러한 영역 중 하나에 문제가 표시되면 에이전트에서 이를 확인할 수 없기 때문에 누락된 내용을 전달합니다.
>
> * 수행한 작업 및 위치(예: &quot;앱 관리에서 설치를 클릭함&quot;).
> * 예상했던 일이요
> * 무슨 일이야?
> * 화면에 표시되는 정확한 오류 텍스트 또는 메시지입니다.
> * 브라우저 콘솔 또는 Adobe Developer Console의 App Builder 로그 및 이벤트 등록 디버그 추적에서 발생하는 모든 관련 오류.

보고서가 구체적일수록 에이전트는 문제를 더 잘 진단할 수 있습니다.

## 선택적 단계

**코드 다운로드**

즐겨 사용하는 IDE에서 계속 다듬거나 편집하려면 개발 단계 탐색기 도구 모음에서 다운로드 아이콘을 클릭하여 CDA에서 생성된 코드를 다운로드합니다. 대상 폴더를 선택하고 **저장**&#x200B;을 클릭한 다음 작업 영역 패키지의 압축을 해제합니다.

>[!NOTE]
>
> 다음을 찾습니다.
>
> * 개발 단계 탐색기에 표시되는 모든 파일은 압축 해제된 폴더에 있습니다.
> * `aio app build`(으)로 프로젝트를 빌드할 때 &quot;컴파일&quot; 오류가 없습니다.

CDA가 사용하는 것과 동일한 에이전트 기술을 사용하려면 해당 기술을 프로젝트 폴더에 설치합니다.

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

그런 다음 IDE 또는 CLI를 시작하고 프롬프트를 표시합니다.

**파일 또는 링크를 통해 컨텍스트 첨부**

블루프린트 또는 개발 단계에서 직접 메시지를 표시하는 대신 텍스트 파일 또는 링크를 사용하여 컨텍스트를 첨부할 수 있습니다.

1. 채팅 상자에 있는 첨부 파일 아이콘을 클릭합니다.
1. 로컬 텍스트 파일을 업로드하려면 **파일 추가**&#x200B;를 클릭하고, 원격 파일을 통해 컨텍스트를 추가하려면 URL을 입력하고 **링크 추가**&#x200B;를 클릭하십시오.
1. **완료**&#x200B;를 클릭하고 메시지를 입력하여 에이전트를 조금씩 이동합니다.

>[!NOTE]
>
> 첨부 파일의 컨텍스트를 다음 순서로 통합하는 에이전트를 찾습니다.

## 알려진 문제 및 해결 방법

**블루프린트 단계에서 작업을 생성하지 않습니다**

차단을 해제하고 계속하려면 에이전트를 살짝 밀어 작업을 생성합니다.

**GitHub에서 푸시하고 가져오는 단추가 작동하지 않습니다**

대신 개발 단계에서 프로젝트 ZIP 파일을 다운로드합니다.

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

<!-- ## Additional resources -->

<!-- Link to related Experience League or Adobe Developer documentation. -->
