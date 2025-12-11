---
title: Adobe Commerce에서 이벤트를 사용할 모듈을 만드는 방법을 알아봅니다.
description: 이벤트를 사용할 Commerce 모듈을 만드는 방법을 알아봅니다.
landing-page-description: 이벤트를 사용할 Adobe Commerce 모듈을 만드는 방법을 알아봅니다.
short-description: 이벤트를 사용할 Adobe Commerce 모듈을 만드는 방법을 알아봅니다.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Adobe Commerce 모듈 개발

사용자 지정 모듈 개발에서 이벤트를 등록하고, 지원되는 이벤트를 찾고, 새 XML 파일 `io_events.xml`을(를) 사용하는 방법을 알아봅니다. 또한 이 비디오에서는 개발자에게 이미 정의되어 있을 수 있는 모든 이벤트에 대한 구독을 취소하는 방법과 을 사용할 수 있는 등록된 이벤트를 찾는 방법을 보여 줍니다. [Adobe Commerce용 Adobe I/O Events 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}에서 추가 설명서를 찾았습니다.

## 이 비디오는 누구의 것입니까?

* I/O 이벤트를 사용하는 Adobe Commerce 및 Adobe Developer App Builder을 처음 사용하는 개발자입니다.

## 비디오 콘텐츠 {#video-content}

* Adobe Developer App Builder에서 사용하기 위해 Commerce에서 이벤트 등록
* 등록할 수 있는 이벤트 식별
* io_events.xml에 이벤트를 등록하는 방법 알아보기
* Commerce 인스턴스 `app/etc/config.php`에서 이벤트를 등록하는 방법 알아보기
* 이벤트 구독을 취소하는 방법 알아보기

>[!VIDEO](https://video.tv.adobe.com/v/3419838?captions=kor&quality=12&learn=on)

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
