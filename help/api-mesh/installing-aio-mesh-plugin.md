---
title: Adobe Developer IO 명령줄 인터페이스 및 API Mesh 플러그인 설치
description: Adobe Developer IO 명령줄 인터페이스 및 API Mesh 플러그인을 설치하는 방법을 알아봅니다
landing-page-description: App Builder Adobe을 사용하고 API Mesh 플러그인을 사용하여 Adobe Developer IO를 설치하는 방법을 알아봅니다.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Adobe Developer IO 및 Mesh 플러그인 설치

시작하기 전에 설정해야 할 몇 가지 사항이 있습니다. 먼저 Adobe Developer IO 명령줄 인터페이스 설정입니다. 다음으로, API Mesh 플러그인이 각 환경에 구성되어 있는지 확인합니다.
노드, nvm 및 Adobe Developer IO 설치를 위한 로컬 환경 설정에 대한 지침은 를 반드시 방문하십시오. [GraphQL Mesh 시작](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## 이 비디오 누구?

* 앱 빌더 Adobe을 처음 사용하는 개발자 또는 [!DNL Magento Open Source] Adobe Developer IO 및 API Mesh를 사용한 제한된 경험 제공.

## 비디오 컨텐츠

* API Mesh 소개
* Adobe Developer IO 명령줄 인터페이스 설치
* AIO 명령줄에 API Mesh 플러그인 추가

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## NPM 및 AIO를 사용하는 명령 예

Adobe Developer 명령줄 인터페이스를 설치하는 것은 훨씬 간단합니다. 노드를 설치한 후 이 명령을 실행합니다 `npm install -g @adobe/aio-cli`
Adobe Developer cli가 설치되면 mesh 플러그인을 설치할 수 있습니다. 이 명령을 실행하면 됩니다 `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
