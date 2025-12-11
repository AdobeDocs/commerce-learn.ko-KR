---
title: Adobe Commerce을 위한 프로세스 외부 확장성
description: Adobe App Builder에 대해 알아보고 이 애플리케이션이 프로세스 외 확장성에 있어 중요한 이유를 알아봅니다.
landing-page-description: App Builder란 무엇이며 Adobe Commerce 개발 전략에 어떻게 도움이 되는지 알아봅니다.
short-description: App Builder란 무엇이며 Adobe Commerce 개발 전략에 어떻게 도움이 되는지 알아봅니다.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 6%

---

# App Builder 소개

지금까지 Adobe Commerce 개발에서는 처리 중 확장성을 사용했습니다. 처리 중인 모델에는 업그레이드, 서버의 PHP 버전 및 Commerce에서 사용하는 기타 필수 서버 애플리케이션 및 서비스와 호환될 수 있는 모든 새 코드가 필요합니다. Adobe Developer App Builder은 이러한 호환성 문제를 방지하기 위해 프로세스 외부 확장성을 사용합니다.

## Adobe Commerce용 App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

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
* 이제 쉽게 업그레이드할 수 있습니다. 사용자 지정 기능은 Commerce 코드베이스 외부에 있으므로 업그레이드 시 호환성 문제를 방지할 수 있습니다.
* Commerce 외부로 기능 및 논리를 이동하면 일반적으로 프로세스 내 개발 메서드에서 사용하는 리소스가 확보됩니다.

## 아키텍처 {#architecture}

Adobe Developer App Builder은 기본 솔루션 대신 Adobe Commerce과 같은 Adobe Cloud 솔루션을 확장하기 위해 공통적이고 일관적이며 표준화된 개발 플랫폼을 제공합니다. 여기에는 다음과 같은 항목이 포함됩니다.

* 사용자 지정 마이크로서비스 및 확장 개발에 사용되는 Adobe Developer Console. 플러그인 및 통합을 만드는 데 필요한 모든 도구 및 API에 액세스하는 동안 프로젝트를 빌드하고 관리합니다.
* 사용자 지정 확장 및 통합을 구축할 수 있는 오픈 소스 도구, SDK 및 라이브러리입니다. React Spectrum(Adobe의 UI 툴킷)을 사용하여 모든 Adobe 앱에 대해 하나의 공통 UI를 가질 수 있습니다.
* Adobe의 서버리스 플랫폼에서 인프라를 호스팅하기 위한 I/O Runtime과 이벤트 기반 통합을 위한 I/O Events 등의 서비스. Adobe은 또한 데이터 및 파일 저장을 위한 기본 지원을 제공합니다.
* Adobe Experience Cloud을 사용하여 확장 및 통합을 Experience Cloud 조직에 게시합니다. 시스템 관리자는 이러한 확장을 검토, 관리 및 승인할 수 있습니다. 게시되면 사용자 지정 App Builder 확장 및 도구를 다른 Adobe Experience Cloud 앱과 함께 사용할 수 있습니다.

다음 다이어그램은 App Builder에 구축된 표준 애플리케이션이 이러한 기능을 사용하는 방법을 보여 줍니다.

![아키텍처](/help/assets/app-builder/app-builder-architecture.jpeg)

App Builder 아키텍처에 대한 자세한 내용은 [아키텍처 개요](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}를 참조하십시오.

## App Builder 시작 {#additional-resources}

초기 설정을 포함하는 구성 가능한 상거래 전략에 대한 개요는 다음 블로그 게시물을 통해 확인할 수 있습니다.

[App Builder이 상거래 플랫폼의 비즈니스 민첩성을 높이는 데 어떻게 도움이 됩니까](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builder을 시작할 수 있도록 Adobe은 다음 설명서를 만들었습니다.

* [App Builder 시작](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 설명서를 사용하여 학습 계속 {#appbuilder-documentation}

App Builder은 사용자 정의 애플리케이션을 개발하는 데 도움이 되는 안내서 및 참조 설명서를 포함하여 개발자를 위한 비디오 및 설명서를 제공합니다.

* [App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 샘플 애플리케이션 중 하나를 사용해 보십시오. {#appbuilder-codesamples}

개발을 시작할 준비가 되셨습니까? 다음 링크에는 시작하는 데 도움이 되는 샘플 응용 프로그램이 포함되어 있습니다.

* Adobe Developer 웹 사이트의 [App Builder 코드 랩](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 지원 {#support}

개발자 지원 요청의 경우 도움이 필요하면 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"}을 사용하십시오.

{{$include /help/_includes/app-builder-related-links.md}}
