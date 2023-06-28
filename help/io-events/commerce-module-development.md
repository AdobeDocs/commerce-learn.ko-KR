---
title: Adobe Commerce에서 이벤트를 사용할 모듈을 만드는 방법을 알아봅니다.
description: 이벤트를 사용할 상거래 모듈을 만드는 방법을 알아봅니다.
landing-page-description: 이벤트를 사용할 Adobe Commerce 모듈을 만드는 방법을 알아봅니다.
short-description: 이벤트를 사용할 Adobe Commerce 모듈을 만드는 방법을 알아봅니다.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Adobe Commerce 모듈 개발

이벤트를 등록하고, 지원되는 이벤트를 찾고, 새 XML 파일을 사용하는 방법에 대해 알아봅니다 `io_events.xml` 사용자 지정 모듈 개발에서. 또한 이 비디오에서는 개발자에게 이미 정의되어 있을 수 있는 모든 이벤트에 대한 구독을 취소하는 방법과 을 사용할 수 있는 등록된 이벤트를 찾는 방법을 보여 줍니다. 추가 설명서는 다음 위치에서 찾을 수 있습니다. [Adobe Commerce에 대한 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 이 비디오는 누구의 것입니까?

* I/O 이벤트를 사용하는 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하는 개발자입니다.

## 비디오 콘텐츠 {#video-content}

* Adobe Developer App Builder에서 사용할 이벤트를 상거래에 등록
* 등록할 수 있는 이벤트 식별
* io_events.xml에 이벤트를 등록하는 방법 알아보기
* 상거래 인스턴스에서 이벤트를 등록하는 방법 알아보기 `app/etc/config.php`
* 이벤트 구독을 취소하는 방법 알아보기

>[!VIDEO](https://video.tv.adobe.com/v/3415802?quality=12&learn=on)

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
