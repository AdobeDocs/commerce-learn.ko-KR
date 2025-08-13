---
title: 데이터 쿼리 방법
description: Adobe Commerce Optimizer 인스턴스에 대한 데이터를 쿼리하는 방법에 대해 알아봅니다.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 쿼리 데이터 Adobe Commerce Optimizer

Adobe Commerce Optimizer 인스턴스에서 GraphQL을 사용하여 데이터를 쿼리하는 방법을 알아봅니다.

## 이 비디오는 누구의 것입니까?

* Commerce 솔루션 설계자 및 개발자

## 비디오 콘텐츠

* GraphQL을 사용하여 데이터 쿼리
* jq를 사용하여 json을 읽기 쉽게 만들기

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## 코드 샘플

`{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-source-locale}}` 및 `{{your-search-query-string}}`과(와) 같은 값을 쿼리에 필요한 값과 교환하십시오.

기본 샘플 쿼리

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

출력을 예쁜 인쇄하기 위해 `jq`을(를) 사용하는 기본 샘플 쿼리

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 관련 컨텐츠

* [머천다이징 API 시작](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/ko/docs/commerce/optimizer/overview){target="_blank"}
