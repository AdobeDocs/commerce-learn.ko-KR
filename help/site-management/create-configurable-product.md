---
title: 구성 가능한 제품 만들기
description: REST API 및 상거래 관리자를 사용하여 구성 가능한 제품을 만드는 방법을 알아봅니다.
kt: 14586
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 76716d4c9530963f198a855e101c76b6374c6d75
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 0%

---


# 구성 가능한 제품 만들기

구성 가능한 제품은 여러 간단한 제품의 상위 제품입니다. 구매자가 특정 제품 변형을 선택하기 위해 하나 이상의 선택을 하도록 구성 가능한 제품을 정의합니다. 예를 들어 제품이 셔츠인 경우 구매자는 크기 및 색상 옵션을 선택하여 셔츠를 선택해야 합니다.

구성 가능한 제품은 더 많은 SKU를 사용하며 처음에는 설정하는 데 시간이 조금 더 걸릴 수 있지만 결국 시간을 절약할 수 있습니다. 비즈니스를 확장하려는 경우 구성 가능한 제품 유형은 여러 옵션이 있는 제품에 적합합니다.

구성 가능한 제품을 만들기 전에 구성 가능한 제품에 포함할 모든 간단한 제품을 Adobe Commerce에서 사용할 수 있는지 확인하십시오. 존재하지 않는 모든 을(를) 만듭니다.

이 자습서에서는 REST API 및 Adobe Commerce 관리자를 사용하여 구성 가능한 제품을 만드는 방법을 알아봅니다.

REST API를 사용하여 구성 가능한 제품을 만듭니다.

1. 다음에 대한 속성 가져오기 [속성 집합](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html) 후속 API 호출에 ID 번호를 사용합니다.
1. 구성 가능한 제품에서 사용할 간단한 제품을 만듭니다.
1. 구성 가능한 빈 제품을 만들고 간단한 제품을 연결합니다.
1. 구성 가능한 제품에 대한 제품 속성을 설정합니다.
1. 빈 구성 가능한 제품을 간단한 제품으로 채웁니다.
1. 구성 가능한 제품 및 모든 속성을 가져옵니다.
1. 구성 가능한 제품에 대해 할당된 하위 제품을 가져옵니다.
1. 구성 가능한 제품에 대한 간단한 제품 연결을 삭제합니다.

Adobe Commerce 관리자에서 구성 가능한 제품을 만들 때 먼저 간단한 제품을 만들거나 마법사를 사용하여 사용할 새로운 간단한 제품을 만드는 자동화된 도구를 사용할 수 있습니다.

## 이 비디오는 누구의 것입니까?

- 웹 사이트 관리자
- 전자 상거래 머천다이저
- REST API를 사용하여 Adobe Commerce에서 구성 가능한 제품을 만드는 방법을 배우고자 하는 새로운 Adobe Commerce 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## cURL을 사용하여 색상 속성 가져오기

이 예에서는 속성 세트 10에 대해 모든 개별 속성이 있는 전체 속성 세트가 반환됩니다. 길어질 수 있습니다. 수백 개의 줄이 흔하지 않습니다. 응답을 검토할 때 색상에 대한 속성 ID는 중간에 있을 수 있습니다. grep 또는 기타 방법을 사용하여 결과를 검색함으로써 이러한 값을 신속하게 검색할 수 있습니다. 내 응답은 665행 근처에 있으며 JSON 응답의 다음 스니펫에 포함됩니다.

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


속성 ID를 검색하여 구성 가능한 제품을 설정하려면 `attribute-sets/10/attributes` 대체할 다음 cURL 요청의 일부 `10` 속성 세트 ID를 사용하는 환경. 이 요청은 GET 메서드를 사용합니다.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## cURL을 사용하여 첫 번째 간단한 제품 만들기

### 환경 ID 및 제품 세부 사항 조정

API를 사용하여 cURL을 사용하여 다음 POST 요청을 전송하여 첫 번째 간단한 제품을 만듭니다.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- 변경 `"attribute-set": 10` 바꾸기 `10` (속성 세트 ID 포함)
- 변경 `"value": "13"` 바꾸기 `13` 를 사용(환경 값 포함)하십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## cURL을 사용하여 두 번째 단순 제품 만들기

API를 사용하여 cURL을 사용하여 다음 POST 요청을 전송하여 두 번째 간단한 제품을 만듭니다.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- 변경 `"attribute_set_id": 10,` 및 바꾸기 `10` (속성 세트 id가 있어야 함).
- 변경 `"value": "14"` 및 바꾸기 `14` 를 사용(환경 값 포함)하십시오.


```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## cURL을 사용하여 세 번째 단순 제품 만들기

API를 사용하여 cURL을 사용하여 다음 POST 요청을 전송하여 세 번째 간단한 제품을 만듭니다.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- 변경 `"attribute_set_id": 10,` 바꾸기 `10` (속성 세트 ID 포함)
- 변경 `"value": "15"` 및 바꾸기 `15` 를 사용(환경 값 포함)하십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## cURL을 사용하여 구성 가능한 빈 제품 만들기

API를 사용하여 cURL을 사용하여 다음 POST 요청을 전송하여 빈 구성 가능한 제품을 만듭니다.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- 변경 `"attribute_set_id": 10,` 및 바꾸기 `10` (속성 세트 id가 있어야 함)
- 변경 `"value": "93"` 및 바꾸기 `93` 를 사용(환경 값 포함)하십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## 구성 가능한 제품에 사용할 수 있는 옵션 설정

API를 사용하여 구성 가능한 제품에 사용할 수 있는 옵션을 설정하여 cURL을 사용하여 다음 POST 요청을 전송합니다.

요청을 제출하기 전에 변경 `"attribute_id": 93,` 바꾸기 `93` (환경의 속성 id) 사용.

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

구성 가능한 제품(상위)에 대한 옵션을 설정하는 것을 잊은 경우 하위 제품을 구성 가능한 제품에 연결하려고 하면 오류가 발생합니다. 오류 메시지는 다음 예제와 유사합니다.

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## 구성 가능한 항목에 하위 제품 연결

이제 세 가지 간단한 제품을 만들었습니다.

- `"Kids Hawaiian Ukulele Red"`,
- `"Kids-Hawaiian-Ukulele-Blue"`
- `"Kids-Hawaiian-Ukulele-Green"`

API를 사용하여 각 제품에 대한 다음 POST 요청을 전송하여 이러한 간단한 제품을 구성 가능한 제품의 하위 항목으로 추가합니다. 각 제품에 대해 별도의 요청을 제출합니다.

각 요청에 대해 `childSKU` 값을 추가하는 하위 제품에 대한 값과 함께 사용합니다. 다음 예제에서는 간단한 제품을 지정합니다 `kids-Hawaiian-Ukulele-red` SKU를 사용하여 구성 가능한 제품 `Kids-Hawaiian-Ukulele-red`.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## cURL을 사용하여 구성 가능한 제품 가져오기

이제 3개의 할당된 하위 SKU로 구성 가능한 제품을 만들었습니다. API에서 할당된 제품에 대해 연결된 ID를 보고 cURL을 사용하여 다음 GET 요청을 보낼 수 있습니다. 이 요청은 구성 가능한 제품에 대한 자세한 정보를 반환합니다.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

다음은 GET 메서드를 사용합니다

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 구성 가능한 제품에 연결된 하위 제품 가져오기

이 요청은 구성 가능한 제품에 연결된 하위 항목만 반환합니다. 이 응답에는 SKU 및 가격을 포함한 하위 제품에 대한 모든 속성이 있습니다.

다음은 GET 메서드를 사용합니다

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 구성 가능한 상위 제품에서 하위 제품 삭제 또는 제거

API를 사용하여 cURL을 사용하여 다음 DELETE 요청을 보내어 카탈로그에서 제품을 삭제하지 않고 구성 가능한 제품에서 하위 제품을 제거할 수 있습니다.

다음은 DELETE 메서드를 사용합니다

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## 추가 리소스

- [구성 가능한 제품 튜토리얼 만들기](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
- [구성 가능한 제품](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html){target="_blank"}
- [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
