---
title: Adobe Commerce 2.4.5용 IO 이벤트를 설치하는 방법 알아보기
description: Adobe Developer App Builder에서 사용할 Adobe Commerce 2.4.5에서 IO 이벤트에 필요한 모듈을 설치하는 방법을 알아봅니다
landing-page-description: Composer를 사용하여 Adobe Commerce 2.4.5에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.5 using composer.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.5"
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 0%

---


# Adobe Commerce 2.4.5 설치

버전 2.4.5용 Composer를 사용하여 Adobe Commerce에서 여러 새 모듈을 설치하는 방법을 알아봅니다. 이렇게 하면 Adobe Commerce 애플리케이션에서 사용되는 필수 모듈이 설정됩니다. 에 추가 설명서가 있습니다. [Adobe Commerce용 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 이 비디오 누구?

* I/O 이벤트를 사용하는 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하는 개발자

## 비디오 컨텐츠 {#video-content}

* 작성기를 사용하여 필요한 모듈 설치
* 온-프레미스 호스팅에 대해 실행할 명령
* Adobe Commerce Cloud에 대해 실행할 명령
* Adobe Commerce Cloud yaml 필수 편집

>[!VIDEO](https://video.tv.adobe.com/v/3415794)

## 유용한 명령 {#useful-commands}

자체 호스팅 환경을 사용하고 있는지 아니면 Adobe Commerce Cloud을 사용하고 있는지에 따라 약간 다른 다양한 명령이 있습니다.

### 온-프레미스 호스팅 {#on-premise}

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
