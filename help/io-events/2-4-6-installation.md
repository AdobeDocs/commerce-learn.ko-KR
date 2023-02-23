---
title: Adobe Commerce 2.4.6용 IO 이벤트를 설치하는 방법 알아보기
description: Adobe Developer App Builder에서 사용할 Adobe Commerce 2.4.6에서 IO 이벤트에 필요한 모듈을 설치하는 방법을 알아봅니다
landing-page-description: Adobe Commerce 2.4.6에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: 4f73e2a6852d545d9b30b03158b6d8a35e3eba49
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 0%

---


# Adobe Commerce 2.4.6 설치

버전 2.4.6용 Composer를 사용하여 Adobe Commerce에서 여러 새 모듈을 설치하는 방법을 알아봅니다. 추가 설명서는 다음에 있습니다. [Adobe Commerce용 Adobe I/O 이벤트 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## 이 비디오 누구?

* I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder를 처음 사용하는 개발자.

## 비디오 컨텐츠 {#video-content}

* 온-프레미스 호스팅에 대해 실행할 명령
* Adobe Commerce Cloud에 대해 실행할 명령
* Adobe Commerce Cloud yaml 필수 편집

>[!VIDEO](https://video.tv.adobe.com/v/3415795)

## 유용한 명령 {#useful-commands}

자체 호스팅 환경을 사용하고 있는지 아니면 Adobe Commerce Cloud을 사용하고 있는지에 따라 약간 다른 다양한 명령이 있습니다.

### 온-프레미스 호스팅 {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### 클라우드의 Adobe Commerce {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
