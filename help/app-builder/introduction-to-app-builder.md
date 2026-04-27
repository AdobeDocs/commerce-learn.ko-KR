---
title: Adobe Commerce을 위한 프로세스 외부 확장성
description: Adobe App Builder에 대해 알아보고 이 애플리케이션이 프로세스 외 확장성에 있어 중요한 이유를 알아봅니다.
jira: KT-11433
doc-type: Tutorial
duration: 322
last-substantial-update: 2023-02-16T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
TQID: https://experienceleague.adobe.com/c3dl6gZ7Jtje5rZCB9HrwmCbFrcu8bllL0z0B9cl5Jg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: c32adafa-ed01-4b31-997e-2413013911b0id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 777
ht-degree: 2%

---

# App Builder 소개

지금까지 Adobe Commerce 개발에서는 처리 중 확장성을 사용했습니다. 처리 중인 모델에는 업그레이드, 서버의 PHP 버전 및 Commerce에서 사용하는 기타 필수 서버 애플리케이션 및 서비스와 호환될 수 있는 모든 새 코드가 필요합니다. Adobe Developer App Builder은 이러한 호환성 문제를 방지하기 위해 프로세스 외부 확장성을 사용합니다.

## Adobe Commerce용 App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?learn=on)

Adobe Developer App Builder은 Adobe 솔루션을 확장하기 위해 사용자 정의 경험을 통합하고 만들기 위한 서버리스 확장성 플랫폼이며 이제 Adobe Commerce에서 사용할 수 있습니다. App Builder을 사용하면 Commerce 기본 기능을 확장하고 서드파티 솔루션과 통합하는 안전하고 확장 가능한 앱을 구축할 수 있습니다. 이제 개발자로서 Adobe Commerce을 통해 프로세스 외부 확장성을 활용할 수 있으며, 이를 통해 즉각적이고 장기적인 이점을 제공합니다.

App Builder은 [!DNL Adobe Commerce]을(를) 확장하는 사용자 지정 응용 프로그램을 통합하고 만들기 위한 통합 타사 확장성 프레임워크를 제공합니다. 이 확장성 프레임워크는 Adobe의 인프라에 빌드되므로 개발자는 사용자 지정 마이크로 서비스를 빌드하고 다른 Adobe 솔루션 및 서드파티 통합에서 [!DNL Adobe Commerce]을(를) 확장 및 통합할 수 있습니다.

App Builder에서는 고객이 다양한 사용 사례에서 [!DNL Adobe Commerce]을(를) 확장할 수 있는 방법을 제공합니다.

* 미들웨어 확장성 - 사용자 정의 커넥터를 빌드하여 외부 시스템을 Adobe 애플리케이션과 연결하거나 사전 설치된 통합 세트를 이용합니다.
* 핵심 서비스 확장성 - 사용자 정의 기능 및 비즈니스 논리를 사용하여 기본 동작을 확장하여 핵심 애플리케이션 기능을 확장합니다.
* 사용자 경험 확장성 - 핵심 경험을 확장하여 비즈니스 요구 사항을 지원하거나 고객별 디지털 속성, 상점 및 백오피스 애플리케이션을 구축할 수 있습니다.

Adobe Developer App Builder은 클라우드 기반 솔루션으로, 자동으로 확장이 가능하다는 의미입니다. 또한 이 서비스는 지리적 위치에 관계없이 최고의 성능을 제공하기 위해 전역으로 배포됩니다.

## App Builder에 대해 자세히 알아봐야 하는 이유

Adobe Commerce은 완전한 SAAS 제품이 아니기 때문에 개발하는 코드는 복잡성과 업그레이드 문제를 추가할 수 있습니다. App Builder과 같은 프로세스 외부 확장성을 사용하여 프로세스 내 메서드 없이 Adobe Commerce 스토어에 고유한 사용자 지정 기능을 제공할 수 있습니다.

기타 이점은 다음과 같습니다.

* 분리된 기능을 사용하면 론치 시간을 단축할 수 있습니다.
* 이제 쉽게 업그레이드할 수 있습니다. The custom features are outside the Commerce codebase, which prevents  compatibility issues when upgrading.
* Moving features and logic outside of Commerce frees up resources that are normally used by in-process development methods.

## 아키텍처 {#architecture}

Instead of an out-of-the-box solution, Adobe Developer App Builder provides a common, consistent, and standardized development platform for extending Adobe Cloud solutions such as Adobe Commerce including:

* Adobe Developer Console used for custom microservice and extension development. Build and manage projects while accessing all the tools and APIs needed to create plugins and integrations.
* Open-source tools, SDKs, and libraries to build custom extensions and integrations. Use  React Spectrum (Adobe&#39;s UI toolkit) to have one common UI for all Adobe apps.
* services such as I/O Runtime for hosting infrastructure on Adobe&#39;s serverless platform and I/O Events for event-based integrations. Adobe also provides out-of-the-box support for storing data and files.
* Adobe Experience Cloud where you submit extensions and integrations to publish in your Experience Cloud Org. System admins can review, manage, and approve these extensions. Once published, your custom App Builder extensions and tools are available alongside other Adobe Experience Cloud apps.

The following diagram illustrates how a standard application built on App Builder uses these functionalities:

![Architecture](/help/assets/app-builder/app-builder-architecture.jpeg)

For more details about the App Builder architecture, see the [Architecture Overview](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Get Started with App Builder {#additional-resources}

A n overview of composable commerce strategy, that includes the initial set-up can be found by reading the following blog post:

[How App Builder helps drive business agility for your commerce platform](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

To help you get started with App Builder, Adobe has created the following documentation:

* [App Builder Getting Started](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Continue learning with Documentation {#appbuilder-documentation}

App Builder provides videos and documentation for developers, including guides and reference documentation to help develop your own custom applications:

* [App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 샘플 애플리케이션 중 하나를 사용해 보십시오. {#appbuilder-codesamples}

개발을 시작할 준비가 되셨습니까? 다음 링크에는 시작하는 데 도움이 되는 샘플 응용 프로그램이 포함되어 있습니다.

* [Adobe Developer 웹 사이트의 App Builder 코드 랩](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 지원 {#support}

개발자 지원 요청의 경우 도움이 필요하면 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"}을 사용하십시오.

{{$include /help/_includes/app-builder-related-links.md}}
