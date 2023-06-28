---
title: Adobe I/O Runtime 명령줄 인터페이스 및 API Mesh 플러그인 설치
description: Adobe I/O Runtime 명령줄 인터페이스와 API Mesh 플러그인을 설치하는 방법을 알아봅니다
landing-page-description: Adobe 앱 빌더를 사용하고 API Mesh 플러그인을 사용하여 Adobe I/O Runtime을 설치하는 방법을 알아봅니다.
short-description: Adobe 앱 빌더를 사용하고 API Mesh 플러그인을 사용하여 Adobe I/O Runtime을 설치하는 방법을 알아봅니다.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 0%

---

# Adobe I/O Runtime CLI 및 Mesh 플러그인 설치

Adobe Developer App Builder용 API Mesh를 사용하기 전에 `aio` CLI 및 API Mesh 플러그인.
설치 지침 및 사전 요구 사항은 API Mesh 를 참조하십시오. [시작](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 페이지를 가리키도록 업데이트하는 중입니다.

## 이 비디오는 누구의 것입니까?

* API Mesh 또는 를 처음 사용하는 개발자 [!DNL Adobe Commerce] 을 사용하는 제한된 경험 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 및 API Mesh.

## 비디오 콘텐츠

* API Mesh 소개
* Adobe I/O Runtime CLI(명령줄 인터페이스) 설치
* API Mesh 플러그인 설치

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## 설치 `aio` CLI 및 API Mesh 플러그인

설치 후 `node` 및 `npm`를 클릭하고 다음 명령을 실행하여 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI가 설치되면 다음 명령을 사용하여 API Mesh 플러그인을 설치합니다.

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
