---
title: 구성 가능한 카탈로그 데이터 모델(CCDM)이 존재하는 이유
description: CCDM이 단일 통합 카탈로그를 유지하면서 상점 상호간에 카탈로그 보기 및 머천다이징 서비스를 사용하여 필터링된 제품, 가격 및 규칙을 수신하는 방법에 대해 알아봅니다.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# 구성 가능한 카탈로그 데이터 모델이 있는 이유

최신 상거래 팀은 **브랜드**, **지역**, **대리점** 및 **디지털 채널**&#x200B;에서 종종 판매됩니다. 각 채널이 자체 카탈로그 사본을 유지하는 경우, 팀은 구매자 경험을 개선하는 것보다 SKU, 가격 및 가용성을 조정하는 데 더 많은 시간을 소비합니다. **Adobe** 뒤의 **CCDM(Composable Catalog Data Model)**&#x200B;은(는) SaaS 레이어의 **하나의 통합 카탈로그**&#x200B;와 같은 패턴을 반전하도록 설계되었습니다. **카탈로그 보기** 및 **정책**&#x200B;은(는) 각 상점 또는 통합에서 볼 수 있는 내용을 형성합니다.

## 이 비디오는 누구의 것입니까?

* 카탈로그 소스, 보기 및 정책을 구성하기 전에 Adobe Commerce Optimizer을 처음 접하고 비즈니스 컨텍스트가 필요한 Commerce 솔루션 설계자 및 개발자

## 비디오 콘텐츠

* 복제되고 채널별 카탈로그가 운영상의 위험을 야기하고 혁신 속도를 저하시키는 이유
* CCDM이 다중 브랜드 및 다중 지역 시나리오를 지원하면서 제품 데이터를 통합적으로 유지하는 방법
* 카탈로그 보기가 공유 기본 카탈로그와 특정 상점 또는 대상 간의 &quot;렌즈&quot; 역할을 하는 방법
* 머천다이징 서비스 API가 이러한 보기를 사용하여 Headless 경험이 구성된 카탈로그와 조정된 상태를 유지하는 방법

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## 사일로된 카탈로그로 인한 과제

모든 대리점 사이트, 지역 상점 또는 브랜드 속성이 **자체 카탈로그 데이터베이스**&#x200B;를 유지 관리하는 경우 다음과 같은 문제가 발생합니다.

* **복제** — 동일한 SKU, 설명 및 미디어를 여러 번 입력했습니다.
* **Drift** — 가격 업데이트, 새 특성 또는 단종된 항목이 한 채널에 도착하지만 다른 채널에는 도착하지 않습니다.
* **느린 시작** - 새로운 각 터치포인트가 단일 제품 레코드를 다시 사용하는 대신 대량 데이터 작업을 반복합니다.

CCDM은 다른 시스템이 보강하는 **하나의 구성 가능한 카탈로그**&#x200B;에서 제품 정보를 사용할 수 있도록 존재하지만 상점 전면은 여전히 **채널 적합** 구성 및 가격을 받습니다.

## 구성 가능한 카탈로그 데이터 모델의 변경 사항

Adobe Commerce Optimizer에서 제품 데이터는 하나 이상의 **카탈로그 소스**(예: `en-US`와 같은 로케일 또는 PIM 또는 ERP와 같은 업스트림 시스템)에서 **통합 기본 카탈로그로 수집됩니다**. 해당 소스는 원시 속성 및 값을 제공합니다.

**카탈로그 보기**&#x200B;에서는 비즈니스 컨텍스트에 대해 통합 데이터를 **구성 및 노출**&#x200B;하는 방법을 정의합니다. **정책**&#x200B;을 전달하는 제품, **가격 책자**&#x200B;를 적용하는 제품, **카탈로그 원본**&#x200B;을(를) 지원하는 제품. 따라서 동일한 기본 레코드가 각 사이트에 대한 전체 카탈로그를 복제하지 않고 **많은 예측**(예: 딜러, 지역 또는 브랜드당 개별 보기)을 지원할 수 있습니다.

이러한 분리—**데이터가 출처**(카탈로그 소스)와 표시 방법&#x200B;**비교**(카탈로그 보기)—를 통해 팀은 채널당 병렬 카탈로그를 유지 관리하는 대신 CCDM을 채택합니다.

## 상점 렌즈로서의 카탈로그 뷰

[머천다이징 서비스에 대한 카탈로그 보기](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}에 설명된 대로 카탈로그 보기는 **렌즈**&#x200B;처럼 동작합니다. 쇼핑객은 보기가 허용하는 제품, 가격 및 규칙만 볼 수 있고 **기본 카탈로그**&#x200B;는 공유 기록 시스템으로 유지됩니다. 이 모델은 **머천다이징 서비스**&#x200B;와 직접 연결되어 API 클라이언트가 올바른 보기(및 관련 헤더)를 전달하고 각 경험에 대해 일관된 정책 기반 응답을 받게 됩니다.

이러한 조각이 전체 흐름에 어떻게 적합한지에 대한 자세한 설명은 개발자 연습 [상점용 합성 가능한 카탈로그 만들기](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}를 참조하십시오.

## 관련 컨텐츠

* [카탈로그 보기에 대해 알아보기](./learn-about-the-ccdm-feature-catalog-views.md)
* [머천다이징 서비스에 대한 카탈로그 보기](https://experienceleague.adobe.com/en/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [상점용 합성 가능 카탈로그 만들기](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [머천다이징 API 시작하기](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
