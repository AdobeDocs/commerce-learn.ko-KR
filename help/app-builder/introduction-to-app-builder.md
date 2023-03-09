---
title: Adobe Commerce을 위한 프로세스 외부 확장성
description: Adobe App Builder와 이것이 프로세스 외부 확장성의 중요한 측면인 이유에 대해 알아봅니다.
landing-page-description: App Builder가 무엇이며 Adobe Commerce 개발 전략에 어떻게 도움이 되는지 알아봅니다.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: 82ccecf2789e1eedf447af2705a3840d0302fdba
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 0%

---


# App Builder 소개

지금까지 Adobe Commerce 개발에서는 처리 중 확장성을 사용했습니다. 처리 중인 모델에는 업그레이드, 서버의 PHP 버전 및 Commerce에서 사용하는 기타 필수 서버 응용 프로그램 및 서비스와 호환될 수 있는 새 코드가 필요합니다. Adobe Developer App Builder는 이러한 호환성 문제를 방지하기 위해 프로세스 외부 확장성을 사용합니다.

## Adobe Commerce용 App Builder {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder는 Adobe 솔루션을 확장하기 위해 사용자 지정 경험을 통합하고 만들기 위한 서버리스 확장성 플랫폼이며, 이제 Adobe Commerce에서 사용할 수 있습니다. App Builder를 사용하면 Commerce 기본 기능을 확장하고 타사 솔루션과 통합하는 안전하고 확장 가능한 앱을 빌드할 수 있습니다. 이제 개발자로서 Adobe Commerce을 통해 프로세스 외부 확장성을 활용할 수 있으며, 이를 통해 즉각적이고 장기적인 이점을 제공합니다.

App Builder는 확장된 사용자 정의 애플리케이션을 통합하고 생성할 수 있는 통합된 타사 확장성 프레임워크를 제공합니다 [!DNL Adobe Commerce]. 이 확장성 프레임워크는 Adobe의 인프라를 기반으로 구축되기 때문에 개발자는 맞춤형 마이크로서비스를 구축하고 확장 및 통합할 수 있습니다 [!DNL Adobe Commerce] 다른 Adobe 솔루션 및 서드파티 통합 전반에서 사용할 수 있습니다.

App Builder는 고객이 확장할 수 있는 방법을 제공합니다 [!DNL Adobe Commerce] 다양한 사용 사례에서:

* 미들웨어 확장성 - 사용자 정의 커넥터를 구축하여 외부 시스템을 Adobe 애플리케이션과 연결하거나 사전 구축된 통합 세트를 활용할 수 있습니다.
* 핵심 서비스 확장성 - 사용자 정의 기능 및 비즈니스 논리를 사용하여 기본 동작을 확장하여 핵심 애플리케이션 기능을 확장합니다.
* 사용자 경험 확장성 - 핵심 경험을 확장하여 비즈니스 요구 사항을 지원하거나 고객별 디지털 속성, 상점 및 백오피스 애플리케이션을 구축할 수 있습니다.

App Builder(이전 이름: Project Firefly)는 클라우드 기반 솔루션으로서 자동으로 확장됩니다. 또한 이 서비스는 지리적 위치에 관계없이 최고의 성능을 제공하기 위해 전역으로 배포됩니다.

## App Builder에 대해 자세히 알아봐야 하는 이유

Adobe Commerce은 완전한 SAAS 제품이 아니기 때문에 개발하는 코드는 복잡성과 업그레이드 문제를 추가할 수 있습니다. App Builder와 같은 프로세스 외부 확장성을 사용하여, 프로세스 내 메서드 없이 Adobe Commerce 스토어에 고유한 사용자 지정 기능을 제공할 수 있습니다.

기타 이점은 다음과 같습니다.

* 분리된 기능을 사용하면 론치 시간을 단축할 수 있습니다.
* 이제 쉽게 업그레이드할 수 있습니다. 사용자 지정 기능은 Commerce 코드 베이스의 외부에 있으므로 업그레이드 시 호환성 문제를 방지할 수 있습니다.
* Commerce 외부로 기능 및 논리를 이동하면 일반적으로 In-Process 개발 메서드에서 사용하는 리소스가 확보됩니다.

## 아키텍처 {#architecture}

Adobe Developer App Builder는 기본 솔루션 대신 Adobe Commerce과 같은 Adobe 클라우드 솔루션을 확장하기 위해 공통적이고, 일관되고, 표준화된 개발 플랫폼을 제공합니다.

* 사용자 지정 마이크로서비스 및 확장 개발에 사용되는 Adobe Developer 콘솔. 플러그인 및 통합을 만드는 데 필요한 모든 도구 및 API에 액세스하는 동안 프로젝트를 빌드하고 관리합니다.
* 사용자 지정 확장 및 통합을 구축할 수 있는 오픈 소스 도구, SDK 및 라이브러리입니다. React Spectrum(Adobe의 UI 툴킷)을 사용하여 모든 Adobe 앱에 대해 하나의 공통 UI를 가질 수 있습니다.
* Adobe의 서버리스 플랫폼에서 인프라를 호스팅하기 위한 I/O Runtime과 이벤트 기반 통합을 위한 I/O Events 등의 서비스. Adobe은 또한 데이터 및 파일 저장을 위한 기본 지원을 제공합니다.
* Adobe Experience Cloud Experience Cloud 를 사용하십시오. 시스템 관리자는 이러한 확장을 검토, 관리 및 승인할 수 있습니다. 게시되면 사용자 지정 App Builder 확장 및 도구를 다른 Adobe Experience Cloud 앱과 함께 사용할 수 있습니다.

다음 다이어그램은 App Builder에 구축된 표준 애플리케이션이 이러한 기능을 사용하는 방법을 보여 줍니다.

![아키텍처](/help/assets/app-builder/firefly-architecture.jpeg)

App Builder 아키텍처에 대한 자세한 내용은 [아키텍처 개요](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Amazon Sales Channel 확장 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel 확장 기능은 아직 개발 중이며 공식적으로 출시되지 않았습니다.  이러한 비디오 및 튜토리얼은 실용적인 사용 사례를 위해 Adobe Developer App Builder를 사용하는 방법을 보여 주기 위한 것입니다.

다음 자습서에서는 App Builder 확장을 사용하여 Adobe Commerce을 Amazon Sales Channel에 연결하는 방법을 보여줍니다.

* [기술 개요 App Builder](../app-builder/app-builder-technical-overview.md)
* [확장성 프레임워크](../app-builder/extensibility-framework-commerce-eventing.md)
* [기능 데모 App Builder](../app-builder/app-builder-functional-demonstration.md)

## App Builder 시작 {#additional-resources}

초기 설정을 포함하는 구성 가능한 상거래 전략에 대한 개요는 다음 블로그 게시물을 통해 확인할 수 있습니다.

[App Builder가 상거래 플랫폼의 비즈니스 민첩성을 높이는 데 어떻게 도움이 됩니까?](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builder를 시작하는 데 도움이 되도록 Adobe에서 다음 설명서를 만들었습니다.

* [App Builder 시작](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 설명서를 사용하여 학습 계속 {#appbuilder-documentation}

App Builder는 사용자 정의 애플리케이션을 개발하는 데 도움이 되는 안내서 및 참조 설명서를 포함하여 개발자를 위한 비디오 및 설명서를 제공합니다.

* [App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 샘플 애플리케이션 중 하나를 사용해 보십시오. {#appbuilder-codesamples}

개발을 시작할 준비가 되셨습니까? 다음 링크에는 시작하는 데 도움이 되는 샘플 응용 프로그램이 포함되어 있습니다.

* [Adobe Developer 웹 사이트의 App Builder 코드 랩](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 지원 {#support}

개발자 지원 요청의 경우 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 도움이 필요하신가요?

{{$include /help/_includes/app-builder-related-links.md}}
