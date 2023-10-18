---
title: GraphQL 소개
description: Adobe Commerce 및  [!DNL Magento Open Source]에서 GraphQL을 사용하는 방법에 대해 알아봅니다. Adobe Commerce 및  [!DNL Magento Open Source]에 GraphQL GET 및 POST 호출을 사용합니다.
short-description: Adobe Commerce 및  [!DNL Magento Open Source]에 GraphQL GET 및 POST 호출을 사용하는 방법을 알아봅니다.
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 750c8c9c5c6b3e01b9af8aacae31f3d521c4f7b7
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 6%

---

# GraphQL 소개

GraphQL 및 Adobe Commerce에 대한 시리즈 중 1부입니다. GraphQL은 클라이언트측 애플리케이션이 백엔드와 통신하는 강력한 방법에 대한 업계 표준으로 빠르게 자리매김했습니다. 플랫폼이 Headless 구현 영역에서 기능을 계속 확장함에 따라 Adobe Commerce 개발자에게 점점 더 관련성이 높아지는 주제입니다.

GraphQL을 처음 사용하는 경우 이 섹션에서는 기본 개념과 사용법을 안내합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3424117?learn=on)

## 이 시리즈의 GraphQL 관련 비디오 및 튜토리얼

* [2부 GraphQL - 쿼리](../graphql-rest/graphql-queries.md)
* [3부 GraphQL - 돌연변이](../graphql-rest/graphql-mutations.md)
* [4부 GraphQL - 스키마](../graphql-rest/graphql-schema.md)

## GraphQL란?

GraphQL은 고유한 API 쿼리 언어 및 해당 쿼리 언어에 대한 응답으로 데이터를 제공하는 런타임에 대한 사양입니다.

REST와 같은 기존 웹 API는 서로 다른 시스템에서 데이터를 주고받는 데는 유용했지만 Progressive Web Application과 같은 최신 앱 링크 경험에는 최고 성능보다 낮은 성능을 제공했습니다. 이와 같은 응용 프로그램에서는 _동일_ 애플리케이션은 웹 API를 통해 통신합니다. REST와 같은 체계의 제한된 접근법은 종종 많은 종류의 데이터를 빨리 가져와야 하는 이 맥락에서 적절한 유연성을 제공하지 않는다.

GraphQL을 사용하면 클라이언트가 _정확하게_ 필요한 데이터입니다. 여러 데이터 형식을 가져오기 위해 여러 네트워크 요청을 필요로 하는 대신 하나의 요청으로 여러 형식을 쿼리할 수 있습니다. 또한, 요청된 유형과 필드만 포함하여(쿼리를 직관적으로 미러링하는 형식으로) 응답을 희박하게 유지합니다.

GraphQL 사양을 구현하는 런타임은 모든 언어로 작성할 수 있습니다. Adobe Commerce 및 [!DNL Magento Open Source] 사용
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP 구현과 는 그 위에 자체 레이어를 만듭니다.

[전체 GraphQL 설명서 보기](https://graphql.org/learn){target="_blank"}

## GraphQL 클라이언트 사용

코드 샘플 및 튜토리얼을 테스트하려면 GUI GraphQL 클라이언트가 필요합니다. 다음과 같은 몇 가지 옵션이 있습니다.

* [알테어](https://altairgraphql.dev/){target="_blank"} 는 GraphQL을 위해 특별히 빌드된 완벽하고 완전한 기능을 갖춘 클라이언트입니다. Adobe은 워크스루 비디오에서 알테어를 사용합니다.
* 데스크탑 애플리케이션을 설치하지 않으려는 경우
  [크롬](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} 브라우저.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} 는 GraphQL Foundation에서 GraphQL IDE를 구현한 것입니다. 이것은 설치 가능한 도구가 아니라 직접 인터페이스를 빌드하는 데 사용할 수 있는 패키지입니다.
* 이미 잘 알고 있는 경우 [Postman](https://www.postman.com/){target="_blank"}, 전용 GraphQL 클라이언트만큼 완전히 기능하지는 않지만 GraphQL 쿼리에 대한 적절한 지원을 제공합니다.

GraphQL 클라이언트에서 요청을 URL 경로에 제출해야 합니다 `/graphql` Adobe Commerce 또는 [!DNL Magento Open Source] 인스턴스. 테스트에 기존 인스턴스를 사용하려는 경우 Venia 테마의 데모(PWA Studio 구현의 예)를 사용할 수 있습니다. `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
