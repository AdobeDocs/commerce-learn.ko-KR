---
title: API Mesh에서 GraphQL 단일 소스 메쉬 만들기
description: Adobe Commerce 및 [!DNL Adobe App Builder]. 하나의 소스가 있는 메쉬 생성에 대해 알아봅니다.
landing-page-description: Adobe Commerce 및 [!DNL Adobe App Builder]. 하나의 소스가 있는 메쉬 생성에 대해 알아봅니다.
short-description: Adobe Commerce 및 [!DNL Adobe App Builder]. 하나의 소스가 있는 메쉬 생성에 대해 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 단일 소스로 메쉬 만들기

이 비디오에서는 개발자가 Adobe Developer App Builder용 API Mesh에서 단일 소스와의 메쉬를 만드는 방법을 이해하는 데 도움이 됩니다. 이 기본 예가 예상대로 작동하려면 공개적으로 액세스할 수 있는 API 또는 GraphQL 엔드포인트가 필요합니다. 이 비디오에서는 간단한 `mesh.json` 전자 상거래 인스턴스에 사용할 파일입니다. 자세한 내용 및 코드 샘플은 [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 이 비디오 누구?

* API 메쉬를 처음 사용하는 사람
* 여러 GraphQL 및 API 소스를 결합하려는 개발자
* 네트워크 탭을 필터링하고 GraphQL으로 필터링하는 방법을 알고 있어야 하는 모든 사용자

## 비디오 컨텐츠

* API Mesh를 역방향 프록시로 사용
* JSON 구성 파일에서 메쉬 만들기
* 새로 만든 GraphQL 종단점에 액세스

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## json 구성 파일 만들기

API Mesh는 JSON 구성 파일을 사용하여 소스 핸들러를 정의합니다. JSON 파일에는 다음 항목이 포함되어 있습니다. `sources` 메쉬의 소스가 포함된 배열입니다. 다음은 단일 소스가 있는 메쉬의 예입니다.

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
