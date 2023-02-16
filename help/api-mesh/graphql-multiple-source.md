---
title: API Mesh에서 사용할 여러 소스 GraphQL 만들기
description: Adobe Commerce 및 [!DNL Adobe App Builder]. 몇 가지 일반적인 오류와 이를 해결하는 방법에 대해 알아봅니다.
landing-page-description: Adobe Commerce 및 [!DNL Adobe App Builder]. 여러 소스가 있는 메쉬를 만들고 몇 가지 일반적인 오류를 해결하는 방법에 대해 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b3d5b22a597b342df6bf9846346d656dd4ce1383
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# 여러 소스가 있는 메쉬 만들기

이 비디오에서는 개발자가 Adobe Developer App Builder용 API Mesh에서 여러 소스가 포함된 메쉬를 만드는 방법을 이해하는 데 도움이 됩니다. 이 비디오에서는 여러 소스가 있는 메쉬를 만들고 오류를 식별하는 방법을 보여 줍니다. 자세한 내용 및 코드 샘플은 [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 이 비디오 누구?

* API 메쉬를 처음 사용하는 사람
* 여러 API와 GraphQL 소스를 결합하려는 개발자

## 비디오 컨텐츠

* 사용 방법 [변형](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/) 기본 소스 스키마를 수정하려면
* 이름 충돌, 스키마 가용성 및 기타 스키마 구문 문제와 같은 오류를 해결하는 방법
* 수정된 구성으로 메쉬 업데이트

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## json 구성 파일 만들기

API Mesh는 JSON 구성 파일을 사용하여 소스 핸들러를 정의합니다. JSON 파일에는 다음 항목이 포함되어 있습니다. `sources` 메쉬의 소스가 포함된 배열입니다. 다음은 여러 소스가 있는 메쉬의 예입니다.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
