---
title: 첫 번째 App Builder 앱 만들기
description: 첫 번째 프로세스 외부 애플리케이션 구축에 필요한 사전 요구 사항, 기대 사항 및 재사용 가능한 패턴을 포함하여 Adobe Commerce을 사용한 Adobe Developer App Builder에 대해 알아봅니다.
jira: KT-21679
doc-type: Tutorial
duration: 197
last-substantial-update: 2023-03-13T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 0b6a91dd-e5c4-4ead-84d4-362de070815e
TQID: https://experienceleague.adobe.com/vaWPlxMkONIlEhq4-WEjw8wKWBaBb1iYmeOPSjsnnjk
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: e03f0a058d1a08b1a67fd278c1b6127566a370ac
workflow-type: tm+mt
source-wordcount: 332
ht-degree: 0%

---

# 개요 {#overview}

이 자습서에는 다음 사전 요구 사항이 포함되어 있습니다.

* Adobe Developer Console 액세스가 완료되었습니다.
* App Builder에 대한 전체 액세스 권한 또는 체험판 액세스 권한이 부여되었습니다.
* [Adobe Developer App Builder 애플리케이션이 생성되었습니다.](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app){target="_blank"}
* [Adobe Developer App Builder 프로젝트가 생성되었습니다.](https://developer.adobe.com/console){target="_blank"}
* [Adobe Developer App Builder 작업 영역이 생성되었습니다. - 2.6단계](https://developer.adobe.com/app-builder/docs/get_started/app_builder_get_started/first-app#2-creating-a-new-project-on-developer-console){target="_blank"}
* [프로젝트를 초기화하고 실행하는 AIO CLI 명령이 실행되었습니다.](https://developer.adobe.com/runtime){target="_blank"}

첫 번째 App Builder 응용 프로그램 빌드에 대한 자세한 내용을 보려면 다음과 같은 블로그 게시물을 보고 초기 설정 및 구성에 도움을 받으십시오. [App Builder이 상거래 플랫폼을 위한 비즈니스 민첩성을 높이는 데 어떻게 도움이 됩니까](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}.

## 이 자습서를 읽고 예상할 사항

이 자습서를 완료하면 Adobe Commerce과 통신하여 주문 내역 및 제품을 가져오는 간단한 단일 페이지 애플리케이션이 있어야 합니다. 이 항목에서는 코드 샘플 및 지침과 함께 App Builder 기능을 다룹니다. 이러한 비디오를 시청하면 개발 시간이 절약되고 다른 아이디어를 영감을 얻으며 프로세스 외부 개발을 채택할 수 있습니다.

## 이 자습서를 따르는 방법

이 튜토리얼은 왼쪽 탐색에서 페이지 순서를 따르도록 설계되었습니다. 그러나 이 주문은 필수가 아닙니다. 초기 Adobe Developer App Builder 앱을 빌드하는 일반적인 개념에 대해 논의하므로 각 페이지는 개별적으로 볼 수 있습니다.

## 이 비디오는 누구의 것입니까?

* Adobe App Builder을 사용하는 경험이 제한된 Adobe Commerce을 처음 사용하는 개발자입니다.

## 비디오 콘텐츠

* App Builder 및 샘플 모듈 소개
* 사전 요구 사항
* 샘플 모듈 사용에 대한 기대치
* 샘플 모듈의 재사용 가능한 부분

>[!VIDEO](https://video.tv.adobe.com/v/3421027?captions=kor&learn=on)

{{avoid-400-error}}

{{$include /help/_includes/app-builder-first-app-related-links.md}}
