---
title: GraphQL를 사용 하는 스키마 언어
description: GraphQL와 관련 된 스키마에 대해 알아보십시오. 스키마에 대 한 설명과 스키마를 읽는 몇 가지 흥미로운 패턴 및 방법을 알아보십시오.
landing-page-description: 이는 GraphQL에 대 한 소개입니다. 스키마 이해 및 일부 요소를 해석 하는 방법
short-description: 이는 GraphQL에 대 한 소개입니다. 스키마 이해 및 일부 요소를 해석 하는 방법
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# 스키마 언어

사용 되는 쿼리 및 mutations는 서버에서 구현 되는 특정 데이터 그래프에 의존 하 여 GraphQL 런타임이 쿼리를 해결 하는 데 사용 합니다. GraphQL 사양은 데이터 그래프의 유형과 관계를 표현 하는 알 수 없는 언어를 정의 합니다.

다음은 지금까지 살펴본 쿼리 및 mutations를 지 원하는 축약 유형 스키마입니다.

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

[여기에 표시 되지 않는 몇 가지 개념에 대 한 구문을 비롯 하 여 유형 시스템의 세부 사항에 대 한 자세한 내용은 GraphQL 설명서 ](https://graphql.org/learn/schema/) {target="_blank"} 를 참조 하십시오. 그러나 위의 예는 자체 설명이 있습니다. 또한 구문과 비슷한 구문을 쿼리 하는 방법도 주목 하십시오. GraphQL 스키마를 정의 하는 것은 특정 유형의 사용 가능한 인수 및 필드를 해당 필드 유형과 함께 표현 하는 것입니다. 복잡 한 각 필드 유형에는 단순 스칼라 유형 좋아요 `String` 될 때까지 트리를 통해 정의 해야 합니다.

`input`선언은 좋아요 a `type` 이지만 인수에 대 한 입력으로 사용 될 수 있는 유형을 정의 합니다. 선언도 주목 `interface` 하십시오. 이 기능은 PHP에서 인터페이스와 동일한 기능을 수행 하거나 더 적게 사용 합니다. 다른 유형은이 인터페이스에서 상속 됩니다.

이 구문이 `[CartItemInput!]!` 복잡 하 게 보이지만 끝에서 매우 직관적입니다. 대괄호 내부 _는 `!`_ 배열의 모든 값이 null이 아니어야 하는 반면 _, 외부_ 에 있는 값은 배열 값 자체가 null이 아니어야 함을 선언 합니다 (예: 빈 배열).

>[!NOTE]
>
>스키마에 따라 데이터를 가져오고 서식을 지정 하는 방법에 대 한 논리와 이러한 논리가 특정 유형에 매핑되는 방법은 GraphQL 런타임 구현입니다. 그러나 구현에서는 중첩 필드에 대 한 이해를 명확 하 게 하는 개념적인 흐름을 팔로우 해야 합니다. 요청에 지정 된 각 필드를 검사 하는 루트 `Query` 또는 `Mutation` 유형과 관련 된 해결 작업이 수행 됩니다. 복합 유형으로 확인 되는 각 필드에 대해 모든 항목이 스칼라 값으로 해결 될 때까지 해당 형식에 대 한 비슷한 확인이 수행 됩니다.

{{$include /help/_includes/graphql-rest-related-links.md}}
