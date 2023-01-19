---
title: Adobe Commerce의 프로세스 부족 확장성
description: Adobe App Builder와 프로세스가 아닌 확장성 중 중요한 측면에 대해 알아보십시오.
landing-page-description: 앱 빌더란 무엇이며 Adobe Commerce 개발 전략에 도움이 되는 방법을 알아봅니다.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-11T00:00:00Z
source-git-commit: ef0fa95e776b97ddbaf30e0acd1340e30f12738f
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 0%

---


# 프로세스 외부의 확장성

Adobe Commerce 개발은 지금까지 기본 애플리케이션과 동일한 저장소를 사용하여 수행되었습니다.  이를 인프로세스라고 합니다.  이 기법은 매우 유용하며 개발자에게 애플리케이션을 확장하는 데 필요한 예상 메커니즘을 제공합니다.  하지만, 이것은 가격입니다.  코드 베이스에 새 코드를 추가할 때마다 모든 업그레이드와 호환되어야 합니다.  또한 상거래에 사용될 다른 많은 서버 응용 프로그램 및 서비스뿐만 아니라 서버 PHP 버전과 호환되어야 합니다.  Adobe Developer App Builder는 기능을 확장하는 동일한 요구 사항을 적용하지만 사이트 외부로 이동합니다.  코드 및 논리는 완전히 외부적이며 이 메서드를 프로세스 외부에서 참조합니다.

## Adobe Commerce용 App Builder {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder는 개발자가 확장할 수 있는 확장성 프레임워크를 제공합니다 [!DNL Adobe Commerce] 프로세스 외부의 확장성을 제공합니다.

App Builder는 확장 가능한 사용자 지정 애플리케이션을 통합 및 생성하기 위한 통합 타사 확장성 프레임워크를 제공합니다 [!DNL Adobe Commerce]. 이 확장성 프레임워크는 Adobe의 인프라를 기반으로 구축되므로 개발자는 맞춤형 마이크로 서비스를 빌드하고 확장 및 통합할 수 있습니다 [!DNL Adobe Commerce] Adobe 솔루션 및 기타 타사 통합 간에 연결할 수 있습니다.

App Builder 는 고객이 확장할 수 있는 방법을 제공합니다 [!DNL Adobe Commerce] 다양한 사용 사례에서

* 미들웨어 확장성 - 사용자 정의 커넥터를 빌드하여 외부 시스템을 Adobe 애플리케이션과 연결하거나 사전 빌드된 통합 세트를 활용할 수 있습니다.
* 핵심 서비스 확장성 - 사용자 정의 기능 및 비즈니스 논리를 사용하여 기본 동작을 확장하여 코어 애플리케이션 기능을 확장합니다.
* 사용자 경험 확장성 - 핵심 경험을 확장하여 비즈니스 요구 사항을 지원하거나 고객별 디지털 속성, 상점 및 백오피스 애플리케이션을 구축합니다.

앱 빌더 (이전에 Project Firefly라고 함)는 클라우드 기반 솔루션으로서, 자동으로 크기가 조절됩니다. 또한 이 서비스는 지리적 위치에 상관없이 최고의 성능을 제공하도록 전체적으로 배포됩니다.

## App Builder에 대해 자세히 알고 있어야 하는 이유

Adobe Commerce은 완전히 SAAS가 아니므로 개발하거나 설치하는 코드는 복잡성과 업그레이드 문제를 추가할 수 있습니다. App Builder와 같은 프로세스 내 확장성을 사용하여 in-process 메서드를 사용하지 않고도 Adobe Commerce 스토어에 사용자 지정 고유한 기능을 제공할 수 있습니다.

기타 이점은 다음과 같습니다.

* 분리된 기능을 사용하면 시작 시간이 더 빨라집니다.
* 이제 업그레이드가 더 쉬워졌습니다. 사용자 지정 기능은 커머스 코드 베이스의 외부에 있으므로 업그레이드할 때 호환성 문제가 발생하지 않습니다.
* 상거래 외부에서 기능과 로직을 이동하면 프로세스 내 개발 방법에서 일반적으로 사용되는 리소스가 확보됩니다.

## 아키텍처 {#architecture}

Adobe Developer App Builder는 기본 솔루션 대신 다음을 포함하여 Adobe Commerce과 같은 Adobe 클라우드 솔루션을 확장하는 공통, 일관성 및 표준화된 개발 플랫폼을 제공합니다.

* 사용자 정의 마이크로 서비스 및 확장 개발에 사용되는 Adobe Developer 콘솔. 플러그인 및 통합을 만드는 데 필요한 모든 도구 및 API에 액세스하면서 프로젝트를 빌드하고 관리합니다.
* 오픈 소스 도구, SDK 및 라이브러리를 사용하여 사용자 지정 확장 및 통합을 구축할 수 있습니다. 모든 Adobe 앱에 대해 하나의 공통 UI를 갖도록 React Spectrum(Adobe의 UI 툴킷)을 사용할 수 있습니다.
* Adobe의 서버리스 플랫폼에서 인프라를 호스팅하기 위한 I/O 런타임 및 이벤트 기반 통합을 위한 I/O 이벤트 등의 서비스. 또한 Adobe은 데이터 및 파일 저장을 위한 기본 지원을 제공합니다.
* Adobe Experience Cloud에서 Experience Cloud 조직에 게시할 확장 및 통합을 제출할 수 있습니다. 시스템 관리자는 이러한 확장을 검토, 관리 및 승인할 수 있습니다. 게시되면 사용자 지정 App Builder 확장 및 도구를 다른 Adobe Experience Cloud 앱과 함께 사용할 수 있습니다.

다음 다이어그램은 App Builder를 기반으로 구축된 표준 애플리케이션에서 이러한 기능을 사용하는 방법을 보여줍니다.

![아키텍처](/help/assets/app-builder/firefly-architecture.jpeg)

App Builder 아키텍처에 대한 자세한 내용은 [아키텍처 개요](https://developer.adobe.com/app-builder/docs/guides/).

## App Builder 시작 {#additional-resources}

App Builder를 시작할 수 있도록 Adobe에서 다음 설명서를 만들었습니다.

* [앱 빌더 시작](https://developer.adobe.com/app-builder/docs/getting_started/)

## 설명서 학습 계속 {#appbuilder-documentation}

App Builder는 사용자 정의 애플리케이션을 개발하는 데 도움이 되는 안내서와 참조 설명서 를 포함하여 개발자를 위한 비디오 및 설명서를 제공합니다.

* [App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/)
* [App Builder 비디오](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## 샘플 응용 프로그램 중 하나를 사용해 보십시오 {#appbuilder-codesamples}

개발을 시작할 준비가 되었습니까? 다음 링크에는 시작하는 데 도움이 되는 샘플 응용 프로그램이 포함되어 있습니다.

* [Adobe Developer 웹 사이트의 App Builder 코드 Labs](https://developer.adobe.com/app-builder/docs/resources/)

## 지원 {#support}

개발자 지원 요청의 경우 [Experience League 포럼](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) 지원 요청.
