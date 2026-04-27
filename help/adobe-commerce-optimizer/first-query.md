---
title: How to query data
description: Learn how to query data for an Adobe Commerce Optimizer instance.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 243
last-substantial-update: 2025-08-13T00:00:00.000Z
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
TQID: https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 114
ht-degree: 0%

---

# Query data Adobe Commerce Optimizer

Learn how to query data using GraphQL on an Adobe Commerce Optimizer instance.

## 이 비디오는 누구의 것입니까?

* Commerce Solution Architect and developers

## 비디오 콘텐츠

* Query data using GraphQL
* Using jq to make json easier to read

>[!VIDEO](https://video.tv.adobe.com/v/3470807?captions=kor&learn=on)

## 코드 샘플

Be sure to exchange values like `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-view-id}}` and `{{your-search-query-string}}` with the values needed on your query.

Basic sample query

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Basic sample query using `jq` to pretty-print the output

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 관련 컨텐츠

* [Getting started with Merchandising API](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/ko/docs/commerce/optimizer/overview){target="_blank"}
