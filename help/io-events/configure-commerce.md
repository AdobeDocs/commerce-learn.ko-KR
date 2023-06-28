---
title: Adobe Commerce 구성
description: Adobe Developer App Builder에서 이벤트를 사용할 수 있도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
landing-page-description: Adobe Developer App Builder에서 사용하기 위해 이벤트 메커니즘을 사용하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
short-description: Adobe Developer App Builder에서 사용하기 위해 이벤트 메커니즘을 사용하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# Adobe Commerce 구성

이벤트를 노출하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다. 추가 설명서는 다음 위치에서 찾을 수 있습니다. [Adobe Commerce에 대한 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 이 비디오는 누구의 것입니까?

* 개발자는 I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하므로 Adobe App Builder 프로젝트를 만들어야 합니다.

## 비디오 콘텐츠 {#video-content}

* Commerce 관리자의 Adobe I/O 이벤트 구성
* 상거래 관리자에 개인 키 저장
* Commerce 관리자에 고유 식별자 저장
* 이벤트 공급자 만들기

>[!VIDEO](https://video.tv.adobe.com/v/3415799?quality=12&learn=on)

## 유용한 명령 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
