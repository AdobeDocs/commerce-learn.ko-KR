---
title: Edge Delivery Services 장바구니 항목
description: Adobe Commerce Edge Delivery Services의 제품 속성 및 슬롯을 사용하여 게재 타임라인, 판매 알림 및 장바구니 할인을 표시하는 방법에 대해 알아봅니다.
feature: Best Practices, Compliance, Extensibility, Storefront, Tools and External Services
topic: Architecture, Commerce, Headless
role: Developer
level: Beginner
doc-type: Technical Video
duration: 430
last-substantial-update: 2024-12-16T00:00:00.000Z
jira: KT-16686
exl-id: a9a6deb3-6431-4cb2-a3c2-0d584d3da3b5
TQID: 'https://experienceleague.adobe.com/Scjl9nt8Oai2tM3y2eTEbxUYWACzoJgAY9U2-bBK4rY'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: b5f00040-57a0-4a6d-a39e-383b1936c2c9
    internal-label: Compliance
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
    internal-label: Storefront
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%
---
# 장바구니 항목에 대한 제품 속성 사용

Adobe Commerce 장바구니를 사용자 정의하고 확장하는 방법을 알아봅니다. 이 데모에서는 세 가지 업데이트가 있습니다.  데모는 역주문 상품에 대한 배송 타임라인을 표시하고, 반납 및 최종 판매 공지를 보여주며, 소계가 75달러 이상인 카트에 대해 25% 할인을 적용하는 것으로 시작한다. 제품 속성 및 슬롯을 활용하여 이러한 기능을 장바구니에 추가하면 전반적인 사용자 경험이 향상됩니다.

## 배울 내용

몇 가지 코드 행을 변경하여 고객에게 새로운 기능 세트를 보여 주기 위해 Adobe Commerce 장바구니 드롭인을 사용하고 트랜잭션 흐름을 조정하는 방법을 알아봅니다.  또한 Sharepoint에서 이러한 변경 내용을 관리하도록 변환하여 A/B 테스트를 수행하는 방법에 대해 알아봅니다.

## 이 비디오는 누구의 것입니까?

* 전자 상거래 배달 타임라인, 판매 정보 및 할인을 통해 장바구니를 개선하고자 하는 비즈니스
* 장바구니 항목에 대한 이러한 사용자 지정을 구현하는 방법을 알아보아야 하는 개발자입니다.
* 고객 경험을 향상시키기 위해 기본 기능을 사용하여 어떤 옵션을 사용할 수 있는지 이해하려는 머천다이저.

## 비디오 콘텐츠

* 장바구니에서 주문 후 제품에 대한 제품 세부 사항을 사용자 지정하고 사용자 지정된 배달 타임라인을 표시합니다.
* 장바구니 항목에 대한 표준 기능을 사용하는 반품 및 최종 판매 정보 예.
* 이 정보를 효과적으로 표시하기 위해 장바구니 규칙 및 바닥글 슬롯을 사용하여 표시되는 쿠폰 할인.
* 제품 속성 및 슬롯을 사용하여 장바구니 기능을 확장하고 사용자 정의하여 모든 프로젝트 요구 사항이 충족되도록 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3441123?captions=kor&learn=on)


## 추가 설명서

* [장바구니에 제품 라인 추가](https://experienceleague.adobe.com/developer/commerce/storefront/dropins/cart/tutorials/add-product-lines-to-cart-summary/?lang=ko)