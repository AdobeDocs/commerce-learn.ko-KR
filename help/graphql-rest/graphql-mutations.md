---
title: GraphQL을 사용하여 돌연변이 수행
description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
landing-page-description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
short-description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
kt: 13938
doc-type: video
duration: 268
audience: all
last-substantial-update: 2023-10-12T00:00:00.000Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
TQID: https://experienceleague.adobe.com/DyzC0YLv2eWrfSAUZb-32cMAePHjurmp1RyynMbsa7Q
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 406
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

REST는 데이터를 변경하는 요청과 요청 유형 또는 &quot;동사&quot;가 없는 요청(GET과 POST 또는 PUT)을 구별합니다.
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
the syntax is identical to a query. Like queries, the mutation includes:

* An arbitrary operation name (`doAddToCart`)
* A list of variables (for example, `$cartId`)
* An initial field (`addProductsToCart`) with arguments (for example, `cartId`, set to the value of `$cartId`) in parentheses
* A subselection of fields in braces

The fields subselection allows you to flexibly define the fields you would like returned (from the type assigned as the
return value of `addProductsToCart` - `AddProductsToCartOutput`) after the mutation is completed.

As explained previously, fields defined in a GraphQL schema start on a root type for queries (typically referred to as a `Query`). Similarly,
another root type exists for mutations (typically referred to as `Mutation`). `addProductsToCart` is a field
on that root type.

A few other notes about the above example:

* The `!` character suffixing `String` and `CartItemInput` indicates that the variable is required.
* The square brackets (`[]`) around the `CartItemInput` type specified for `$cartItems` indicate a list
of that type rather than a single value.

{{$include /help/_includes/graphql-rest-related-links.md}}
