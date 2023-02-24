---
title: Adobe I/O Runtime 명령줄 인터페이스 및 API Mesh 플러그인 설치
description: Adobe I/O Runtime 명령줄 인터페이스 및 API Mesh 플러그인을 설치하는 방법을 알아봅니다
landing-page-description: App Builder Adobe을 사용하고 API Mesh 플러그인을 사용하여 Adobe I/O Runtime을 설치하는 방법을 알아봅니다.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: f365e4cbf3f9bd8a0364acb9d28f8d9479809011
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Adobe I/O Runtime CLI 및 Mesh 플러그인 설치

Adobe Developer App Builder용 API Mesh를 사용하기 전에 `aio` CLI 및 API Mesh 플러그인입니다.
설치 지침 및 사전 요구 사항을 알려면 API Mesh 를 참조하십시오 [시작하기](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} 페이지.

## 이 비디오 누구?

* API Mesh를 처음 사용하는 개발자 또는 [!DNL Adobe Commerce] 사용 경험이 제한되어 있음 [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} 및 API Mesh.

## 비디오 컨텐츠

* API Mesh 소개
* Adobe I/O Runtime CLI 설치(명령줄 인터페이스)
* API Mesh 플러그인 설치

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## 설치 `aio` CLI 및 API Mesh 플러그인

설치 후 `node` 및 `npm`다음 명령을 실행하여 `aio` CLI:

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI가 설치되면 다음 명령을 사용하여 API Mesh 플러그인을 설치합니다.

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
