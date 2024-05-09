---
title: 다운로드 가능한 제품 만들기
description: REST API 및 Adobe Commerce 관리자를 사용하여 다운로드 가능한 제품을 만드는 방법을 알아봅니다.
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: eba043cd4169cd762653557bf9283b8d6a208ef0
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# 다운로드 가능한 제품 만들기

REST API 및 Adobe Commerce 관리자를 사용하여 다운로드 가능한 제품을 만드는 방법을 알아봅니다.

## 이 비디오는 누구의 것입니까?

- 웹 사이트 관리자
- 전자 상거래 머천다이저
- REST API를 사용하여 Adobe Commerce에서 제품을 만드는 방법을 배우고자 하는 새로운 Adobe Commerce 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## 허용된 다운로드 가능한 도메인

다운로드를 허용할 도메인을 지정해야 합니다. 도메인이 프로젝트의 `env.php` 명령줄을 통해 파일을 생성합니다. 다음 `env.php` 파일 다운로드 가능한 콘텐츠를 포함할 수 있는 도메인에 대한 세부 정보. REST API를 사용하여 다운로드 가능한 제품을 만들면 오류가 발생합니다 _다음 이전_  다음 `php.env` 파일이 업데이트되었습니다.

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

도메인을 설정하려면 서버에 연결합니다. `bin/magento downloadable:domains:add www.example.com`

이 작업이 완료되면 `env.php` 이(가) 다음 내에서 수정되었습니다. _downloadable_domains_ 배열입니다.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

이제에 도메인이 추가됩니다. `env.php`, Adobe Commerce 관리자 또는 REST API를 사용하여 다운로드 가능한 제품을 만들 수 있습니다.

다음을 참조하십시오 [구성 참조](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains) 자세히 알아보십시오.

>[!IMPORTANT]
>일부 Adobe Commerce 버전에서는 Adobe Commerce 관리에서 제품을 편집할 때 다음 오류가 발생할 수 있습니다. 제품이 REST API를 사용하여 만들어지지만 연결된 다운로드에는 `null` 가격.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

이 오류를 수정하려면 업데이트 링크 API를 사용하십시오. `POST V1/products/{sku}/downloadable-links.`

다음을 참조하십시오. [cURL을 사용하여 제품 다운로드 링크 업데이트](#update-downloadable-links) 섹션 을 참조하십시오.

## cURL을 사용하여 다운로드 가능한 제품 만들기(원격 서버에서 다운로드)

이 예에서는 다운로드할 파일이 동일한 서버에 없는 경우 cURL을 사용하여 다운로드 가능한 제품을 만드는 방법을 보여 줍니다. 이 사용 사례는 파일이 S3 버킷 또는 기타 디지털 에셋 관리자에 저장된 경우에 일반적입니다.

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

이 사용 사례에서는 카탈로그를 관리하는 관리자가 `upload file`, 파일이 로 전송됩니다. `pub/media/downloadable/files/links/` 디렉토리.  자동화는 다음 패턴을 기반으로 파일을 만들어 해당 위치로 이동합니다.

- 업로드된 각 파일은 파일 이름의 처음 두 문자를 기준으로 폴더에 저장됩니다.
- 업로드가 시작되면 Commerce 애플리케이션은 기존 폴더를 생성 또는 사용하여 파일을 전송합니다.
- 파일을 다운로드할 때 `link_file` 경로의 섹션은 다음에 추가된 경로의 부분을 사용합니다. `pub/media/downloadable/files/links/` 디렉토리.

예를 들어 업로드된 파일의 이름이 인 경우 `download-example.zip`:

- 파일이 경로에 업로드됩니다. `pub/media/downloadable/files/links/d/o/`.
하위 디렉터리 `/d` 및 `/d/o` 존재하지 않는 경우 생성됩니다.

- 파일의 최종 경로는 다음과 같습니다. `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- 다음 `link_url` 이 예제의 값은 다음과 같습니다 `d/o/download-example.zip`

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
다음 `SKU` 는 제품을 만들 때 생성된 제품 ID입니다. 예를 들어 아래 코드 샘플에서는 숫자 39이지만 웹 사이트의 ID를 사용하도록 업데이트되었는지 확인하십시오. 이렇게 하면 다운로드 가능한 제품에 대한 링크가 업데이트됩니다.

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

cURL을 사용하여 제품 다운로드 링크를 업데이트할 때 URL에는 업데이트 중인 제품에 대한 SKU가 포함됩니다.  다음 코드 예제에서 SKU는 입니다. `abcd12345`. 명령을 제출하면 업데이트하려는 제품의 SKU와 일치하도록 값을 변경합니다.

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

- [다운로드 가능한 제품 유형](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html){target="_blank"}
- [다운로드 가능한 도메인 구성 안내서](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains){target="_blank"}
- [Adobe Developer REST 자습서](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
