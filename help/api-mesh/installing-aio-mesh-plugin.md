---
title: Adobe I/O Runtime CLI 및 API Mesh 플러그인 설치
description: Adobe Developer App Builder용 API Mesh를 시작하기 위해 Adobe I/O Runtime 명령줄 인터페이스와 API Mesh 플러그인을 설치하는 방법을 알아봅니다.
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%
---
# Adobe I/O Runtime CLI 및 Mesh 플러그인 설치

Adobe Developer App Builder용 API Mesh 사용을 시작하려면 먼저 `aio` CLI 및 API Mesh 플러그인을 설치해야 합니다.
설치 지침 및 필수 구성 요소를 보려면 API Mesh [시작](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"} 페이지를 방문하십시오.

## 이 비디오는 누구의 것입니까?

* [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} 및 API Mesh를 사용하여 경험이 제한된 API Mesh 또는 [!DNL Adobe Commerce]을(를) 처음 사용하는 개발자입니다.

## 비디오 콘텐츠

* API Mesh 소개
* Adobe I/O Runtime CLI(명령줄 인터페이스) 설치
* API Mesh 플러그인 설치

>[!VIDEO](https://video.tv.adobe.com/v/3414122?learn=on)

## `aio` CLI 및 API Mesh 플러그인 설치 중

`aio` CLI를 설치하려면 `node` 및 `npm`을(를) 설치한 후 다음 명령을 실행하십시오.

```bash
npm install -g @adobe/aio-cli
```

Adobe I/O Runtime CLI가 설치되면 다음 명령을 사용하여 API Mesh 플러그인을 설치합니다.

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
