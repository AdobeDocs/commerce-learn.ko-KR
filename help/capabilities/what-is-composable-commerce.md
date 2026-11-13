---
title: Adobe과 컴포저블 Commerce
description: Adobe 플랫폼 옵션, Edge Delivery Services 상점, API Mesh, App Builder 및 Commerce 개발 MCP 도구를 사용한 구성 가능한 상거래에 대해 알아봅니다.
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# 구성 가능한 상거래

컴포저블 커머스는 API를 통해 상거래 기능을 결합한 아키텍처 접근법이다. 긴밀하게 결합된 하나의 애플리케이션에서 모든 기능을 사용할 필요 없이 상점 환경, 카탈로그 관리, 제품 검색, 체크아웃 및 기타 비즈니스 요구 사항에 맞게 구성 요소를 선택하고 통합할 수 있습니다.

Headless commerce는 상거래 백엔드에서 상점 표시 계층을 분리합니다. 구성 가능한 커머스는 팀이 함께 유지할 기능과 독립적으로 통합할 기능을 선택할 수 있도록 함으로써 이러한 분리를 기반으로 합니다. Headless Storefront는 모든 백엔드 구성 요소를 본질적으로 독립적으로 교체할 수 없습니다.

Adobe은 상거래 플랫폼 옵션, storefront 기술, 통합 및 개발 도구를 통해 이 접근 방식을 지원합니다. Adobe Commerce as a Cloud Service(ACCS), Adobe Commerce Optimizer(ACO), Edge Delivery Services, API Mesh, App Builder 및 Commerce 개발 MCP는 서로 다른 용도로 사용됩니다. 그들은 교환 가능한 제품 또는 필요한 결합 스택이 아닙니다.

## 이점 및 장단점

컴포저블 접근 방식은 다음과 같은 이점을 제공합니다.

* **고객 경험 조정:** 상점 구성 요소를 선택하고 비즈니스 요구 사항에 맞는 서비스를 통합하십시오.
* **점진적 현대화:** 다른 시스템과 해당 데이터를 유지하면서 선택한 기능을 바꿉니다.
* **독립적으로 확장 및 발전:** 통합 및 확장을 디자인하므로 변경 사항에 전체 상거래 응용 프로그램을 릴리스할 필요가 없습니다.
* **상거래 기능 재사용:** 모든 비즈니스 기능을 다시 작성하는 대신 지원되는 플랫폼 기능 및 API를 사용하십시오.

이러한 이점은 구현에 따라 다릅니다. 기술적 완성도, 제공 비용, 라이선스, 보안 및 데이터 개인정보 보호를 평가합니다. 서비스 전반에 걸쳐 API 계약, 데이터 동기화, 인증, 성능 및 장애에 대한 소유권을 정의합니다. 또한 모듈식 아키텍처를 통해 통합과 운영 복잡성을 높일 수 있습니다.

## Adobe Commerce 아키텍처 옵션

먼저 관리 상거래 플랫폼이 필요한지 또는 기존 트랜잭션 백엔드를 중심으로 고객 경험을 현대화할지 여부를 결정합니다.

비디오 비교에 대해서는 [Adobe의 사용 가능한 상거래 옵션](./adobes-available-commerce-options.md)을 참조하십시오.

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service은 관리형 SaaS 상거래 플랫폼입니다. Adobe은 핵심 애플리케이션, 인프라 및 업데이트를 관리합니다. 아키텍처에는 Commerce Foundation, 헤드리스 상점 기능, 머천다이징 서비스 및 개발자 도구가 포함되어 있습니다.

지원되는 확장 지점이 있는 관리되는 상거래 기반을 평가할 때 이 옵션을 사용합니다. 사용자 정의 비즈니스 논리의 경우, 기존 PHP 확장을 변경되지 않고 배포할 수 있다고 가정하는 대신 지원되는 프로세스 외부 확장성에 대한 계획을 수립하십시오. ACCS는 Luma 상점 전면을 지원하지 않습니다. 해당되는 경우 구현 계획에 상점 마이그레이션을 포함하십시오.

[Adobe Commerce as a Cloud Service 개요](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/overview)를 참조하세요.

### Adobe Commerce Optimizer

Adobe Commerce Optimizer은 카탈로그 관리, 제품 검색, 검색, 권장 사항 및 상점 경험을 지원하는 동시에 기존 상거래 백엔드를 유지할 수 있습니다. 카탈로그 데이터는 외부 소스에서 가져올 수 있으며, 서드파티 시스템은 장바구니 및 체크아웃 라이프사이클을 계속 담당할 수 있습니다.

고객 대면 현대화가 목표이고 트랜잭션 백엔드를 교체할 필요가 없는 경우 이 옵션을 사용합니다. 카탈로그 수집 및 장바구니와 체크아웃 통합을 명시적으로 계획합니다. ACCS와 ACO는 필수 조합 스택이 아닌 별도의 아키텍처 옵션입니다. 구현에 대한 서비스를 선택하기 전에 현재 통합 지원을 확인하십시오.

[Adobe Commerce Optimizer 개요](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview)를 참조하세요.

### 기존 Adobe Commerce 배포

Adobe Commerce on cloud infrastructure 및 온프레미스 배포는 headless 상점 및 지원되는 API 기반 통합을 점진적으로 채택할 수 있습니다. 배포에 따라 기존 Storefront를 유지하거나 지원되는 Headless Storefront를 채택할 수 있습니다. 기존 상거래 기능을 함께 유지하면 통합 작업을 줄이는 동시에 선택한 서비스를 별도로 발전시킬 수 있습니다.

기능 가용성, 확장 호환성, 라이선스 및 설정 요구 사항은 서비스 및 버전에 따라 다릅니다. 아키텍처를 선택하기 전에 [Adobe Commerce 플랫폼 및 서비스를 비교](https://experienceleague.adobe.com/en/docs/commerce/information-center/explore-platforms-and-services/platform-and-services)하십시오.

## Edge Delivery Services 기반 Commerce Storefront

Edge Delivery Services에서 제공하는 Commerce Storefront는 Storefront 경험 및 컨텐츠 전달 레이어를 제공합니다. Headless 아키텍처를 통해 팀은 상거래 기반과는 독립적으로 프론트엔드를 개발할 수 있습니다. Storefront 구성 요소는 쇼핑 기능을 위해 상거래 API를 사용하는 반면 문서 기반 작성 및 시각적 편집은 콘텐츠 업데이트를 지원합니다.

카탈로그 데이터, 가격 책정, 장바구니 작업, 체크아웃 및 주문을 담당하는 시스템과 상점 인터페이스를 구분합니다. Edge Delivery Services은 트랜잭션 상거래 백엔드를 대체하지 않습니다. Storefront를 구현하기 전에 선택한 백엔드에 대한 지원 서비스 및 통합 경로를 확인하십시오.

[Commerce Storefront 설명서](https://experienceleague.adobe.com/en/tools/commerce-storefront/)를 참조하세요.

## 통합 및 확장성

API Mesh 및 App Builder은 핵심 상거래 애플리케이션에 모든 사용자 지정을 임베드할 필요 없이 상거래 기능을 연결하고 확장하는 데 도움이 됩니다.

### API 메쉬

Adobe Developer App Builder용 API Mesh는 여러 Adobe, 개인 및 서드파티 API 소스를 쿼리 가능한 GraphQL 엔드포인트에 결합합니다. 프론트엔드 경험에 대한 데이터를 작성하고 기존 서비스에 대한 액세스를 조정하는 데 사용합니다.

API를 구성해도 API 계약, 인증, 오류 처리 또는 트랜잭션 비즈니스 로직의 소유권을 정의할 필요가 없습니다. 통합 디자인에서 이러한 책임을 명시적으로 유지합니다.

[API Mesh 개요](https://developer.adobe.com/graphql-mesh-gateway/)를 참조하세요.

### Adobe App Builder

Adobe App Builder은 Adobe 솔루션을 확장하고 Adobe I/O Runtime을 비롯한 Adobe 인프라에서 실행되는 애플리케이션을 위한 플랫폼을 제공합니다. CRM, ERP 및 PIM과 같은 시스템과의 프로세스 외부 사용자 지정 및 통합에 사용할 수 있습니다.

사용하는 Commerce에 대해 지원되는 확장 지점, 이벤트 및 웹후크를 선택합니다. 핵심 상거래 릴리스 라이프사이클과 별도로 애플리케이션을 디자인하고 테스트합니다.

[App Builder 개요](https://developer.adobe.com/app-builder/docs/intro_and_overview/)를 참조하세요.

## Commerce 개발 MCP 및 기술

Commerce 개발 MCP 및 기술은 App Builder 애플리케이션의 AI 지원 개발 및 상점 맞춤화를 지원합니다. 또한 ACCS로 이동할 때 기존 Adobe Commerce PHP 확장을 App Builder 애플리케이션으로 마이그레이션하는 데 도움이 될 수 있습니다.

모델 컨텍스트 프로토콜(MCP) 서버는 호환되는 AI 코딩 클라이언트를 개발 도구에 연결합니다. Adobe의 도구에는 Commerce 개발자 MCP 서버, App Builder MCP 서버 및 Dropins AI 도구와 Commerce 관련 기술이 포함됩니다.

Commerce 개발 MCP는 Storefront 게재, 장바구니, 체크아웃 또는 주문 처리 서비스가 아닌 개발 도구입니다. 모든 Commerce 제품과 자동으로 호환되는 애플리케이션은 아닙니다. 생성된 변경 사항을 검토하고 타겟 환경에 대해 테스트합니다.

[Commerce 개발 MCP 및 기술](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools)을 참조하세요.

## 구성 가능한 구현 계획

1. 비즈니스 목표, 고객 경험, 보안 요구 사항 및 팀이 사용할 수 있는 기술을 정의합니다.
2. 상거래 플랫폼을 선택하고 유지하거나 대체할 기존 시스템을 식별합니다.
3. 지원되는 저장소를 선택하고 프레젠테이션, 머천다이징 및 트랜잭션 서비스 간의 경계를 정의합니다.
4. 카탈로그 정보, 가격, 재고, 고객, 장바구니, 체크아웃 및 주문에 대한 데이터 소유권 및 API 계약을 설정합니다.
5. 통합 전반에 걸친 인증, 데이터 동기화, 캐싱, 모니터링 및 장애 처리를 계획합니다.
6. 각 릴리스 전에 기능, 성능 및 보안을 점진적으로 구현하고 검증하십시오.

구성 가능한 구현은 구성 요소가 비즈니스 요구 사항을 충족하고 안정적으로 함께 작동할 때 성공합니다. 아키텍처를 더욱 모듈화하기 위한 서비스만 추가하는 것이 아니라 필요한 기능을 선택하십시오.

## 관련 리소스

이러한 튜토리얼, 비디오, 구현 안내서 및 재생 목록을 탐색하여 아키텍처를 계획하고 상거래 통합을 구축하십시오.

### Commerce 플랫폼 및 상점

* [Adobe Commerce as a Cloud Service 아키텍처](../commerce-as-a-cloud-service/architecture.md)
* [구성 가능한 상점 및 모듈식 아키텍처](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Adobe Commerce Optimizer 개요](../adobe-commerce-optimizer/overview.md)
* [구성 가능한 카탈로그 데이터 모델이 있는 이유](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Edge Delivery Services을 사용한 쇼핑객 경험](../edge-delivery-services/shopper-experience.md)

### 통합 및 개발

* [App Builder을 통한 프로세스 중단 확장성](../app-builder/foundations/introduction-to-app-builder.md)
* [API Mesh 시작](../api-mesh/getting-started-api-mesh.md)
* [여러 소스를 사용하여 API Mesh 만들기](../api-mesh/graphql-multiple-source.md)
* [AI 및 MCP 도구를 사용하여 맞춤형 배송률 구축](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### 아키텍처 및 개발 설명서

* [Adobe Commerce as a Cloud Service으로 마이그레이션](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/migration/overview)
* [Adobe Commerce Optimizer 통합](https://experienceleague.adobe.com/en/docs/commerce/optimizer/integrations-overview)
* [Commerce Storefront 개발 시작](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Commerce 개발 MCP 및 기술에 대한 우수 사례](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### 재생 목록

* [Adobe App Builder 및 Adobe Commerce을 사용하여 API Mesh 시작](https://experienceleague.adobe.com/en/playlists/commerce-get-started-app-builder-and-api-mesh)
