---
title: app.config.yaml 파일
description: 이 샘플 응용 프로그램의 app.config.yaml 파일에 있는 파일 유형에 대해 알아봅니다.
landing-page-description: Adobe Commerce에서 사용되는 Adobe Developer App Builder와 app.config.yaml에 포함되는 파일 유형에 대해 알아봅니다.
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-3-13
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: ff5f1811-ca93-494e-8e5c-a5e0c7bb673d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---

# app.config.yaml 파일에 대한 설명 및 사용 {#app-config-yaml}

이 파일은 응용 프로그램의 구성을 결정합니다.

## 이 비디오는 누구의 것입니까?

* Adobe Commerce을 처음 사용하는 개발자로서 Adobe App Builder에 대한 경험이 부족하여 `app.config.yaml` 샘플 응용 프로그램에서

## 비디오 콘텐츠

* 다음 `app.config.yaml` 논의된 파일
* 다른 항목에 대한 정의는 어떻게 연결됩니까? `.js` 파일

>[!VIDEO](https://video.tv.adobe.com/v/3416592?quality=12&learn=on)

## 코드 샘플

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

파일의 샘플 모듈에서 사용되는 이러한 정적 값을 볼 수 있습니다 `actions/commerce.index.js`

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
