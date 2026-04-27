---
title: How Adobe Creates Composable Commerce
description: Learn about composable commerce, prioritizing an API-first approach and implement a modular and service-oriented architecture.
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 323
last-substantial-update: 2024-07-6
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c32adafa-ed01-4b31-997e-2413013911b0
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1305
ht-degree: 0%

---

# Composable commerce

Composable commerce is an architectural approach in e-commerce that involves decoupling the front-end presentation layer from the back-end commerce functionality. &#x200B; It allows businesses to select and combine the best components or modules to create a customized solution. This approach involves breaking down the traditional monolithic e-commerce platform into smaller, independent services or microservices that can be composed together. Composable commerce offers benefits such as flexibility, scalability, customization, agility, and the ability for easier integrations with other systems and technologies.

Adobe Commerce provides many capabilities and tools for supporting merchants in adopting and implementing composable commerce. Adobe Commerce offers a composable commerce methodology and hybrid headless and non-headless front-end experiences. With the out of process extensibility in mind, Adobe offers API Mesh for integrating multiple services, and Adobe App Builder for creating custom microservices.

## Why is composable commerce important

Understanding composable commerce is important for businesses and individuals involved in the e-commerce industry for several reasons. It provides flexibility and customization, scalability and agility, integration capabilities, future-proofing, and developer empowerment. Composable commerce enables businesses to adapt and evolve their e-commerce platforms, scale and grow their operations. A few more impressive benefits include the ability to integrate with other systems and technologies, keep up with evolving customer expectations, and leverage the expertise of developers.

It helps businesses navigate the evolving e-commerce landscape, make informed decisions, and leverage the benefits of flexibility, scalability, customization, agility, and integration capabilities. Additionally, knowing about composable commerce is important for business growth, customization, agility and innovation, cost efficiency, integration and flexibility, and future-proofing. It enables businesses the opportunity for quick adaption for new features and respond to changing market conditions, create highly customized solutions. There are more benefits that include the ability to innovate rapidly, reduce development and maintenance costs, and integrate with third-party services and systems.

Overall, understanding composable commerce empowers businesses to make informed decisions, optimize their e-commerce architecture and strategy, and drive growth and success in the digital marketplace.

## How can companies benefit from composable commerce

Companies can benefit from composable commerce in several ways:

**Flexibility and Customization:** Composable commerce allows companies to select and combine the best components or microservices to create a customized e-commerce solution that meets their specific needs. It enables businesses to tailor their platform to deliver unique customer experiences, implement specialized functionalities, and differentiate themselves in the market.

**Scalability and Agility:** With a composable architecture, companies can scale different components of their e-commerce platform independently. 즉, 수요에 따라 자원을 할당하고 특정 기능을 확장하여 최적의 성능과 비용 효율성을 보장할 수 있습니다. 또한 Composable Commerce를 통해 민첩한 개발 작업을 수행할 수 있으므로 팀이 서로 다른 구성 요소에서 동시에 작업하고 독립적으로 변경 사항을 배포하여 마켓 출시 시간을 단축할 수 있습니다.

**통합 기능:** 구성 가능한 상거래를 통해 서드파티 시스템, 서비스 및 기술과의 원활한 통합을 지원합니다. 기업은 전자 상거래 플랫폼을 결제 게이트웨이, ERP 시스템, CRM 시스템, 마케팅 자동화 플랫폼 등 다양한 도구와 쉽게 연결할 수 있습니다. 이 통합 기능을 통해 기업은 동급 최고의 솔루션을 활용하고 운영 효율성과 고객 경험을 향상시키는 통합 에코시스템을 구축할 수 있습니다.

**미래 지향적 기능:** 컴포저블 커머스는 기술 및 시장 트렌드가 변화함에 따라 전자 상거래 플랫폼을 채택하고 발전시킬 수 있는 유연성을 기업에 제공합니다. 이를 통해 손쉽게 구성 요소를 추가하거나 교체하고, 새로운 기술을 통합하고, 경쟁에서 앞서나갈 수 있습니다. 이러한 미래 지향적 기능은 기업이 지속적으로 혁신하고 변화하는 고객 기대치를 충족할 수 있도록 해줍니다.

**개발자 권한 부여:** 컴포저블 커머스는 개발자에게 다양한 기술, 언어 및 프레임워크를 사용하여 작업할 수 있는 유연성을 제공하여 권한을 부여합니다. 개발자가 특정 구성 요소나 마이크로서비스에 집중할 수 있도록 해 전문화와 전문성을 가능케 한다. 이러한 권한 부여로 개발자 생산성이 향상되고 개발 주기가 빨라지며 최신 도구와 기술을 활용할 수 있습니다.

전반적으로 컴포저블 상거래를 통해 회사는 변화하는 비즈니스 요구에 적응하고 탁월한 고객 경험을 제공하며 디지털 시장에서 성장과 성공을 이끌 수 있는 유연하고 확장 가능하며 사용자 정의 가능한 전자 상거래 플랫폼을 만들 수 있습니다.

## 구성 가능한 상거래를 위한 Adobe Commerce의 기능은 무엇입니까?

Adobe Commerce은 판매자가 구성 가능한 상거래를 채택하고 구현할 수 있도록 지원하는 몇 가지 기능을 제공합니다.

**구성 가능한 Commerce 방식:** Adobe Commerce은 판매자가 구성 가능한 아키텍처의 원리를 이해하고 구현하는 데 도움이 되는 구성 가능한 상거래 방식을 제공합니다. 이러한 방법론을 통해 기업은 유연성, 확장성 및 맞춤화의 이점을 활용하면서 복잡성, 기술 성숙도 및 프로젝트 크기와 같은 요소를 고려할 수 있습니다.

**다양한 기능:** Adobe Commerce은 GraphQLAPI를 통해 액세스할 수 있는 포괄적인 기능 집합을 제공합니다. 이러한 다양한 기능을 통해 판매자는 상거래 스택에 필요한 공급업체 수를 최소화하여 시장 출시 시기를 단축할 수 있습니다. 비즈니스 스택이 발전함에 따라 추가적인 타사 서비스 또는 기능을 구성하고 통합할 수 있는 유연성을 유지하면서 비즈니스를 더 빠르게 시작할 수 있습니다.

**하이브리드 프론트엔드 경험:** Adobe Commerce은 동일한 에코시스템 내에서 Headless 및 Headless가 아닌 프론트엔드 경험을 모두 지원합니다. 이러한 유연성으로 인해 판매자는 각 특정 프론트엔드 사용 사례에 가장 적합한 아키텍처 접근 방식을 선택할 수 있습니다. 전체 시스템을 동시에 마이그레이션할 필요 없이 Headless 및 구성 가능한 아키텍처로 점진적으로 전환할 수 있습니다.

**API Mesh:** Adobe Commerce의 API Mesh를 사용하면 여러 마이크로서비스, 타사 도구 및 애플리케이션을 프론트엔드 개발자를 위한 통합 API 엔드포인트로 간편하게 통합할 수 있습니다. 이를 통해 개발자는 여러 데이터 소스를 단일 GraphQL 엔드포인트로 결합하여 복잡성을 줄이고 새로운 기능 및 경험의 개발 및 유지 관리를 간소화할 수 있습니다.

**Adobe App Builder:** Adobe App Builder은 판매자가 사용자 지정 마이크로 서비스를 만들고, 사용자 지정 경험을 빌드하고, Adobe 솔루션을 확장할 수 있는 서버를 사용하지 않는 확장성 플랫폼입니다. App Builder을 사용하면 판매자는 Adobe Commerce의 기본 기능을 확장하고 서드파티 솔루션과 통합하는 안전하고 확장 가능한 앱을 구축할 수 있습니다. 따라서 상인은 커스터마이제이션 및 마이크로서비스 관련 인프라를 자체적으로 구축하고 유지 관리할 필요가 없으므로 복잡성이 줄어들고 TCO가 절감됩니다.

Adobe Commerce에서 제공하는 이러한 기능을 통해 구성 가능한 상거래의 채택 및 구현이 간소화되어 상인은 유연성, 확장성, 맞춤화 및 통합 기능의 이점을 활용하는 동시에 복잡성과 개발 노력을 줄일 수 있습니다.

## 마지막 생각

컴포저블 커머스는 프론트엔드 프레젠테이션 레이어와 백엔드 커머스 기능을 분리하는 것을 포함하는 전자 상거래의 아키텍처 접근법입니다. 다음은 구성 가능한 상거래에 대해 학습한 주요 교훈입니다.

**아키텍처:** 구성 가능한 커머스는 모듈식 및 분리된 아키텍처를 따르므로 기업에서 최상의 구성 요소 또는 마이크로서비스를 선택하고 결합하여 사용자 지정된 솔루션을 만들 수 있습니다. 이 아키텍처는 유연성, 확장성 및 민첩성을 제공합니다.

**이점:** 컴포저블 커머스는 유연성 및 사용자 지정, 확장성 및 민첩성, 통합 기능, 미래 대비 기능, 개발자 권한 부여 등 여러 이점을 제공합니다. 이를 통해 기업은 개발자 전문 지식을 조정, 확장, 통합, 혁신 및 활용할 수 있습니다.

**고려 사항:** 구성 가능한 상거래를 고려할 때 복잡성, 내부 기술 성숙도, 프로젝트 크기 및 구조, 사용자 지정 대 표준화, 총 소유 비용, 보안 및 데이터 개인 정보 보호 등의 요소를 신중하게 평가해야 합니다.

**Adobe Commerce:** Adobe Commerce은 상인이 구성 가능한 상거래를 채택하고 구현할 수 있도록 지원하는 기능과 도구를 제공합니다. 여기에는 구성 가능한 상거래 방법론, 다양한 기능, 하이브리드 프론트엔드 경험, 통합을 위한 API Mesh 및 맞춤형 마이크로 서비스를 위한 Adobe App Builder이 포함됩니다.

**비즈니스 영향:** 구성 가능한 상거래를 통해 기업은 유연하고 확장 가능하며 사용자 지정 가능한 전자 상거래 플랫폼을 만들 수 있습니다. 이를 통해 고유한 고객 경험을 제공하고, 수요에 따라 확장하며, 다른 시스템과 통합하고, 향후 운영을 보장하고, 개발자 전문 지식을 활용할 수 있습니다.

컴포저블 커머스를 이해하는 것은 전자 상거래 업계의 기업이 경쟁력을 유지하고 변화하는 시장 상황에 적응하며 탁월한 고객 경험을 제공하는 데 중요합니다. 컴포저블 커머스를 수용하여 기업은 유연성, 확장성, 맞춤화, 민첩성, 통합 기능의 이점을 활용하여 디지털 시장에서 성장과 성공을 이끌어낼 수 있습니다.
