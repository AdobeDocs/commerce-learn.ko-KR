---
title: Adobe Commerce 2.4.5용 IO 이벤트를 설치하는 방법 알아보기
description: Adobe Developer App Builder에서 사용하기 위해 Adobe Commerce 2.4.5에서 IO 이벤트에 필요한 모듈을 설치하는 방법을 알아봅니다
landing-page-description: 작성기를 사용하여 Adobe Commerce 2.4.5에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
short-description: 작성기를 사용하여 Adobe Commerce 2.4.5에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Adobe Commerce 2.4.5 설치

버전 2.4.5용 Composer를 사용하여 Adobe Commerce에 여러 새 모듈을 설치하는 방법을 알아봅니다. Adobe Commerce 애플리케이션에서 사용되는 필수 모듈을 설정합니다. [Adobe Commerce에 대한 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}에서 추가 설명서를 찾았습니다.

## 이 비디오는 누구의 것입니까?

* I/O 이벤트를 사용한 Adobe Commerce 및 Adobe Developer App Builder을 처음 사용하는 개발자

## 비디오 콘텐츠 {#video-content}

* 작성기를 사용한 필수 모듈 설치
* 온-프레미스 호스팅에 대해 실행할 명령
* Adobe Commerce Cloud에 대해 실행할 명령
* Adobe Commerce Cloud yaml 필수 편집

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## 유용한 명령 {#useful-commands}

자체 호스팅 환경을 사용하는지 또는 Adobe Commerce Cloud을 사용하는지에 따라 약간 다른 다양한 명령이 있습니다.

### On Premise 호스팅 {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### 클라우드의 Adobe Commerce {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
