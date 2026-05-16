---
title: 구성 가능한 카탈로그 데이터 모델의 카탈로그 보기에 대해 알아보기
description: Adobe CCDM(Composable Catalog Data Model)의 카탈로그 보기가 개별 제품, 가격 및 규칙을 사용하여 하나의 기본 카탈로그를 각 상점에 매핑하는 방법에 대해 알아봅니다.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: 96a1356a399fa5cdca9d9befd7c14ebad1b0162f
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Adobe 구성 가능한 카탈로그 데이터 모델의 카탈로그 보기

카탈로그 보기는 구성 가능한 단일 카탈로그와는 다르게 각 대상자를 제공하는 방법입니다. 이 튜토리얼에서는 카탈로그 보기가 무엇인지, Adobe Commerce Optimizer이 이 보기를 쇼핑객에게 적용하는 방법, **Carvelo Automoals** 데모 시나리오를 사용하여 고유한 보기 ID가 제품, 가격 및 규칙의 앵커인 이유를 설명합니다.

## 이 비디오는 누구의 것입니까?

* Adobe Commerce Optimizer에서 다중 브랜드 또는 다중 판매자 카탈로그를 모델링하는 Commerce 솔루션 설계자 및 개발자

## 비디오 콘텐츠

* Carvelo Automotive 시나리오(브랜드, 대리점 및 계약)
* 카탈로그 보기가 무엇인지와 통합 기본 카탈로그에 대한 &quot;렌즈&quot; 메타포
* Storefront가 카탈로그 보기를 사용하여 제품 및 가격을 필터링하는 방법(예: Celport)
* 카탈로그 신뢰할 수 있는 단일 소스의 고유 ID 및 비즈니스 가치 보기

>[!VIDEO](https://video.tv.adobe.com/v/3491269?captions=kor&learn=on)

## 시나리오: Carvelo Automons

**Carvelo Automoals**&#x200B;은(는) Adobe Commerce 데모, 설명서 및 튜토리얼에 자주 사용되는 가상 자동차 부품 회사입니다. Carvelo는 다음과 같은 세 가지 대리점을 통해 세 가지 브랜드(**Aurora**, **Bolt**, **Cruz**)에서 부품을 판매합니다.

* **Arkbridge**&#x200B;은(는) West Coast Inc에 속합니다.
* **Kingsbluff** 및 **Celport**&#x200B;은(는) East Coast Inc에 속합니다.

각 대리점은 어떤 제품을 판매할 수 있는지에 대해 자체 합의를 맺고 있습니다. 이렇게 하면 실제로 복잡한 설정이 생성되며, 약 6백만 SKU의 **단일 기본 카탈로그**&#x200B;에서 계속 실행할 수 있습니다. 이를 가능하게 하는 기능은 **카탈로그 보기**&#x200B;입니다.

## 카탈로그 보기란 무엇입니까?

**카탈로그 보기**&#x200B;는 특정 비즈니스 컨텍스트에 대해 구성된 카탈로그 보기입니다. **렌즈**&#x200B;로 생각하십시오. 통합 기본 카탈로그는 모든 브랜드에 대한 모든 SKU를 포함하여 중간에 위치합니다. 각 대리점은 고유한 렌즈(카탈로그 보기)를 얻습니다.

이 예제에서는

* **Arkbridge** 렌즈는 Aurora, Bolt 및 Cruz 부품의 세 가지 브랜드를 모두 표시할 수 있습니다.
* **Celport** 렌즈는 Bolt 및 Cruz 부분의 하위 집합만 표시합니다.

각 카탈로그 보기는 **가격 책정**&#x200B;도 제어할 수 있습니다. **기본 카탈로그 데이터는 변경되지 않습니다**. 해당 컨텍스트에 대해 볼 수 있는 측면(구별, 가격 및 규칙)만 변경됩니다.

## Commerce Optimizer에서 카탈로그 보기를 적용하는 방법

쇼핑객이 **Celport** 상점을 방문하면 Adobe Commerce Optimizer은 **Celport 카탈로그 보기**&#x200B;를 사용하여 적용되는 제품, 가격 및 규칙을 정확하게 결정합니다. 쇼핑객은 그 렌즈가 허락하는 것만 본다.

Aurora 타이어, Bolt 모터 또는 Cruz 배터리와 같은 다른 제품이 여전히 카탈로그에 있을 수 있지만, 카탈로그 보기에서 허용하지 않는 경우 **Celport의 상점 전면이 절대 노출하지 않습니다**.

## 카탈로그 보기 ID 및 비즈니스 값

모든 카탈로그 보기에는 **고유 ID**&#x200B;가 있습니다. 해당 ID는 상점 첫 페이지를 카탈로그 구성과 연결합니다. 상점 구성에서 이를 한 번 설정하면 다운스트림 동작(올바른 제품, 올바른 가격 및 올바른 규칙)이 따릅니다.

모든 대리점 또는 브랜드에 대해 별도의 카탈로그를 유지 관리하며 동기화를 유지하는 대신 구성 가능한 **하나** 카탈로그를 유지 관리합니다. 카탈로그 보기는 해당 **신뢰할 수 있는 단일 원본**&#x200B;에서 **다양한 상점 환경**&#x200B;을 형성하는 방법입니다.

## 관련 컨텐츠

* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/ko/docs/commerce/optimizer/overview){target="_blank"}
* [머천다이징 API 시작하기](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
