---
title: 최신 Adobe Commerce 구현 계획
description: 이 웨비나는 최신 상거래 시스템, 특히 Adobe Commerce의 구현을 계획하는 것이며 검색 단계, 서비스, 글로벌 참조 아키텍처, 프론트엔드 개발 사례 및 모범 사례에 대한 논의를 포함합니다.
feature: API Mesh, App Builder, Best Practices, Extensibility, Integration
topic: Architecture, Commerce, Integrations, Performance
old-role: Architect, Data Architect, Leader
role: Developer, Leader
level: Beginner, Intermediate
doc-type: Technical Video
duration: 3624
last-substantial-update: 2024-05-07T00:00:00.000Z
jira: KT-15465
exl-id: a8e1cd14-e9ff-46ba-87ea-c1cba3b4f72f
TQID: 'https://experienceleague.adobe.com/QO48-EUvrWmkJHTmaboYTDtFKX9UjtBabUgkwGEZxJM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: cc04bd17-78d5-5120-8c3d-1b8a57e49280
    internal-label: Integration
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: f8ddfd3b-6194-46e8-a176-0e918039be56
    internal-label: Cloud architecture
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
    internal-label: Leader
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%
---
# 최신 Adobe Commerce 구현 계획

이 웨비나는 카탈로그 서비스, 고객 아키텍처 이해 및 Edge Delivery Services을 포함하여 Adobe Commerce과 관련된 다양한 주제에 대한 포괄적인 개요를 제공합니다.
주제에는 Adobe Commerce Catalog as a Service의 라이브 검색 및 제품 추천, 고객 아키텍처&#x200B;을 이해하는 데 사용되는 API 및 데이터 흐름 분석, Edge Delivery Services의 높은 성능 및 낮은 지연 시간과 같은 각 서비스의 새로운 기능 또는 기존 기능과 이점이 포함됩니다. 또한 문서에는 효율적인 프로젝트 구축을 위한 GRA(Global Reference Architecture) 개념뿐만 아니라 고객과 상호 작용할 때 표준 문서 및 질문 목록을 갖는 것이 중요하다는 언급이 있습니다.

## 대상자

* 개발팀, 관리자, 수석 개발자, 기술 설계자
* 업그레이드, 마이그레이션 또는 새로운 상거래 오퍼링으로 Adobe Commerce을 구현하는 팀

## 비디오 콘텐츠

* 라이브 검색 및 제품 추천을 포함한 카탈로그 서비스는 Adobe Commerce에 포함되어 있으며 Experience League에서 사용할 수 있는 안내서가 있습니다.
* 고객 아키텍처 이해에는 API, 데이터 흐름, 일반적인 제품 분석, 고객 요구 사항과 상거래 기능 매핑이 포함됩니다.
* 고객 아키텍처를 이해하고 원활한 프로젝트 구현을 보장하려면 표준 문서 세트와 질문 목록을 작성하는 것이 중요합니다.
자동 분석 도구는 마이그레이션 프로젝트에 사용할 수 있으며 그 외의 경우에는 수동 설명서 검토 및 설문지가 필요합니다.
* 최근 Adobe Commerce용으로 출시된 Edge Delivery Services은 대기 시간이 짧고 작성 경험이 간소화된 고성능 상점 프런트 솔루션을 제공합니다.
* Edge Delivery Services은 성능을 우선시하며 등대 점수, SEO 및 전체 웹 사이트 속도를 크게 향상시킬 수 있습니다.
최신 구현의 계획, 빌드 및 유지 관리에 대한 Adobe Commerce 및 4부분으로 구성된 시리즈의 개요
* 효율적인 프로젝트 구축을 위한 GRA(Global Reference Architecture) 개념 소개
* 검색 단계에서 역할, 주요 문서 및 프로젝트 구성 요소를 이해하는 것이 얼마나 중요한지에 대해 설명합니다.
* 가격 책정 및 데이터 연결을 비롯한 다양한 서비스의 이점 및 기능에 대한 설명.
* 확장 가능한 상거래 기반이 필요하며 핵심 제품을 사용자 지정하지 않고도 애플리케이션을 확장할 수 있는 서비스의 사용이 필요하다는 점을 강조했습니다.
* 프로젝트의 가장 어려운 부분부터 시작하고 재사용 가능한 구성 요소에 대한 GRA 사용을 고려하는 것을 포함하여 작업의 적절한 계획, 구성 및 우선 순위 지정의 중요성.

>[!VIDEO](https://video.tv.adobe.com/v/3428987?learn=on)

{{$include /help/_includes/enablement-series-related-links.md}}
