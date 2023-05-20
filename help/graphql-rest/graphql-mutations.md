---
title: GraphQL을 사용하여 돌연변이 수행
description: Adobe Commerce 및 의 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. [!DNL Magento Open Source]. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
landing-page-description: Adobe Commerce 및 의 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. [!DNL Magento Open Source]. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
short-description: Adobe Commerce 및 의 GraphQL을 사용하여 돌연변이를 수행하는 방법에 대해 소개합니다. [!DNL Magento Open Source]. POST 호출을 사용하여 첫 번째 돌연변이를 수행합니다.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 0%

---

# 돌연변이

모든 전체 API 사양은 데이터를 쿼리할 뿐만 아니라 만들고 업데이트하는 기능도 제공해야 합니다.

REST는 데이터를 변경하는 요청과 요청 유형 또는 &quot;동사&quot;(GET 대 POST 또는 PUT)가 없는 요청을 구별합니다.
GraphQL을 사용할 때 데이터 수정 쿼리는 `mutation` 서버에 정의된 스키마의 다른 루트 유형에 해당하는 키워드입니다.

사용자의 장바구니에 제품을 추가하기 위한 이 예제 돌연변이를 살펴보십시오. (이렇게 하려면 로그인한 고객의 세션에 대해 생성되었거나 를 사용하여 생성된 장바구니 ID가 필요합니다. `createEmptyCart` 돌연변이.)

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

위의 예에서 가장 주목해야 할 사항은 의 사용과 별도로 입니다. `mutation` 대신 키워드 `query`: 구문은 쿼리와 동일합니다. 쿼리와 마찬가지로 돌연변이는 다음을 포함합니다.

* 임의의 작업 이름(`doAddToCart`)
* 변수 목록(예: `$cartId`)
* 초기 필드(`addProductsToCart`인수가 있는 (예: ) `cartId`, 값으로 설정 `$cartId`괄호 안의 )
* 중괄호로 묶인 필드의 하위 선택

필드 하위 선택을 사용하면 반환 값으로 지정된 유형에서 반환할 필드를 유연하게 정의할 수 있습니다. `addProductsToCart` - `AddProductsToCartOutput`) 돌연변이가 완료된 후.

앞에서 설명한 바와 같이, GraphQL 스키마에 정의된 필드는 쿼리의 루트 유형에서 시작합니다(일반적으로 `Query`). 마찬가지로, 돌연변이에 대해 또 다른 루트 유형이 존재합니다(일반적으로 다음과 같이 지칭됨) `Mutation`). `addProductsToCart` 은(는) 해당 루트 유형의 필드입니다.

위의 예제에 대한 몇 가지 다른 참고 사항:

* 다음 `!` 문자 누락 `String` 및 `CartItemInput` 는 변수가 필요함을 나타냅니다.
* 대괄호(`[]`) 주위에 `CartItemInput` 다음에 대해 지정된 유형: `$cartItems` 단일 값이 아닌 해당 유형의 목록을 나타냅니다.

{{$include /help/_includes/graphql-rest-related-links.md}}
