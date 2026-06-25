---
title: Adobe Commerce 2.4.6용 IO 이벤트를 설치하는 방법 알아보기
description: Adobe Developer App Builder에서 사용하기 위해 Adobe Commerce 2.4.6에서 IO 이벤트에 필요한 모듈을 설치하는 방법을 알아봅니다
landing-page-description: Adobe Commerce 2.4.6에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
short-description: Adobe Commerce 2.4.6에 필요한 여러 모듈을 설치하는 방법을 알아봅니다.
kt: 11887
doc-type: tutorial
duration: 167
audience: all
last-substantial-update: 2023-02-22T00:00:00.000Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
TQID: https://experienceleague.adobe.com/19IVX54xo-RAJAOuXNIABdy11ExmBRAbWugi4eZ0cF0
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 166
ht-degree: 0%

---

# Adobe Commerce 2.4.6 설치

버전 2.4.6용 Composer를 사용하여 Adobe Commerce에 여러 새 모듈을 설치하는 방법을 알아봅니다. [Adobe Commerce용 Adobe I/O Events 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}에서 추가 설명서를 찾았습니다.

## 이 비디오는 누구의 것입니까?

* I/O 이벤트를 사용하는 Adobe Commerce 및 Adobe Developer App Builder을 처음 사용하는 개발자입니다.

## 비디오 콘텐츠 {#video-content}

* 온-프레미스 호스팅에 대해 실행할 명령
* Adobe Commerce Cloud에 대해 실행되는 명령
* Adobe Commerce Cloud yaml 필수 편집

>[!VIDEO](https://video.tv.adobe.com/v/3415795?learn=on)

## 유용한 명령 {#useful-commands}

자체 호스팅 환경을 사용하는지 또는 Adobe Commerce Cloud를 사용하는지에 따라 약간 다른 다양한 명령이 있습니다.

### On Premise 호스팅 {#on-premise}

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

