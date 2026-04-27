---
title: Adobe Commerce 구성
description: Adobe Developer App Builder에서 이벤트를 사용할 수 있도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
landing-page-description: Adobe Developer App Builder에서 사용하기 위해 이벤트 메커니즘을 사용하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
short-description: Adobe Developer App Builder에서 사용하기 위해 이벤트 메커니즘을 사용하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다.
kt: 11889
doc-type: tutorial
duration: 299
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 151
ht-degree: 0%

---

# Adobe Commerce 구성

이벤트를 노출하도록 Adobe Commerce을 구성하는 방법에 대해 알아봅니다. [Adobe Commerce용 Adobe I/O Events 설치](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}에서 추가 설명서를 찾았습니다.

## 이 비디오는 누구의 것입니까?

* 개발자는 I/O 이벤트를 사용하여 Adobe Commerce 및 Adobe Developer App Builder을 처음 사용하므로 Adobe App Builder 프로젝트를 만들어야 합니다.

## 비디오 콘텐츠 {#video-content}

* Commerce 관리자의 Adobe I/O 이벤트 구성
* Commerce 관리자에 개인 키 저장
* Commerce 관리자에 고유 식별자 저장
* 이벤트 공급자 만들기

>[!VIDEO](https://video.tv.adobe.com/v/3415799?learn=on)

## 유용한 명령 {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
