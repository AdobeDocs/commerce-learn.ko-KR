---
title: GraphQL을 사용한 스키마 언어
description: GraphQL과 관련된 스키마에 대해 알아봅니다. 스키마에 대한 설명과 함께 흥미로운 패턴 및 스키마를 읽는 방법을 읽어 보십시오.
landing-page-description: GraphQL을 소개합니다. 스키마 이해 및 일부 요소를 해석하는 방법
short-description: This is an introduction to GraphQL. Understanding the schema and how to interpret some of the elements
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# 스키마 언어

사용된 쿼리 및 변형은 GraphQL 런타임에서 쿼리를 해결하는 데 사용하고 사용하는 특정 데이터 그래프를 기반으로 합니다. GraphQL 사양은 데이터 그래프의 유형과 관계를 표현하기 위한 다양한 언어를 정의합니다.

다음은 지금까지 본 쿼리 및 돌연변이를 지원하는 축약형 스키마입니다.

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

당신은 [GraphQL 설명서](https://graphql.org/learn/schema/){target="_blank"} 여기에는 표시되지 않는 일부 개념에 대한 구문을 포함하여 유형 시스템의 세부 사항을 알려주십시오. 그러나 위의 예는 자명하다. (또한 쿼리 구문과 구문이 얼마나 유사한지 확인합니다.) GraphQL 스키마를 정의하는 것은 해당 필드의 유형과 함께 주어진 유형의 사용 가능한 인수 및 필드를 표현하는 문제입니다. 각 복합 필드 유형에는 정의 등이 있어야 하며, 트리를 통해 다음과 같은 간단한 스칼라 형식에 도달할 때까지 `String`.

다음 `input` 선언은 모든 면에서 `type` 그러나 은 인수에 대한 입력으로 사용할 수 있는 형식을 정의합니다. 또한 `interface` 선언입니다. 이 기능은 PHP의 인터페이스와 거의 동일한 기능을 제공합니다. 다른 형식은 이 인터페이스에서 상속됩니다.

구문 `[CartItemInput!]!` 까다롭게 보이지만 결국에는 상당히 직관적입니다. 다음 `!` _내부_ 대괄호는 배열의 모든 값이 null이 아니고, _외부_ 는 배열 값 자체가 null이 아니어야 함을 나타냅니다(예: 빈 배열).

>[!NOTE]
>
>스키마에 따라 데이터를 가져오고 포맷하는 방법과 이러한 논리를 특정 유형에 매핑하는 방법에 대한 논리는 GraphQL 런타임 구현에 달려 있습니다. 그러나 구현은 중첩 필드에 대한 이해에 비추어 의미가 있는 개념 흐름을 따라야 합니다. 루트와 연결된 해결 작업 `Query` 또는 `Mutation` 요청에 지정된 각 필드를 검사하는 유형이 수행됩니다. 복합 유형으로 확인되는 각 필드에 대해 모든 필드가 스칼라 값으로 확인될 때까지 해당 유형에 대해 유사한 확인이 수행됩니다.

{{$include /help/_includes/graphql-rest-related-links.md}}
