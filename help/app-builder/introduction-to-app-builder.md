---
title: Adobe Commerce의 프로세스 부족 확장성
description: Adobe App Builder와 프로세스가 아닌 확장성 중 중요한 측면에 대해 알아보십시오.
landing-page-description: App Builder가 무엇이고 Adobe Commerce 개발 전략에 이 기능을 사용할 수 있는지 알아봅니다.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: 021df5e5f98341204e9cc486c249dcd87fab2aa3
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---


# App Builder 소개

지금까지 Adobe Commerce 개발에서는 프로세스 내 확장성을 사용했습니다. 인프로세스 모델은 업그레이드, 서버의 PHP 버전, Commerce에서 사용하는 기타 필수 서버 응용 프로그램 및 서비스와 호환되도록 새 코드를 필요로 합니다. Adobe Developer App Builder는 이러한 호환성 문제를 방지하기 위해 프로세스 외부의 확장성을 사용합니다.

## Adobe Commerce용 App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder는 Adobe 솔루션을 확장하기 위해 사용자 지정 경험을 통합하고 만드는 서버를 사용하지 않는 확장성 플랫폼이며 이제 Adobe Commerce에서 사용할 수 있습니다. App Builder를 사용하면 상거래 기본 기능을 확장하고 타사 솔루션과 통합하는 안전하고 확장 가능한 앱을 빌드할 수 있습니다. 개발자는 이제 Adobe Commerce을 통해 프로세스 외부 확장성을 활용할 수 있으므로 이를 통해 즉각적이고 장기적인 이점을 제공합니다.

App Builder는 확장 가능한 사용자 지정 애플리케이션을 통합 및 생성하기 위한 통합 타사 확장성 프레임워크를 제공합니다 [!DNL Adobe Commerce]. 이 확장성 프레임워크는 Adobe의 인프라를 기반으로 구축되므로 개발자는 맞춤형 마이크로 서비스를 구축하고 확장 및 통합할 수 있습니다 [!DNL Adobe Commerce] 다른 Adobe 솔루션 및 타사 통합 간에 연결할 수 있습니다.

App Builder 는 고객이 확장할 수 있는 방법을 제공합니다 [!DNL Adobe Commerce] 다양한 사용 사례에서

* 미들웨어 확장성 - 사용자 정의 커넥터를 빌드하여 외부 시스템을 Adobe 애플리케이션과 연결하거나 사전 빌드된 통합 세트를 활용할 수 있습니다.
* 핵심 서비스 확장성 - 사용자 정의 기능 및 비즈니스 논리를 사용하여 기본 동작을 확장하여 코어 애플리케이션 기능을 확장합니다.
* 사용자 경험 확장성 - 핵심 경험을 확장하여 비즈니스 요구 사항을 지원하거나 고객별 디지털 속성, 상점 및 백오피스 애플리케이션을 구축합니다.

Adobe Developer App Builder는 클라우드 기반 솔루션으로서, 자동으로 크기가 조절됩니다. 또한 이 서비스는 지리적 위치에 상관없이 최고의 성능을 제공하도록 전체적으로 배포됩니다.

## App Builder에 대해 자세히 알고 있어야 하는 이유

Adobe Commerce은 완전히 SAAS 제품이 아니므로 개발되는 코드는 복잡성과 업그레이드 문제를 추가할 수 있습니다. App Builder와 같은 프로세스 내 확장성을 사용하여 in-process 메서드를 사용하지 않고도 Adobe Commerce 스토어에 사용자 지정 고유한 기능을 제공할 수 있습니다.

기타 이점은 다음과 같습니다.

* 분리된 기능을 사용하면 시작 시간이 더 빨라집니다.
* 이제 업그레이드가 더 쉬워졌습니다. 사용자 지정 기능은 Commerce Codebase 외부에 있으며, 업그레이드할 때 호환성 문제가 발생하지 않습니다.
* Commerce 외부에서 기능 및 논리를 이동하면 프로세스 내 개발 방법으로 일반적으로 사용되는 리소스가 해제됩니다.

## 아키텍처 {#architecture}

Adobe Developer App Builder는 기본 솔루션 대신 다음을 포함하여 Adobe Commerce과 같은 Adobe 클라우드 솔루션을 확장하는 공통, 일관성 및 표준화된 개발 플랫폼을 제공합니다.

* 사용자 정의 마이크로 서비스 및 확장 개발에 사용되는 Adobe Developer 콘솔. 플러그인 및 통합을 만드는 데 필요한 모든 도구 및 API에 액세스하면서 프로젝트를 빌드하고 관리합니다.
* 오픈 소스 도구, SDK 및 라이브러리를 사용하여 사용자 지정 확장 및 통합을 구축할 수 있습니다. 모든 Adobe 앱에 대해 하나의 공통 UI를 갖도록 React Spectrum(Adobe의 UI 툴킷)을 사용할 수 있습니다.
* Adobe의 서버리스 플랫폼에서 인프라를 호스팅하기 위한 I/O 런타임 및 이벤트 기반 통합을 위한 I/O 이벤트 등의 서비스. 또한 Adobe은 데이터 및 파일 저장을 위한 기본 지원을 제공합니다.
* Adobe Experience Cloud에서 Experience Cloud 조직에 게시할 확장 및 통합을 제출할 수 있습니다. 시스템 관리자는 이러한 확장을 검토, 관리 및 승인할 수 있습니다. 게시되면 사용자 지정 App Builder 확장 및 도구를 다른 Adobe Experience Cloud 앱과 함께 사용할 수 있습니다.

다음 다이어그램은 App Builder를 기반으로 구축된 표준 애플리케이션에서 이러한 기능을 사용하는 방법을 보여줍니다.

![아키텍처](/help/assets/app-builder/app-builder-architecture.jpeg)

App Builder 아키텍처에 대한 자세한 내용은 [아키텍처 개요](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Amazon Sales Channel 확장 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel 확장은 아직 개발 중이며 공식적으로 출시되지 않았습니다.  이 비디오와 자습서는 실제 사용 사례에 Adobe Developer App Builder를 사용하는 방법을 보여주기 위한 것입니다.

다음 자습서에서는 앱 빌더 확장을 사용하여 Adobe Commerce을 Amazon Sales Channel에 연결하는 방법을 보여줍니다.

* [기술 개요 App Builder](../app-builder/app-builder-technical-overview.md)
* [확장성 프레임워크](../app-builder/extensibility-framework-commerce-eventing.md)
* [기능 데모 앱 빌더](../app-builder/app-builder-functional-demonstration.md)

## App Builder 시작 {#additional-resources}

다음 블로그 게시물을 읽고 초기 설정을 포함하는 복합 가능한 상거래 전략의 개요를 찾을 수 있습니다.

[App Builder를 통해 전자 상거래 플랫폼에 대한 비즈니스 민첩성을 향상시키는 방법](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builder를 시작할 수 있도록 Adobe에서 다음 설명서를 만들었습니다.

* [앱 빌더 시작](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 설명서 학습 계속 {#appbuilder-documentation}

App Builder는 사용자 정의 애플리케이션을 개발하는 데 도움이 되는 안내서와 참조 설명서 를 포함하여 개발자를 위한 비디오 및 설명서를 제공합니다.

* [App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [App Builder 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 샘플 응용 프로그램 중 하나를 사용해 보십시오 {#appbuilder-codesamples}

개발을 시작할 준비가 되었습니까? 다음 링크에는 시작하는 데 도움이 되는 샘플 응용 프로그램이 포함되어 있습니다.

* [Adobe Developer 웹 사이트의 App Builder 코드 Labs](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 지원 {#support}

개발자 지원 요청의 경우 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 지원 요청.

{{$include /help/_includes/app-builder-related-links.md}}
