---
title: GraphQL을 사용하여 쿼리 수행
description: Adobe Commerce에서 GraphQL을 사용하여 쿼리를 수행하는 방법 및 [!DNL Magento Open Source]. GET 및 POST 호출을 사용하는 GraphQL에 대한 소개입니다.
landing-page-description: Adobe Commerce에서 GraphQL을 사용하여 쿼리를 수행하는 방법 및 [!DNL Magento Open Source]. GET 및 POST 호출을 사용하는 GraphQL에 대한 소개입니다.
short-description: Adobe Commerce에서 GraphQL을 사용하여 쿼리를 수행하는 방법 및 [!DNL Magento Open Source]. GET 및 POST 호출을 사용하는 GraphQL에 대한 소개입니다.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '937'
ht-degree: 0%

---

# GraphQL 쿼리

본격적인 예제를 사용하여 GraphQL 쿼리 구문을 바로 살펴보겠습니다. (https://venia.magento.com/graphql에 대해 직접 시도할 수 있습니다.)

다음과 같은 GraphQL 쿼리를 준수하십시오. 쿼리는 하나씩 분류됩니다.

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

위의 쿼리에 대한 GraphQL 서버의 가능한 응답은 다음과 같습니다.

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

위의 예제는 서버에서 정의된 Adobe Commerce용 기본 GraphQL 스키마에 의존합니다. 이 요청에서는 여러 유형의 데이터를 한 번에 쿼리합니다. 쿼리는 원하는 필드를 정확히 표시하며 반환된 데이터의 형식은 쿼리 자체와 비슷합니다.

>[!NOTE]
>
>GraphQL 클라이언트는 전송되는 실제 HTTP 요청의 형식을 난독화하지만 쉽게 찾을 수 있습니다. 브라우저 기반 클라이언트를 사용하는 경우 다음을 확인합니다. [!UICONTROL Network] 쿼리를 전송할 때 탭입니다. 요청에 &quot;query: `{string}`&quot;, 여기서 `{string}` 는 전체 쿼리의 원시 문자열입니다. 요청이 GET으로 전송되는 경우 쿼리가 쿼리 문자열 매개 변수 &quot;query&quot;로 대신 인코딩될 수 있습니다. REST와 달리 HTTP 요청 유형은 쿼리의 내용만 중요하지 않습니다.


## 원하는 항목을 쿼리하는 중

`country` 및 `categories` 예제에서는 서로 다른 두 종류의 데이터에 대해 서로 다른 두 개의 &quot;쿼리&quot;를 나타냅니다. 각 데이터 유형에 대해 별도의 끝점과 명시적 끝점을 정의하는 REST와 같은 기존 API 패러다임과 다릅니다. GraphQL에서는 한 번에 많은 유형의 데이터를 가져올 수 있는 표현식을 사용하여 단일 끝점을 쿼리할 수 있는 유연성을 제공합니다.

마찬가지로 쿼리는 두 필드 모두에 대해 원하는 필드를 정확히 지정합니다 `country` (`id` 및 `full_name_english`) 및 `categories` (`items`: 자체 하위 선택 필드가 있고 사용자가 다시 받는 데이터는 해당 필드 사양을 미러링합니다. 이러한 데이터 유형에 사용할 수 있는 필드가 더 있을 수 있지만 요청한 내용만 반환됩니다.


>[!NOTE]
>
>다음에 대한 반환 값이 `items` 실제로 _배열_ 을 선택할 수 있지만 하위 필드를 직접 선택할 수도 있습니다. 필드 유형이 목록인 경우 GraphQL은 목록의 각 항목에 적용할 하위 선택 사항을 암묵적으로 이해합니다.

## 인수

반환할 필드는 각 유형의 중괄호 안에 지정되지만, 해당 필드의 명명된 인수와 값은 형식 이름 뒤에 있는 괄호 안에 지정됩니다. 인수는 종종 선택 사항이며 쿼리 결과가 필터링, 서식 지정 또는 변환되는 방식에 영향을 줍니다.

을(를) 통과하고 있습니다. `id` 인수 `country`, 쿼리할 특정 국가 지정 및 `filters` 인수 `categories`.

## 필드 끝까지

While you would think of `country` 및 `categories` 별도의 쿼리나 엔티티로서 쿼리에 표시된 전체 트리는 실제로 필드로만 구성됩니다. 의 표현식 `products` 문법적으로 와 다르지 않습니다. `categories`. 둘 다 밭이고, 축조에도 차이가 없다.

모든 GraphQL 데이터 그래프에는 단일 &quot;루트&quot; 유형(일반적으로 다음과 같음)이 있습니다. `Query`)를 클릭하여 트리를 시작하고 엔티티로 간주되는 유형이 이 루트의 필드에 할당됩니다. 예제 쿼리는 실제로 루트 유형에 대해 하나의 일반 쿼리를 만들고 필드를 선택하는 것입니다 `country` 및 `categories`. 그런 다음 해당 필드의 하위 필드를 선택하고, 잠재적으로 여러 수준 깊이를 선택합니다. 필드의 반환 형식이 복잡한 형식(예: 스칼라 형식이 아닌 자체 필드가 있는 형식)이면 원하는 필드를 계속 선택합니다.

중첩된 필드에 대한 이러한 개념을 사용하여 인수를 전달할 수 있습니다. `products` (`pageSize` 및 `currentPage`)을 참조하십시오. `categories` 필드.

![GraphQL 필드 트리](../assets/graphql-field-tree.png)

## 변수

다른 쿼리를 시도해 보겠습니다.

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

가장 먼저 눈에 띄는 것은 키워드를 추가한 것입니다 `query` 쿼리의 여는 중괄호 앞에 작업 이름(`getProducts`). 이 작업 이름은 임의로 지정되며 서버 스키마의 어떤 이름과도 일치하지 않습니다. 변수 도입을 지원하기 위해 이 구문이 추가되었습니다.

이전 쿼리에서는 필드의 인수에 대한 값을 문자열 또는 정수로 직접 하드 코딩했습니다. 그러나 GraphQL 사양에서는 변수를 사용하여 사용자 입력을 기본 쿼리에서 구분하는 데 대한 첫 번째 클래스 지원을 제공합니다.

새 쿼리에서 전체 쿼리의 여는 중괄호 앞에 괄호를 사용하여 `$search` 변수(변수는 항상 달러 기호 접두사 구문을 사용함). 에 제공 중인 변수는 이 변수입니다 `search` 인수 `products`.

쿼리에 변수가 있으면 GraphQL 요청에 쿼리 자체와 함께 별도의 JSON 인코딩 값 사전이 포함되어야 합니다. 위의 쿼리의 경우 쿼리 본문 외에 다음 JSON의 변수 값을 전송할 수 있습니다.

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>자체 Adobe Commerce 인스턴스가 아닌 Venia 예제 사이트에 대해 이러한 쿼리를 시도하려는 경우 반환된 결과가 비어 있을 수 있습니다. `related_products`.

테스트에 사용하는 모든 GraphQL 인식 클라이언트(예: Altair 및 GraphiQL)에서 UI는 쿼리와 별도로 JSON 변수를 입력할 수 있도록 지원합니다.

GraphQL 쿼리에 대한 실제 HTTP 요청에 &quot;쿼리: `{string}`&quot;본문에 변수 사전이 포함된 모든 요청에는 단순히 &quot;변수: `{json}`&quot;동일한 본문에서, `{json}` 는 변수 값이 있는 JSON 문자열입니다.

새 쿼리는 또한 _조각_ (`productDetails`)을 클릭하여 여러 위치에서 동일한 필드 선택 사항을 재사용할 수 있습니다. [조각에 대해 자세히 알아보기](https://graphql.org/learn/queries/#fragments){target="_blank"} GraphQL 설명서에서 확인할 수 있습니다.

{{$include /help/_includes/graphql-rest-related-links.md}}
