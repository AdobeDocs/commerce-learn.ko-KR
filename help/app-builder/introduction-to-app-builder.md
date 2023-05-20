---
title: Adobe Commerce을 위한 프로세스 외부 확장성
description: Adobe App Builder와 이것이 프로세스 외부 확장성의 중요한 측면인 이유에 대해 알아봅니다.
landing-page-description: App Builder가 무엇이며 Adobe Commerce 개발 전략에 어떻게 도움이 되는지 알아봅니다.
short-description: 앱 빌더 소개 및 Adobe Systems 상거래 개발 전략에 어떻게 도움이 될 수 있는지 알아보십시오.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 0%

---

# 앱 빌더 소개

지금까지 Adobe Commerce 개발에서는 처리 중 확장성을 사용했습니다. 처리 중인 모델에는 업그레이드, 서버의 PHP 버전 및 Commerce에서 사용하는 기타 필수 서버 응용 프로그램 및 서비스와 호환될 수 있는 새 코드가 필요합니다. Adobe Developer App Builder는 이러한 호환성 문제를 방지하기 위해 프로세스 외부 확장성을 사용합니다.

## Adobe Commerce용 App Builder {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder는 Adobe 솔루션을 확장하기 위해 사용자 지정 경험을 통합하고 만들기 위한 서버리스 확장성 플랫폼이며, 이제 Adobe Commerce에서 사용할 수 있습니다. 앱 빌더를 사용 하 여 기본 기능 및 통합 대상 서드파티 솔루션을 확장 하는 안전 하 고 확장 가능한 앱을 빌드 수 있습니다. 개발자는 이제 Adobe Systems 상거래를 사용 하 여 out-of-process 확장성을 활용 하 고 즉각적이 고 장기적인 혜택을 제공할 수 있습니다.

앱 빌더는 확장 [!DNL Adobe Commerce] 된 사용자 지정 애플리케이션을 통합 하 고 만들기 위한 통합 서드파티 확장 프레임 워크를 제공 합니다. 이 확장성 프레임 워크는 Adobe Systems 인프라에서 작성 되므로 개발자는 사용자 지정 마이크로 서비스를 빌드 하 고 다른 Adobe Systems 솔루션 및 서드파티 통합을 확장 하 고 통합할 [!DNL Adobe Commerce] 수 있습니다.

앱 빌더는 고객이 다양 한 사용 사례를 확장할 [!DNL Adobe Commerce] 수 있는 방법을 제공 합니다.

* 미들웨어 확장성-사용자 지정 커넥터를 만들거나 사전 설치 통합 세트를 활용 하 여 Adobe Systems 애플리케이션을 사용 하 여 외부 시스템을 연결 합니다.
* 핵심 서비스 확장성-사용자 지정 기능 및 비즈니스 로직 기본 동작을 확장 하 여 핵심 애플리케이션 기능을 확장 합니다.
* 사용자 경험 확장성 - 핵심 경험을 확장하여 비즈니스 요구 사항을 지원하거나 고객별 디지털 속성, 상점 및 백오피스 애플리케이션을 구축할 수 있습니다.

Adobe Developer App Builder는 클라우드 기반 솔루션으로, 자동으로 크기가 조절됩니다. 또한 이 서비스는 지리적 위치에 관계없이 최고의 성능을 제공하기 위해 전역으로 배포됩니다.

## App Builder에 대해 자세히 알아봐야 하는 이유

Adobe Commerce은 완전한 SAAS 제품이 아니기 때문에 개발하는 코드는 복잡성과 업그레이드 문제를 추가할 수 있습니다. App Builder와 같은 프로세스 외부 확장성을 사용하여, 프로세스 내 메서드 없이 Adobe Commerce 스토어에 고유한 사용자 지정 기능을 제공할 수 있습니다.

기타 이점은 다음과 같습니다.

* 분리된 기능을 사용하면 론치 시간을 단축할 수 있습니다.
* 이제 쉽게 업그레이드할 수 있습니다. 사용자 지정 기능은 Commerce 코드 베이스의 외부에 있으므로 업그레이드 시 호환성 문제를 방지할 수 있습니다.
* Commerce 외부로 기능 및 논리를 이동하면 일반적으로 In-Process 개발 메서드에서 사용하는 리소스가 확보됩니다.

## 아키텍처 {#architecture}

Adobe Systems 개발자 앱 빌더는 기본 솔루션 대신 다음과 같은 Adobe Systems 상거래와 같은 Adobe Systems 클라우드 솔루션을 확장 하는 데 사용할 수 있는 공통 및 표준화 된 개발 플랫폼을 제공 합니다.

* 사용자 지정 microservice 및 확장 개발에 사용 되는 Adobe Systems 개발자 콘솔. 플러그인 및 통합을 만드는 데 필요한 모든 도구와 Api에 액세스 하는 동안 프로젝트를 빌드하고 관리.
* 사용자 지정 확장 및 통합을 구축할 수 있는 오픈 소스 도구, SDK 및 라이브러리입니다. React Spectrum(Adobe의 UI 툴킷)을 사용하여 모든 Adobe 앱에 대해 하나의 공통 UI를 가질 수 있습니다.
* Adobe의 서버리스 플랫폼에서 인프라를 호스팅하기 위한 I/O Runtime과 이벤트 기반 통합을 위한 I/O Events 등의 서비스. Adobe은 또한 데이터 및 파일 저장을 위한 기본 지원을 제공합니다.
* Experience Cloud 조직에서 게시에 확장 및 통합을 제출 하는 Adobe Experience Cloud. System admins는 이러한 확장을 검토 하 고 관리 하 고 승인할 수 있습니다. 게시 된 사용자 지정 앱 빌더 확장 및 도구는 다른 Adobe Experience Cloud 앱과 함께 사용할 수 있습니다.

다음 다이어그램은 앱 빌더에서 작성 된 표준 애플리케이션 다음 기능을 사용 하는 방법을 보여줍니다.

![아키텍처](/help/assets/app-builder/app-builder-architecture.jpeg)

앱 빌더 아키텍처에 대 한 자세한 내용은 아키텍처 개요 ](https://developer.adobe.com/app-builder/docs/guides/) {target="_blank"} 를 참조 [ 하십시오.

## Amazon Sales Channel 확장 {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>Amazon Sales Channel 확장 기능은 아직 개발 중이며 공식적으로 출시되지 않았습니다.  이러한 비디오 및 튜토리얼은 실용적인 사용 사례를 위해 Adobe Developer App Builder를 사용하는 방법을 보여 주기 위한 것입니다.

다음 자습서에서는 App Builder 확장을 사용하여 Adobe Commerce을 Amazon Sales Channel에 연결하는 방법을 보여줍니다.

* [기술 개요 App Builder](../app-builder/app-builder-technical-overview.md)
* [확장성 프레임워크](../app-builder/extensibility-framework-commerce-eventing.md)
* [기능 데모 App Builder](../app-builder/app-builder-functional-demonstration.md)

## App Builder 시작 {#additional-resources}

다음 블로그 게시물를 읽고 초기 설정을 포함 하는 작성 가능한 상거래 전략에 대 한 n 개의 개요를 찾을 수 있습니다.

[App Builder가 상거래 플랫폼의 비즈니스 민첩성을 높이는 데 어떻게 도움이 됩니까?](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

App Builder를 시작하는 데 도움이 되도록 Adobe에서 다음 설명서를 만들었습니다.

* [App Builder 시작](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## 설명서를 사용하여 학습 계속 {#appbuilder-documentation}

앱 빌더는 사용자 지정 애플리케이션을 개발 하는 데 도움이 되는 안내서 및 참조 설명서를 포함 한 개발자를 위한 비디오 및 설명서를 제공 합니다.

* [앱 빌더 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [앱 빌더 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## 샘플 애플리케이션 중 하나를 사용해 보십시오. {#appbuilder-codesamples}

개발을 시작할 준비가 되셨습니까? 다음 링크에는 시작 하는 데 도움이 되는 샘플 응용 프로그램이 포함 되어 있습니다.

* [Adobe Developer 웹 사이트의 App Builder 코드 랩](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## 지원 {#support}

개발자 지원 요청의 경우 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} 도움이 필요하신가요?

{{$include /help/_includes/app-builder-related-links.md}}
