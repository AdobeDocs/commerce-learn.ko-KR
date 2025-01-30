---
title: 주요 폴더 및 자동화 스크립트가 포함된 Commerce Integration Starter Kit에 대해 알아봅니다.
description: Commerce 통합 시작 키트의 소스 코드 구성에 대해 알아봅니다. ​
landing-page-description: Commerce 통합 시작 키트에서 Source 코드 조직 살펴보기
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: 6c5017b0c4bbafdd143b78b05cd92853efa7f831
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Adobe 시작 키트에 대한 Source 코드 조직

Adobe Commerce 통합 시작 키트 내의 소스 코드 조직에 대해 알아봅니다&#x200B;. `actions` 및 `scripts`과(와) 같은 주요 폴더와 해당 콘텐츠를 강조 표시하여 프로젝트의 구조를 살펴봅니다&#x200B;. &#39;actions&#39; 폴더에는 이벤트 처리 및 추적에 필요한 코드를 포함하는 `ingestion` 및 `webhook`과(와) 같은 하위 폴더가 있습니다. `starter-kit-info` 및 `scripts` 폴더도 학습합니다. `scripts` 폴더는 프로젝트 내의 이벤트 구성 및 공급자 설정을 간소화하는 `commerce-event-subscribe` 및 `onboarding`과(와) 같은 자동화 스크립트에 중점을 둡니다.
&#x200B;
각 엔터티 폴더 아래의 `commerce` 및 `external` 폴더가 다른 시스템에서 시작된 이벤트를 처리하는 방법을 자세히 설명하면서 원본 코드 구조 뒤의 논리를 살펴보십시오. 이 비디오에서는 이벤트를 적절한 이벤트 핸들러 런타임 작업에 발송하는 `consumer` 폴더의 역할에 대해 설명하고 원활한 처리를 보장합니다. 또한 이 비디오는 실패 이벤트를 효과적으로 처리하기 위해 런타임 작업에 구현된 재시도 메커니즘에 대해서도 다룹니다. &#x200B;Adobe Commerce 통합 시작 키트에서 소스 코드의 조직과 기능을 이해하여 이벤트 처리, 자동화 스크립트 및 구성 설정에 대한 중요한 통찰력을 제공합니다.

## 대상자

* 소스 코드가 `actions` 및 `scripts`과(와) 같은 주요 폴더로 구성되는 방식을 이해하려는 개발자입니다.
* `actions` 폴더에 이벤트 처리 및 배포 추적을 위한 필수 코드가 포함된 `ingestion` 및` webhook`과(와) 같은 하위 폴더가 있습니다.
* `customer`, `order`, `product` 및 `stock`과(와) 같은 엔터티의 폴더가 포함된 `actions` 폴더에 대해 알아보고자 하는 개발자입니다.

## 비디오 콘텐츠

* 세션 중에 `actions` 및 `scripts` 폴더에 중점을 둔 네 개의 기본 폴더(`actions`, `scripts`, `test` 및 `utils`)를 이해합니다. &#x200B;
* `actions` 폴더 및 `ingestion` 및 `webhook`과(와) 같은 중요한 하위 폴더를 포함하는 방법에 대해 알아봅니다.
* `actions` 폴더와 `customer`, `order`, `product`, `stock` 같은 엔터티에 대해 특정 폴더가 있는 이유를 살펴보십시오. 각 폴더에는 Commerce 및 타사 시스템의 이벤트를 효과적으로 관리하기 위해 `commerce` 및 `external` 폴더로 구성된 런타임 작업이 포함되어 있습니다. &#x200B;
* 스타터 키트를 기반으로 프로젝트 배포를 추적하기 위해 Adobe에서 사용하는 런타임 작업이 포함된 `starter-kit-info` 폴더에서 코드를 변경하지 않는 것의 중요성에 대해 알아봅니다. &#x200B;
* 이벤트 구성, 공급자 설정 및 Commerce의 Adobe I/O Events 모듈 구성을 자동화하는 `commerce-event-subscribe` 및 `onboarding`과(와) 같은 자동화 스크립트가 포함된 `scripts` 폴더를 이해합니다. &#x200B;

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
