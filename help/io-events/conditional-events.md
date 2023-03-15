---
title: Adobe Commerce에서 조건부 이벤트 사용 방법 알아보기
description: Adobe Developer App Builder에서 사용할 조건부 이벤트를 사용하는 방법을 알아봅니다.
landing-page-description: Adobe Commerce 조건부 이벤트 사용 방법을 알아봅니다.
short-description: Learn how to use Adobe Commerce conditional events.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---


# Adobe Commerce 조건부 이벤트

Adobe Developer App Builder에서 사용할 수 있는 Adobe Commerce의 조건부 이벤트에 대해 알아봅니다. 에 추가 설명서가 있습니다. [Adobe Commerce용 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/conditional-events/){target="_blank"}.

## 이 비디오 누구?

* 개발자는 I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하므로 Adobe App Builder 프로젝트를 만들어야 합니다.

## 비디오 컨텐츠 {#video-content}

* 조건부 이벤트에 대해 알아보기
* 새 XML 파일 io_events.xml에 대한 적절한 사용법을 알아보십시오.
* 조건부 이벤트 구성 방법 알아보기
* 조건부 이벤트에서 사용할 규칙 정의
* 상거래 인스턴스에서 이벤트를 등록하는 방법을 알아봅니다 `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3415806)

## 유용한 명령 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
