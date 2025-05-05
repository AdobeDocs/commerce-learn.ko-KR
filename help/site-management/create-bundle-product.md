---
title: 번들 제품 만들기
description: REST API 및 Commerce 관리자를 사용하여 번들 제품을 만드는 방법을 알아봅니다.
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 765bf4159892416e02ea1e9b8e4fa69e396d40af
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# 번들 제품 만들기

번들 제품은 상위 제품 아래에 여러 제품을 그룹화하는 방법입니다. 이러한 하위 제품은 정의된 제품 세트이거나 고객을 위한 유연한 구성 옵션을 제공하는 몇 가지 변형을 제공할 수 있습니다. 번들 제품 유형을 설정하는 데 시간이 조금 더 걸리며 구성하기 전에 몇 가지 계획을 수행해야 합니다. 그러나 번들 제품을 제공하면 고객이 제품 선택을 보다 쉽게 사용자 지정할 수 있으므로 쇼핑 경험이 향상됩니다.

예를 들어 웹 스토어에서 `Learning to surf`이라는 제품 번들을 제공할 수 있습니다. 번들은 사용 가능한 옵션을 지정하는 지정된 하위 제품의 컨테이너 역할을 하는 상위 제품입니다.

- 표준 서핑보드
- 전형적인 서핑보드 가죽 끈
- 붉은색 서프보드 핀

추가적인 유연성이 필요한 경우, 몇 가지 하위 제품 옵션을 허용하는 것이 좋습니다. 이를 위해서는 옵션과 하위 제품을 보다 복합적으로 사용해야 합니다. 이전 예에서 를 확장하기 위한 최종 옵션은 다음과 같습니다.

- 표준 서핑보드
- 전형적인 서핑보드 가죽 끈
- 지느러미 색상 선택:
   - 빨강
   - 파랑
   - 노랑

번들이 간단한 제품의 정적 그룹이든 변형이 있는 여러 제품이든, 유연한 구성 옵션을 통해 번들 제품 유형을 Adobe Commerce 스토어에 대한 고유하고 강력한 머천다이징 도구로 만들 수 있습니다.

번들 제품을 만들기 전에 번들 제품에 포함할 모든 간단한 제품을 Adobe Commerce에서 사용할 수 있는지 확인하십시오. 존재하지 않는 모든 을(를) 만듭니다.

이 자습서에서는 REST API 및 Adobe Commerce 관리자를 사용하여 번들 제품을 만드는 방법을 알아봅니다.

REST API를 사용하여 번들 제품을 정의합니다.

1. 번들 제품에서 사용할 간단한 제품을 만듭니다.
1. 번들 제품을 만들고 단순 제품을 연결합니다.
1. 번들 제품 및 모든 관련 옵션을 가져옵니다.
1. 번들 제품의 한 섹션에서 옵션을 삭제합니다.
1. 번들 제품에 대한 옵션을 복원합니다.

Adobe Commerce 관리에서 번들 제품을 만들 때 먼저 간단한 제품을 만들거나 자동화된 도구를 사용하여 마법사를 사용하여 간단한 제품을 만들 수 있습니다.

## 이 비디오는 누구의 것입니까?

- 웹 사이트 관리자
- 전자 상거래 머천다이저
- REST API를 사용하여 Adobe Commerce에서 번들 제품을 만드는 방법을 배우고자 하는 새로운 Adobe Commerce 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3454508?learn=on&captions=kor)

## REST를 사용하여 제품 만들기

다음 명령은 이 예에서 번들 제품을 정의하는 데 필요한 모든 제품을 만듭니다. 5개의 단순 제품과 3개의 옵션이 있는 1개의 번들 제품.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- `"attribute-set": 4`을(를) 변경하여 `4`을(를) 환경의 특성 집합 ID로 바꾸십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## 번들 제품을 만들고 간단한 제품을 옵션으로 할당

다음 POST 요청을 전송하여 번들 제품을 만듭니다.

요청을 제출하기 전에 환경에 대한 값으로 예제를 업데이트하십시오.

- `"attribute_set_id": 4,`을(를) 변경하고 `4`을(를) 환경의 특성 집합 ID로 바꾸십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## 번들 제품에서 옵션 삭제

cURL을 사용하여 다음 DELETE 요청을 전송하여 카탈로그에서 제품을 삭제하지 않고 번들 제품에서 하위 제품을 제거합니다.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## 제품 복원 옵션

번들 제품 옵션을 업데이트할 때에는 이 제품과 연결할 모든 옵션을 포함해야 합니다. 원래 옵션 세트에 세 개의 제품이 포함되어 있고 하나가 제거된 경우 POST 요청에 세 개의 옵션을 모두 포함하여 제품 번들이 모든 옵션을 지정하는지 확인하십시오. 제거한 옵션만 포함한 경우 업데이트된 제품 번들에는 해당 옵션만 포함됩니다.

번들 제품에 대한 생성 응답을 검토하여 옵션 ID를 찾습니다. 해당 응답에서 `option_id`은(는) `35`입니다.

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

다음 POST 요청을 제출하여 제거한 옵션을 추가하려면 제품 번들을 업데이트하십시오.

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## 추가 리소스

- [번들 제품 자습서 만들기](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [제품 번들](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html?lang=ko){target="_blank"}
- [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
