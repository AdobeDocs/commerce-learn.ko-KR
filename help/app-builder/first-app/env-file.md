---
title: .env 파일
description: 이 샘플 응용 프로그램의 .env 파일에 있는 파일 유형에 대해 알아봅니다
landing-page-description: Adobe Commerce과 함께 사용되는 Adobe Developer App Builder 및 .env 파일에서 사용되는 컨텐츠 유형에 대해 알아봅니다
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: 037a7571c87f328dde0f39dd830c7379bd2230b6
workflow-type: tm+mt
source-wordcount: '153'
ht-degree: 0%

---


# 다음 `.env` 파일 {#env-file}

다음 `.env` 는 샘플 모듈에 속하지 않지만 Adobe Developer App Builder 애플리케이션에서 사용하는 데 중요한 특수 파일입니다. 이 파일에는 암호 및 기타 정보가 들어 있습니다. 이 파일을 코드 저장소에 커밋하지 마십시오.

## 이 비디오 누구?

* 개발자는 Analytics에 대해 알려고 하는 Adobe App Builder를 사용하여 경험이 제한된 Adobe Commerce을 처음 사용합니다 `.env` 파일.

## 비디오 컨텐츠

* .env 파일 및 그 목적 소개
* .env 파일을 생성하는 방법
* 파일을 추가하여 새 암호를 추가하는 방법
* 중요한 정보가 포함되어 있으므로 이 파일을 커밋하지 마십시오

>[!VIDEO](https://video.tv.adobe.com/v/3416593)

## 코드 샘플

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

파일의 샘플 모듈에서 사용되는 이러한 정적 값을 볼 수 있습니다 `actions/commerce.index.js`.

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
