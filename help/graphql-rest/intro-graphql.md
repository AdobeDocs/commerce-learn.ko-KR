---
title: GraphQL 소개
description: Adobe Commerce 및 [!DNL Magento Open Source]. Adobe Commerce 및 POST에 대해 GraphQL GET 및 호출 사용 [!DNL Magento Open Source].
landing-page-description: Adobe Commerce 및 [!DNL Magento Open Source]. Adobe Commerce 및 POST에 대해 GraphQL GET 및 호출 사용 [!DNL Magento Open Source].
short-description: Adobe Commerce 및 [!DNL Magento Open Source]. Adobe Commerce 및 POST에 대해 GraphQL GET 및 호출 사용 [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# GraphQL 소개

GraphQL은 강력한 클라이언트측 애플리케이션이 백엔드에 도달하는 방식을 위한 업계 표준이 되었습니다. 플랫폼이 헤드리스 구현 영역에서 기능을 계속 확장함에 따라 Adobe Commerce 개발자에게 점점 더 적합한 주제입니다.

GraphQL을 처음 사용하는 경우 이 섹션에서는 기본 개념 및 사용법을 안내합니다.

## GraphQL 소개

GraphQL은 고유한 API 쿼리 언어 및 해당 쿼리 언어에 대한 응답으로 데이터를 제공하는 런타임의 사양입니다.

REST와 같은 기존 웹 API는 데이터를 서로 다른 시스템에 대해 잘 제공되었지만, Progressive Web Application과 같은 최신 앱 링크 경험에 대해서는 최대 성능보다 적게 제공했습니다. 이와 같은 응용 프로그램에서는 _동일_ 응용 프로그램은 웹 API를 통해 통신합니다. REST 와 같은 엄격한 구성에 대한 접근 방식은 많은 종류의 데이터를 빠르게 가져올 필요가 있는 상황에서 적절한 유연성을 제공하지 않는 경우가 많습니다.

GraphQL을 사용하면 클라이언트가 명시적으로 설명할 수 있습니다 _정확히_ 필요한 데이터입니다. 여러 데이터 유형을 가져오기 위해 여러 네트워크 요청이 필요한 대신 단일 요청이 여러 유형에 대해 쿼리할 수 있습니다. 또한 요청 받은 유형과 필드만(쿼리를 직관적으로 미러링하는 형식으로) 포함하여 응답 속도가 느려집니다.

GraphQL 사양을 구현하는 런타임은 모든 언어로 빌드할 수 있습니다. Adobe Commerce 및 [!DNL Magento Open Source] 사용
[graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} PHP 구현을 구현하고 그 위에 고유한 레이어를 만듭니다.

[전체 GraphQL 설명서 보기](https://graphql.org/learn){target="_blank"}

## GraphQL 클라이언트 사용

코드 샘플 및 자습서를 테스트하려면 GUI GraphQL 클라이언트가 필요합니다. 다음과 같은 몇 가지 옵션이 있습니다.

* [알테어](https://altairgraphql.dev/){target="_blank"} 는 GraphQL용으로 특별히 제작된 완벽하고 완벽한 기능을 갖춘 클라이언트입니다. Adobe은 Altair를 연습용 비디오로 사용합니다.
* 데스크탑 응용 프로그램을 설치하지 않으려면 Outlook에서 바로 실행되는 Altair 확장도 있습니다
   [Chrome](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox, or [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"} 브라우저.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} 는 GraphQL Foundation에서 GraphQL IDE를 구현하는 것입니다. 이 도구는 설치 가능한 도구가 아니라, 직접 인터페이스를 구축하는 데 사용할 수 있는 패키지입니다.
* 이미 잘 알고 계시다면 [Postman](https://www.postman.com/){target="_blank"}전용 GraphQL 클라이언트로 완전히 활용되지는 않지만 GraphQL 쿼리에 대한 지원을 잘 할 수 있습니다.

GraphQL 클라이언트에서 요청을 URL 경로에 제출해야 합니다 `/graphql` Adobe Commerce 또는 [!DNL Magento Open Source] 인스턴스. 테스트에 기존 인스턴스를 사용하려는 경우 Venia 테마(PWA Studio 구현 예제)의 데모를 사용할 수 있습니다. `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
