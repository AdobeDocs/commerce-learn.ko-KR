---
title: 그룹화된 제품 만들기
description: REST API 및 Commerce 관리자를 사용하여 그룹화된 제품을 만드는 방법을 알아봅니다.
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
source-git-commit: 76a67af957b0d8c1eb64ad42f92412f338650d4b
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# 그룹화된 제품 만들기

그룹화된 제품은 그룹으로 표시되는 간단한 독립 실행형 제품으로 구성됩니다. 단일 제품의 변형을 제공하거나 계절 또는 테마별로 그룹화할 수 있습니다. 그룹화된 제품을 만들기 전에 그룹에 포함할 모든 간단한 제품을 Adobe Commerce에서 사용할 수 있는지 확인하고 존재하지 않는 제품을 만듭니다.

이 자습서에서는 REST API 및 Adobe Commerce 관리자를 사용하여 그룹화된 제품을 만드는 방법을 알아봅니다.

REST API를 사용하여 관리자에서 그룹 제품을 만듭니다.

1. 빈 그룹화된 제품을 만듭니다.
1. 그룹화된 제품에서 사용할 간단한 제품을 만듭니다.
1. 비어 있는 그룹화된 제품을 단순 제품으로 채웁니다.
1. 빈 그룹화된 제품을 만들고 간단한 제품을 연결합니다.

   단순 제품을 그룹화된 제품에 연결하면 페이로드의 정렬 순서 특성(`position`)이 프론트엔드에서 사용되어 연결된 제품을 원하는 순서로 표시합니다. `position` 특성을 지정하지 않으면 그룹화된 제품에 추가된 순서대로 제품이 표시됩니다.

Adobe Commerce 관리자에서 그룹화된 제품을 만들 때 먼저 간단한 제품을 만듭니다. 그룹화된 제품을 만들 준비가 되면 간단한 제품을 그룹화된 제품에 한 묶음으로 할당하여 연결합니다.

## 이 비디오는 누구의 것입니까?

- 웹 사이트 관리자
- 전자 상거래 머천다이저
- REST API를 사용하여 Adobe Commerce에서 그룹화된 제품을 만드는 방법을 배우고자 하는 새로운 Adobe Commerce 개발자입니다.

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3454045?learn=on&captions=kor)

## 그룹화된 제품에 대한 설정

이 예에는 세 개의 간단한 제품(먼저 만들어짐)과 그룹화된 제품이 있습니다. 두 개의 단순 제품이 그룹화된 제품과 연결된 다음 세 번째 단순 제품이 그룹화된 제품에 추가됩니다.

## cURL을 사용하여 첫 번째 간단한 제품 만들기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## cURL을 사용하여 두 번째 단순 제품 만들기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## cURL을 사용하여 세 번째 단순 제품 만들기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## cURL을 사용하여 빈 그룹화된 제품 만들기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## 첫 번째 및 두 번째 단순 제품을 그룹화된 제품에 추가합니다

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## 기존의 그룹화된 제품에 세 번째 단순 제품 추가

그룹화된 제품에 처음 연결된 두 제품에 사용되는 적절한 위치 번호(`1` 또는 `2`을 제외한 모든 위치)를 포함하십시오. 이 예제의 경우 위치는 `4`입니다.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## 그룹화된 제품에서 간단한 제품 삭제

그룹화된 제품에서 [간단한 제품을 삭제](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/)하려면 `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`을(를) 사용합니다.

`{type}`(으)로 사용할 항목을 검색하려면 xdebug를 사용하여 요청을 캡처하고 $linkTypes: `related`, `crosssell`, `uupsell` 및 `associated`을(를) 평가하십시오.
![그룹화된 제품 링크 형식 - 대체 텍스트](/help/assets/site-management/catalog/grouped-types.png "xdebug 세션 중에 캡처된 그룹화된 제품 링크 형식")

단순 제품을 그룹화된 제품에 연결할 때 페이로드에는 다음과 유사한 몇 가지 섹션이 포함되어 있습니다.

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

페이로드에서 `link_type` 값 `associated`은(는) DELETE 요청에 필요한 `{type}` 값을 제공합니다. 요청 URL은 `/V1/products/my-new-grouped-product/links/associated/product-sku-three`과(와) 비슷합니다.

`my-new-grouped-product` SKU가 있는 그룹화된 제품에서 `product-sku-three` SKU가 있는 간단한 제품을 삭제하려면 cURL 요청을 참조하십시오.

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## cURL을 사용하여 그룹화된 제품 가져오기

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 추가 리소스

- [그룹화된 제품 만들기 및 관리](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [그룹화된 제품](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=ko){target="_blank"}
- [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
