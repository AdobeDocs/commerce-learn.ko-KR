---
title: GraphQL을 사용한 스키마 언어
description: GraphQL과 관련된 스키마에 대해 알아봅니다. 몇 가지 흥미로운 패턴 및 스키마 읽기 방법과 함께 스키마에 대한 설명을 읽으십시오.
landing-page-description: GraphQL에 대한 소개입니다. 스키마 이해 및 일부 요소 해석 방법
short-description: GraphQL에 대한 소개입니다. 스키마 이해 및 일부 요소 해석 방법
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---

# 스키마 언어

사용된 쿼리 및 돌연변이는 GraphQL 런타임에서 쿼리를 해결하기 위해 사용하고 서버에서 구현하는 특정 데이터 그래프를 사용합니다. GraphQL 사양은 데이터 그래프의 유형 및 관계를 표현하기 위한 불가지론적 언어를 정의합니다.

다음은 지금까지 살펴본 쿼리와 돌연변이를 지원하는 축약된 유형 스키마입니다.

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

다음을 살펴볼 수 있습니다. [GraphQL 설명서](https://graphql.org/learn/schema/){target="_blank"} 여기에 표시되지 않는 일부 개념에 대한 구문을 포함하여 유형 시스템의 세부 정보에 대해 알아봅니다. 그러나 위의 예는 자명합니다. (또한 구문이 쿼리 구문과 얼마나 유사한지 확인합니다.) GraphQL 스키마를 정의하는 것은 해당 필드의 유형과 함께 해당 유형의 사용 가능한 인수 및 필드를 표현하는 문제일 뿐입니다. 각 복합 필드 형식은 다음과 같은 간단한 스칼라 형식에 도달할 때까지 트리를 통해 정의 등을 포함해야 합니다. `String`.

다음 `input` 선언은 모든 점에서 다음과 같다. `type` 인수의 입력으로 사용할 수 있는 형식을 정의합니다. 다음 사항을 참고하십시오. `interface` 선언입니다. 이것은 PHP의 인터페이스와 거의 동일한 기능을 제공합니다. 다른 형식은 이 인터페이스에서 상속합니다.

구문 `[CartItemInput!]!` 까다롭긴 하지만 결국에는 꽤 직관적이다. 다음 `!` _내부_ 대괄호는 배열의 모든 값이 null이 아니어야 하지만 _외부_ 는 배열 값 자체가 null이 아니어야 한다고 선언합니다(예: 빈 배열).

>[!NOTE]
>
>스키마에 따라 데이터를 가져오고 형식을 지정하는 방법 및 이러한 논리가 특정 유형에 매핑되는 방법에 대한 논리는 GraphQL 런타임 구현에 따라 다릅니다. 그러나 구현은 중첩된 필드에 대한 이해에 비추어 의미가 있는 개념 흐름(루트와 연관된 해결 작업)을 따라야 합니다 `Query` 또는 `Mutation` 요청에 지정된 각 필드를 검사하는 유형이 수행됩니다. 복합 유형으로 확인되는 각 필드의 경우, 모든 것이 스칼라 값으로 확인될 때까지 해당 유형에 대해 유사한 확인이 수행됩니다.

{{$include /help/_includes/graphql-rest-related-links.md}}
