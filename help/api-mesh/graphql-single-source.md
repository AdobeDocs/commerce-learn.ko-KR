---
title: API 메시에서 GraphQL 단일 소스 메시 만들기
description: Adobe Systems 상거래 및  [!DNL Adobe App Builder] 에서 API 메시를 사용 하는 방법 Discover. 소스 하나가 있는 메시 만들기에 대해 알아보십시오.
landing-page-description: Adobe Systems 상거래 및  [!DNL Adobe App Builder] 에서 API 메시를 사용 하는 방법 Discover. 소스 하나가 있는 메시 만들기에 대해 알아보십시오.
short-description: Adobe Systems 상거래 및  [!DNL Adobe App Builder] 에서 API 메시를 사용 하는 방법 Discover. 소스 하나가 있는 메시 만들기에 대해 알아보십시오.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---

# 단일 소스가 있는 메시 만들기

이 비디오는 Adobe Systems 개발자 앱 빌더를 위한 API 메시의 단일 소스가 있는 메시를 생성 하는 방법을 이해 하는 데 도움이 됩니다. 이 기본 예제가 예상 대로 작동 하려면 공개적으로 액세스 가능한 API 또는 GraphQL 끝점이 필요 합니다. 이 비디오에서는 Commerce 인스턴스에 사용할 간단한 `mesh.json` 파일을 만드는 방법에 대해서도 설명 합니다. 자세한 내용 및 코드 샘플은 방문 [ 만들기 망상 ](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1) {target="_blank"} 을 참조 하십시오.

## 다음에 대 한이 비디오는 무엇입니까?

* API 메시에 대 한 새로운 사용자
* 여러 GraphQL 및 API 소스 결합에 관심이 있는 개발자
* 네트워크 탭 필터링 하 고 GraphQL 필터링 하는 방법을 알고 있어야 하는 사람은 누구 입니까?

## 비디오 컨텐츠

* API 메시를 역방향 프록시로 사용
* JSON 구성 파일에서 메시 만들기
* 새로 만든 GraphQL 끝점 액세스

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## Json 구성 파일 만들기

API 메시는 JSON 구성 파일을 사용 하 여 소스 처리기를 정의 합니다. JSON 파일에는 메시의 소스가 들어 있는 배열이 포함 되어 있습니다 `sources` . 다음은 단일 소스가 있는 메시의 예입니다.

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
