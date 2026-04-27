---
title: 그룹화된 제품 만들기
description: REST API 및 Commerce 관리자를 사용하여 그룹화된 제품을 만드는 방법을 알아봅니다.
kt: 14585
doc-type: video
duration: 979
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
TQID: https://experienceleague.adobe.com/nosJh3ytiC54wmNWaUmSu9qjZCN-ssjolNZD702EpEg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: c18ed297-2187-4aec-affb-9d9654eca6fc
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 551
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

* 웹 사이트 관리자
* 전자 상거래 머천다이저
* REST API를 사용하여 Adobe Commerce에서 그룹화된 제품을 만드는 방법을 배우고자 하는 새로운 Adobe Commerce 개발자입니다.

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

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

## Create an empty grouped product using cURL

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

## Add the first and second simple products to the grouped product

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

## Add the third simple product to the existing grouped product

Include the appropriate position number (anything except `1` or `2`), which are used for the first two products originally associated to the grouped product. For this example, the position is `4`.

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

## Delete a simple product from a grouped product

To [delete a simple product](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) from a grouped product, use: `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

To discover what to use as `{type}`, use xdebug to capture the request and evaluate the $linkTypes: `related`, `crosssell`, `uupsell`, and `associated`.
![Grouped Product link types - alt text](/help/assets/site-management/catalog/grouped-types.png "Grouped product link types captured during xdebug session")

When linking the simple products to the grouped product, the payload contained a few sections similar to:

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

In the payload, the `link_type` value `associated` provides the `{type}` value required in the DELETE request. The request URL will be similar to `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

See the cURL request to delete the simple product with the `product-sku-three` SKU from the grouped product with the `my-new-grouped-product` SKU:

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## Get a grouped product using cURL

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 추가 리소스

* [Create and manage grouped products](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
* [Grouped Product](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
* [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
