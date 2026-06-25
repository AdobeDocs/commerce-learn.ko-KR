---
title: 데이터 쿼리 방법
description: jq를 사용하여 JSON 응답의 형식을 지정하고 검색 쿼리 변수를 구성하는 방법을 포함하여 GraphQL을 사용하여 Adobe Commerce Optimizer 제품 데이터를 쿼리하는 방법에 대해 알아봅니다.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 204
last-substantial-update: 2025-08-13
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
TQID: https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 127
ht-degree: 0%

---

# 쿼리 데이터 Adobe Commerce Optimizer

Adobe Commerce Optimizer 인스턴스에서 GraphQL을 사용하여 데이터를 쿼리하는 방법을 알아봅니다.

## 이 비디오는 누구의 것입니까?

* Commerce 솔루션 설계자 및 개발자

## 비디오 콘텐츠

* GraphQL을 사용하여 데이터 쿼리
* jq를 사용하여 json을 읽기 쉽게 만들기

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on)

## 코드 샘플

`{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-view-id}}` 및 `{{your-search-query-string}}`과(와) 같은 값을 쿼리에 필요한 값과 교환하십시오.

기본 샘플 쿼리

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

출력을 예쁜 인쇄하기 위해 `jq`을(를) 사용하는 기본 샘플 쿼리

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## 관련 컨텐츠

* [머천다이징 API 시작하기](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}

