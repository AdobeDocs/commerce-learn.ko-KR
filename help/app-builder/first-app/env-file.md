---
title: .env 파일
description: 암호 관리 및 소스 제어에 대한 우발적인 커밋 방지를 포함하여 Adobe Developer App Builder 애플리케이션에 대한 .env 파일을 생성하고 구성하는 방법에 대해 알아봅니다.
jira: KT-21681
doc-type: Tutorial
duration: 177
last-substantial-update: 2023-03-13T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner, Intermediate
exl-id: 934fcdd1-ee61-4914-89ce-f6f04b1bc763
TQID: https://experienceleague.adobe.com/Sup5quVNU60RYmkCJaYGgLq2gHCpCvaIKEFLH2MAifQ
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: e03f0a058d1a08b1a67fd278c1b6127566a370ac
workflow-type: tm+mt
source-wordcount: 147
ht-degree: 0%

---

# .env 파일 생성 및 구성 {#env-file}

`.env`은(는) 샘플 모듈의 일부가 아니지만 Adobe Developer App Builder 응용 프로그램에서 사용하는 데 중요한 특수 파일입니다. 이 파일에는 비밀 및 기타 정보가 포함되어 있습니다. 이 파일을 코드 저장소에 커밋하지 마십시오.

## 이 비디오는 누구의 것입니까?

* Adobe App Builder을 사용하여 경험이 제한된 Adobe Commerce을 처음 사용하는 개발자로서 `.env` 파일에 대해 알아보고자 하는 사람.

## 비디오 콘텐츠

* .env 파일 및 해당 용도 소개
* .env 파일을 생성하는 방법
* 새 암호를 추가하려면 파일을 추가합니다
* 이 파일에는 중요한 정보가 포함되어 있으므로 커밋하지 마십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3421070?captions=kor&learn=on)

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

`actions/commerce.index.js` 파일의 샘플 모듈에서 사용되는 이러한 정적 값을 볼 수 있습니다.

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
