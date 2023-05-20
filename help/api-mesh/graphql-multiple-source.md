---
title: API Mesh에서 사용할 여러 소스 GraphQL 만들기
description: Adobe Commerce에서 API Mesh에 대한 여러 소스를 사용하는 방법 알아보기 및 [!DNL Adobe App Builder]. 몇 가지 일반적인 오류와 이를 해결하는 방법에 대해 알아봅니다.
landing-page-description: Adobe Commerce에서 API Mesh를 사용하는 방법 및 [!DNL Adobe App Builder]. 여러 소스가 있는 메쉬를 만들고 몇 가지 일반적인 오류를 해결하는 방법에 대해 알아봅니다.
short-description: Adobe Commerce에서 API Mesh를 사용하는 방법 및 [!DNL Adobe App Builder]. 여러 소스가 있는 메쉬를 만들고 몇 가지 일반적인 오류를 해결하는 방법에 대해 알아봅니다.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# 여러 소스를 사용하여 메쉬 만들기

이 비디오는 개발자가 Adobe Developer App Builder용 API Mesh에서 여러 소스를 사용하여 메쉬를 만드는 방법을 이해하는 데 도움이 됩니다. 이 비디오는 여러 소스를 사용하여 메쉬를 만들고 오류를 식별하는 방법을 보여 줍니다. 자세한 내용과 코드 샘플은 다음을 참조하십시오. [메쉬 만들기](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## 이 비디오는 누구의 것입니까?

* API Mesh를 처음 사용하는 모든 사용자
* 여러 API 및 GraphQL 소스를 결합하는 데 관심이 있는 개발자

## 비디오 콘텐츠

* 사용 방법 [변형](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} 기본 소스 스키마를 수정하려면
* 이름 충돌, 스키마 가용성 및 기타 스키마 구문 문제와 같은 오류를 해결하는 방법
* 수정된 구성으로 메쉬 업데이트

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## JSON 구성 파일 만들기

API Mesh는 JSON 구성 파일을 사용하여 소스 핸들러를 정의합니다. JSON 파일에는 `sources` 메쉬의 소스를 포함하는 배열입니다. 다음은 여러 소스가 있는 메쉬의 예입니다.

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
