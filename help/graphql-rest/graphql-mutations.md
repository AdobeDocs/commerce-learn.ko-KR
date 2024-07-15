---
title: GraphQL을 사용하여 돌연변이 수행
description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
landing-page-description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
short-description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# 돌연변이

GraphQL 및 Adobe Commerce 시리즈 3부입니다. 돌연변이는 GraphQL을 사용하여 값을 저장, 업데이트 및 반환하는 기능입니다.


>[!VIDEO](https://video.tv.adobe.com/v/3424121?learn=on)

## 이 시리즈의 GraphQL 관련 비디오 및 튜토리얼

* [1부 GraphQL - 소개](../graphql-rest/intro-graphql.md)
* [2부 GraphQL - 쿼리](../graphql-rest/graphql-queries.md)
* [파트4 GraphQL - 스키마](../graphql-rest/graphql-schema.md)

## 예시 돌연변이

모든 전체 API 사양은 데이터를 쿼리할 뿐만 아니라 만들고 업데이트하는 기능도 제공해야 합니다.

REST는 데이터를 변경하는 요청과 요청 유형 또는 &quot;동사&quot;(GET 대 POST 또는 PUT)가 없는 요청을 구별합니다.
GraphQL을 사용하는 경우 데이터 수정 쿼리는 다른 키워드에 해당하는 `mutation` 키워드로 구분됩니다
서버에 정의된 스키마의 루트 유형입니다.

사용자의 장바구니에 제품을 추가하기 위한 이 예제 돌연변이를 살펴보십시오. (생성된 장바구니 ID가 필요합니다.
(로그인한 고객의 세션에 사용하거나 `createEmptyCart` 돌연변이를 사용하는 경우)

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

위의 돌연변이가 다음 변수 사전과 함께 요청으로 전송되는 것을 상상할 수 있습니다.

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

그리고 마지막으로 다음과 같은 응답을 받을 수 있습니다.

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

위의 예에서 주의해야 할 주요 사항은 `query` 대신 `mutation` 키워드를 사용하는 것과 별도로,
구문은 쿼리와 동일합니다. 쿼리와 마찬가지로 돌연변이는 다음을 포함합니다.

* 임의의 작업 이름(`doAddToCart`)
* 변수 목록(예: `$cartId`)
* 괄호로 묶인 인수(예: `cartId`, `$cartId` 값으로 설정됨)가 있는 초기 필드(`addProductsToCart`)
* 중괄호로 묶인 필드의 하위 선택

필드 하위 선택을 사용하면 (으로 지정된 유형에서) 반환할 필드를 유연하게 정의할 수 있습니다.
돌연변이가 완료된 후 `addProductsToCart` - `AddProductsToCartOutput`의 반환 값.

앞에서 설명한 바와 같이, GraphQL 스키마에 정의된 필드는 쿼리의 루트 유형에서 시작합니다(일반적으로 `Query`). 마찬가지로,
다른 루트 형식이 돌연변이에 대해 존재합니다(일반적으로 `Mutation`). `addProductsToCart`은(는) 필드입니다.
루트 형식에 매핑할 수도 있습니다.

위의 예제에 대한 몇 가지 다른 참고 사항:

* `String` 및 `CartItemInput`을(를) 접두사로 사용하는 `!`은(는) 변수가 필요함을 나타냅니다.
* `$cartItems`에 대해 지정된 `CartItemInput` 형식의 대괄호(`[]`)는 목록을 나타냅니다
단일 값이 아닌 해당 형식의

{{$include /help/_includes/graphql-rest-related-links.md}}
