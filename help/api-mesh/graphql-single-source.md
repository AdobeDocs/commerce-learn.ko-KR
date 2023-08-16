---
title: API Mesh에서 GraphQL 단일 소스 메쉬 만들기
description: Adobe Commerce 및  [!DNL Adobe App Builder]에서 API Mesh를 사용하는 방법을 살펴봅니다. 소스가 하나인 메쉬를 만드는 방법에 대해 알아봅니다.
landing-page-description: Adobe Commerce 및  [!DNL Adobe App Builder]에서 API Mesh를 사용하는 방법을 살펴봅니다. 소스가 하나인 메쉬를 만드는 방법에 대해 알아봅니다.
short-description: Adobe Commerce 및  [!DNL Adobe App Builder]에서 API Mesh를 사용하는 방법을 살펴봅니다. 소스가 하나인 메쉬를 만드는 방법에 대해 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 12%

---

# 단일 소스로 메쉬 만들기

이 비디오는 개발자가 Adobe Developer App Builder용 API Mesh에서 단일 소스로 메쉬를 만드는 방법을 이해하는 데 도움이 됩니다. 이 기본 예제가 예상대로 작동하려면 공개적으로 액세스할 수 있는 API 또는 GraphQL 끝점이 필요합니다. 이 비디오에서는 간단한 을 만드는 방법도 설명합니다 `mesh.json` 상거래 인스턴스와 함께 사용할 파일입니다. 자세한 내용과 코드 샘플은 다음을 참조하십시오. [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 이 비디오는 누구의 것입니까?

* API mesh를 처음 사용하는 모든 사용자
* 여러 GraphQL 및 API 소스를 결합하는 데 관심이 있는 개발자
* 네트워크 탭을 필터링하고 GraphQL으로 필터링하는 방법을 알고 있어야 하는 모든 사람

## 비디오 콘텐츠

* API Mesh를 역방향 프록시로 사용
* JSON 구성 파일에서 메쉬 만들기
* 새로 생성된 GraphQL 엔드포인트 액세스

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## JSON 구성 파일 만들기

API Mesh는 JSON 구성 파일을 사용하여 소스 핸들러를 정의합니다. JSON 파일에는 `sources` 메쉬의 소스를 포함하는 배열입니다. 다음은 단일 소스가 있는 메쉬의 예입니다.

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
