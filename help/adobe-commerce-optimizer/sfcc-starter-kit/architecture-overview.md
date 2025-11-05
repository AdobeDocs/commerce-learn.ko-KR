---
title: Salesforce Commerce Cloud 커넥터의 아키텍처 개요
description: Adobe Commerce Optimizer과 함께 Salesforce Commerce Cloud을 위한 아키텍처에 대해 알아봅니다.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 243
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: 7010657a2eb3c9e6dac9eb2ed566a8c716e02f69
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---


# Salesforce Commerce Cloud 스타터 키트 아키텍처

SFCC(Salesforce Commerce Cloud)와 Commerce Optimizer App Builder을 통합하는 Adobe Connector Starter Kit의 아키텍처 및 기능에 대해 알아봅니다. 스타터 키트는 Adobe Commerce Optimizer에서 Edge Delivery 상점에 대한 카탈로그 동기화를 간소화하는 데 사용됩니다. SFCC의 사용자 정의 카트리지가 델타 내보내기 파일을 통해 카탈로그 변경 사항을 감지하고 사용자 정의 API를 통해 노출하는 방법을 설명합니다. 이러한 변경 내용은 App Builder 런타임 작업(동기식 및 비동기식)에서 전체 및 델타 동기화, 메타데이터 업데이트 및 제품별 동기화를 수행하는 데 사용됩니다. 또한 이 시스템에는 상점 정확도를 보장하고 App Builder의 상태 관리를 사용하여 동기화 상태를 추적하고 충돌을 방지하는 유효성 검사 도구가 포함되어 있습니다.

## 이 비디오는 누구의 것입니까?

* Commerce 솔루션 설계자
* 기술 마케팅 엔지니어
* eCommerce 플랫폼 관리자

## 비디오 콘텐츠

* 사용자 지정 SFCC 카트리지 및 API는 델타 내보내기를 통해 카탈로그 변경 사항을 감지하므로 Adobe App Builder과의 효율적인 데이터 동기화를 지원합니다.
* App Builder 런타임 작업은 전체 및 델타 동기화, 유효성 검사 및 상태 추적을 관리하여 정확하고 충돌이 없는 Commerce Optimizer 업데이트를 보장합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3476058?captions=kor&learn=on)
