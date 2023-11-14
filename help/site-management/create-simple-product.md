---
title: 간단한 제품 만들기
description: REST API 및 상거래 관리자를 사용하여 간단한 제품을 만드는 방법을 알아봅니다.
kt: 14446
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-13T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: 9f5d0e83995d12b5884c53fc8bcb0e9d1913768e
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# 간단한 제품 만들기

REST API 및 상거래 관리자를 사용하여 간단한 제품을 만드는 방법을 알아봅니다.

## 이 비디오는 누구의 것입니까?

- 웹 사이트 관리자
- 전자 상거래 머천다이저
- Adobe Commerce에서 제품을 만들기 위해 REST를 사용하는 방법을 배워야 하는 Adobe Commerce의 새로운 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3425650?learn=on)

## Curl 코드 샘플을 사용하여 제품 만들기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "some-product-sku",
    "name": "My curl created product test",
    "attribute_set_id": 4,
    "price": 222,
    "type_id": "simple"
  }
}
```

## 제품을 가져오려면 코드 샘플을 curl하십시오.

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## 추가 리소스

- [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
