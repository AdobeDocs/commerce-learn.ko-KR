---
title: 이벤트를 사용하기 위해 Adobe Commerce에서 모듈을 만드는 방법을 알아봅니다.
description: 이벤트를 사용하도록 상거래 모듈을 만드는 방법을 알아봅니다.
landing-page-description: 이벤트를 사용하도록 Adobe Commerce 모듈을 만드는 방법을 알아봅니다.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
source-git-commit: f1295c93652a9a2ae40a82009a8eb6895f59b909
workflow-type: tm+mt
source-wordcount: '162'
ht-degree: 0%

---


# Adobe Commerce 모듈 개발

이벤트를 등록하고 지원되는 이벤트를 찾고 새 XML 파일을 사용하는 방법을 알아봅니다 `io_events.xml` 사용자 지정 모듈 개발에서 참조할 수 있습니다. 또한 비디오에서는 개발자가 사용할 수 있는 등록된 이벤트를 찾고 이미 정의된 이벤트를 구독하는 방법을 개발자에게 보여줍니다. 에 추가 설명서가 있습니다. [Adobe Commerce용 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 이 비디오 누구?

* I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하는 개발자.

## 비디오 컨텐츠 {#video-content}

* Adobe Developer App Builder에서 사용할 Commerce에서 이벤트 등록
* 등록할 수 있는 이벤트를 식별합니다
* io_events.xml에서 이벤트를 등록하는 방법을 알아봅니다.
* 상거래 인스턴스에서 이벤트를 등록하는 방법을 알아봅니다 `app/etc/config.php`
* 이벤트 구독 취소 방법 알아보기

>[!VIDEO](https://video.tv.adobe.com/v/3415802)

## 유용한 명령 {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
