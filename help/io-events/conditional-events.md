---
title: Adobe Commerce에서 조건부 이벤트를 사용하는 방법 알아보기
description: Adobe Developer App Builder에서 사용할 조건부 이벤트를 사용하는 방법을 알아봅니다.
landing-page-description: Adobe Commerce 조건부 이벤트를 사용하는 방법을 알아봅니다.
short-description: Adobe Commerce 조건부 이벤트를 사용하는 방법을 알아봅니다.
kt: 11890
doc-type: tutorial
duration: 421
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
TQID: https://experienceleague.adobe.com/GuN9--5xQaBnbFvQrkGuqMcDMMe-1tVGPk-hvqs4-UY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 144
ht-degree: 0%

---

# Adobe Commerce 조건부 이벤트

Adobe Developer App Builder에서 사용할 수 있는 Adobe Commerce의 조건부 이벤트에 대해 알아봅니다. [Adobe Commerce용 Adobe I/O Events 설치](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}에서 추가 설명서를 찾았습니다.

## 이 비디오는 누구의 것입니까?

* 개발자는 I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder을 처음 사용하므로 Adobe App Builder 프로젝트를 만들어야 합니다.

## 비디오 콘텐츠 {#video-content}

* 조건부 이벤트에 대해 알아보기
* 새 XML 파일 io_events.xml에 대한 적절한 사용 방법 알아보기
* 조건부 이벤트를 구성하는 방법 알아보기
* 조건부 이벤트에 사용할 규칙 정의
* Commerce 인스턴스 `app/etc/config.php`에서 이벤트를 등록하는 방법 알아보기

>[!VIDEO](https://video.tv.adobe.com/v/3415806?learn=on)

## 유용한 명령 {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
