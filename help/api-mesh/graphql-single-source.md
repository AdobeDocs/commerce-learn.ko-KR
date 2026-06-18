---
title: API Mesh에서 GraphQL 단일 소스 메쉬 만들기
description: Adobe Commerce 및 Adobe App Builder에서 API Mesh를 사용하는 방법을 알아봅니다. 단일 GraphQL 소스로 메쉬를 만들고 새 끝점에 액세스하는 방법에 대해 알아봅니다.
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%

---

# 단일 소스로 메쉬 만들기

이 비디오는 개발자가 Adobe Developer App Builder용 API Mesh에서 단일 소스로 메쉬를 만드는 방법을 이해하는 데 도움이 됩니다. 이 기본 예제가 작동하려면 공개적으로 액세스할 수 있는 API 또는 GraphQL 끝점이 필요합니다. 이 비디오에서는 Commerce 인스턴스에 사용할 간단한 `mesh.json` 파일을 만드는 방법도 설명합니다. 자세한 내용과 코드 샘플을 보려면 [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}를 방문하세요.

## 이 비디오는 누구의 것입니까?

* API mesh를 처음 사용하는 모든 사용자
* 여러 GraphQL 및 API 소스를 결합하는 데 관심이 있는 개발자
* 네트워크 탭을 필터링하고 GraphQL으로 필터링하는 방법을 알고 있어야 하는 모든 사람

## 비디오 콘텐츠

* API Mesh를 역방향 프록시로 사용
* JSON 구성 파일에서 메쉬 만들기
* 새로 생성된 GraphQL 엔드포인트 액세스

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## JSON 구성 파일 만들기

API Mesh는 JSON 구성 파일을 사용하여 소스 핸들러를 정의합니다. JSON 파일에는 메쉬의 소스가 포함된 `sources` 배열이 포함되어 있습니다. 다음은 단일 소스가 있는 메쉬의 예입니다.

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
