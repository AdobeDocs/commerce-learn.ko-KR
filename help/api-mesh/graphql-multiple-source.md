---
title: API Mesh에서 사용할 여러 소스 GraphQL 만들기
description: Adobe Commerce 및 [!DNL Adobe App Builder]. 몇 가지 일반적인 오류와 이를 해결하는 방법에 대해 알아봅니다.
landing-page-description: Adobe Commerce 및 [!DNL Adobe App Builder]. 여러 소스를 포함하는 요청을 만들고 몇 가지 일반적인 오류를 해결하는 방법을 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# 여러 소스 GraphQL API 메쉬 만들기

이 비디오에서는 개발자가 여러 소스를 사용하여 GraphQL 역방향 프록시를 만드는 방법을 이해하는 데 도움이 됩니다. 이 비디오에서는 서로 다른 소스를 결합하고 오류를 식별하고 변경 내용을 git에 저장하는 방법을 보여줍니다. 비디오에 사용된 기본 코드 샘플은 [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## 이 비디오 누구?

* API 메쉬를 처음 사용하는 사람
* 여러 graphql 소스를 사용하려는 개발자
* Graphql을 통해 네트워크 탭을 필터링하고 필터링하는 방법을 알고 있어야 하는 모든 사용자

## 비디오 컨텐츠

* 두 번째 소스의 복잡한 사용자 지정 속성 API 스키마가 기본 소스 스키마를 무시할 수 있는 방법
* 두 번째 재정의 스키마를 설명하기 위해 api 메쉬 구성 수정
* 이름 지정 충돌, 스키마 가용성 및 기타 SDL 구문과 같이 프로세스에서 발생할 수 있는 오류를 해결하는 방법
* 스키마 결합을 시도한 후 일반적인 오류의 예
* 편집 후 api 메쉬 다시 구축
* API Mesh 구성을 수정한 후 변경 내용을 git에 저장

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## json 구성 파일 만들기

Adobe App Builder에서 모든 소스에 대해 알아보려면 JSON 구성에서 이를 정의합니다. 각 소스는 배열의 요소이며 하나 이상을 가질 수 있습니다. 다음은 하나의 응답을 구성하기 위해 함께 매핑되는 여러 소스 요청의 예입니다.

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
        "name": "ERP",
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
