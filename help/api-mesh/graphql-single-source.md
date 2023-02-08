---
title: API Mesh에서 사용할 GraphQL 단일 소스 요청 만들기
description: Adobe Commerce 및 [!DNL Adobe App Builder]. 소스가 하나인 요청을 만드는 방법에 대해 알아봅니다.
landing-page-description: Adobe Commerce 및 [!DNL Adobe App Builder]. 소스가 하나인 요청을 만드는 방법에 대해 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 단일 소스 GraphQL API 메쉬 만들기

이 비디오에서는 개발자가 GraphQL 역방향 프록시를 만들고 단일 소스를 가지는 방법을 이해하는 데 도움이 됩니다. GraphQL Mesh가 예상대로 작동하려면 유효한 GraphQL 스키마를 사용하여 공개적으로 액세스할 수 있는 URL이 필요합니다. 또한 비디오에서는 상거래 웹 사이트에서 사용할 초기 json을 설정하는 방법을 설명합니다. 비디오에 사용된 기본 코드 샘플은 [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 이 비디오 누구?

* API 메쉬를 처음 사용하는 사람
* 여러 graphql 소스를 사용하려는 개발자
* Graphql을 통해 네트워크 탭을 필터링하고 필터링하는 방법을 알고 있어야 하는 모든 사용자

## 비디오 컨텐츠

* API를 역방향 프록시로 사용
* Adobe Developer 명령줄 인터페이스를 사용한 JSON 구성
* 새로 만든 GraphQL 종단점에 액세스

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## json 구성 파일 만들기

Adobe App Builder에서 모든 소스에 대해 알아보려면 JSON 구성에서 이를 정의합니다. 각 소스는 배열의 요소이며 하나 이상을 가질 수 있습니다. 다음은 단일 소스의 예입니다

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
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
