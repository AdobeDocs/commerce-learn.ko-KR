---
title: Create a downloadable product
description: Learn how to create a downloadable product using the REST API and the Adobe Commerce Admin.
kt: 14464
doc-type: video
duration: 946
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
TQID: https://experienceleague.adobe.com/YHtAD-NRQmIG58myhZk9X7-jJjwlk8S4NX9jYnZwnQc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 631
ht-degree: 0%

---

# Create a downloadable product

Learn how to create a downloadable product using the REST API and the Adobe Commerce Admin.

## 이 비디오는 누구의 것입니까?

* 웹 사이트 관리자
* 전자 상거래 머천다이저
* New Adobe Commerce developers who want to learn how to create products in Adobe Commerce using the REST API

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## Allowed downloadable domains

You must specify which domains are permitted to allow downloads. Domains are added to the project&#39;s `env.php` file via the command line. The `env.php` file details the domains allowed to contain downloadable content. An error occurs if a downloadable product is created using the REST API _before_  the `php.env` file is updated:

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

To set the domain, connect to the server: `bin/magento downloadable:domains:add www.example.com`

Once that is complete, the `env.php` is modified inside the _downloadable_ domains_ array.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Now that the domain is added to the `env.php`, you can create a downloadable product in the Adobe Commerce Admin or by using the REST API.

See [Configuration Reference](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ko#downloadable_domains) to learn more.

>[!IMPORTANT]
>On some versions of Adobe Commerce, you might get the following error when a product is edited in the Adobe Commerce Admin. The product is created using the REST API but the linked download has a `null` price.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

To fix this error, use the update link API: `POST V1/products/{sku}/downloadable-links.`

See the [Update a product download link using cURL](#update-downloadable-links) section for more info.

## Create a downloadable product using cURL (download from remote server)

This example shows how to create a downloadable product using cURL when the file to download is not on the same server. 이 사용 사례는 파일이 S3 버킷 또는 기타 디지털 에셋 관리자에 저장된 경우에 일반적입니다.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## cURL(Commerce 애플리케이션 서버에서 다운로드)을 사용하여 다운로드 가능한 제품을 만듭니다

이 예에서는 파일이 Adobe Commerce 애플리케이션과 동일한 서버에 저장되어 있을 때 cURL을 사용하여 Adobe Commerce 관리자로부터 다운로드 가능한 제품을 만드는 방법을 보여 줍니다.

이 사용 사례에서는 카탈로그를 관리하는 관리자가 `upload file`을(를) 선택하면 파일이 `pub/media/downloadable/files/links/` 디렉터리로 전송됩니다.  자동화는 다음 패턴을 기반으로 파일을 만들어 해당 위치로 이동합니다.

* 업로드된 각 파일은 파일 이름의 처음 두 문자를 기준으로 폴더에 저장됩니다.
* 업로드가 시작되면 Commerce 애플리케이션은 기존 폴더를 생성 또는 사용하여 파일을 전송합니다.
* 파일을 다운로드할 때 경로의 `link_file` 섹션은 `pub/media/downloadable/files/links/` 디렉터리에 추가된 경로의 일부를 사용합니다.

예를 들어 업로드된 파일의 이름이 `download-example.zip`인 경우:

* 파일이 `pub/media/downloadable/files/links/d/o/` 경로에 업로드되었습니다.
하위 디렉터리 `/d` 및 `/d/o`이(가) 없는 경우 만들어집니다.

* 파일의 최종 경로는 `/pub/media/downloadable/files/links/d/o/download-example.zip`입니다.

* 이 예제의 `link_url` 값은 `d/o/download-example.zip`입니다.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## curl을 사용하여 제품 가져오기

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Postman을 사용하여 제품 업데이트 {#update-downloadable-links}

끝점 사용 `rest/all/V1/products/{sku}/downloadable-links`
`SKU`은(는) 제품을 만들 때 생성된 제품 ID입니다. 예를 들어 아래 코드 샘플에서는 숫자 39이지만 웹 사이트의 ID를 사용하도록 업데이트되었는지 확인하십시오. 이렇게 하면 다운로드 가능한 제품에 대한 링크가 업데이트됩니다.

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## CURL을 사용하여 제품 다운로드 링크 업데이트

cURL을 사용하여 제품 다운로드 링크를 업데이트할 때 URL에는 업데이트 중인 제품에 대한 SKU가 포함됩니다.  다음 코드 예제에서 SKU는 `abcd12345`입니다. 명령을 제출하면 업데이트하려는 제품의 SKU와 일치하도록 값을 변경합니다.

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## 추가 리소스

* [다운로드 가능한 제품 유형](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html?lang=ko){target="_blank"}
* [다운로드 가능한 도메인 구성 안내서](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html?lang=ko#downloadable_domains){target="_blank"}
* [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
