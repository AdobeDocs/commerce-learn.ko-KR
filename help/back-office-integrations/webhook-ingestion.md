---
title: 수집 Webhook 구성, 배포 및 사용자 정의
description: Commerce과 타사 백오피스 시스템 간의 커뮤니케이션을 용이하게 하기 위해 수집 웹후크를 설정하고 사용자 정의하는 방법에 대해 알아봅니다.
landing-page-description: Commerce Integration Starter Kit를 사용하여 수집 Webhook을 사용하여 Commerce을 서드파티 백오피스 시스템과 통합하는 방법에 대해 알아봅니다.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# 수집 Webhook 구성, 배포 및 사용자 정의

Commerce과 서드파티 백오피스 시스템을 통합하기 위한 수집 웹후크의 설정 및 사용자 정의에 대해 알아봅니다&#x200B;. 이 비디오에서는 타사 시스템의 메시지를 Adobe IO Eventing API로 조정하기 위해 공개적으로 사용 가능한 끝점을 제공하여 Webhook이 시스템 간의 이벤트 통신에서 제한 사항을 해결하는 방법을 설명합니다. 이 프로세스에는 `actions.config.yaml` 파일에서 Webhook를 구성하고 `app.config.yaml` 파일에서 활성화한 다음 배포하여 적절한 기능을 유지하는 작업이 포함됩니다.

이 비디오에서는 타사 이벤트를 통합의 구독 이벤트 유형과 호환되는 형식으로 변환하는 웹후크 코드를 수정하는 단계를 다룹니다. 이 단원에서는 이 번역을 용이하게 하기 위해 `event-mapping.json` 파일을 추가하는 방법에 대해 설명하고 변경 후 런타임 작업을 다시 배포하는 것의 중요성을 강조합니다&#x200B;. 또한 이 비디오에서는 수신 이벤트 페이로드를 확인 및 변환하여 예상 스키마에 맞게 함으로써 성공적인 처리를 보장하고 고객 생성을 위해 Commerce API와 통합됩니다.

## 대상자

* 수집 웹후크를 설정하려는 개발자
* 이벤트 번역을 위해 코드를 사용자 정의하려는 모든 사용자
* 인증 및 페이로드 관리의 중요성을 이해하고자 하는 개발자 및 설계자

## 비디오 콘텐츠

* 구성 및 배포: 이 비디오에서는 `actions.config.yaml` 파일에서 수집 웹후크를 구성하고 `app.config.yaml` 파일에서 이를 활성화하는 것이 중요하다고 강조합니다. 또한 Webhook이 올바르게 작동하도록 변경한 후 프로젝트를 재배포해야 한다는 점도 강조합니다.
* 호환성을 위한 사용자 지정: 타사 이벤트를 통합의 구독한 이벤트 유형에 맞는 형식으로 변환하려면 웹후크 코드를 사용자 지정하는 것이 중요합니다. &#x200B; 이러한 사용자 지정을 통해 시스템 간의 원활한 의사 소통과 성공적인 이벤트 처리를 보장합니다.
* 인증 구현: 기업은 수집 webhook을 사용할 때 권한 없는 요청을 방지하기 위해 요구 사항에 적합한 인증 메커니즘을 구현 할 책임이 있습니다. 이 단계는 통합의 보안 및 무결성을 유지하는 데 필수적입니다.
* 페이로드 유효성 검사 및 변형: 들어오는 이벤트 페이로드를 예상 스키마와 일치하도록 확인 및 변형하는 것은 성공적인 처리 및 Commerce API와의 통합에 중요합니다&#x200B;. 필드를 적절하게 트리밍하고 매핑하면 필요한 데이터로 통합이 효율적으로 작동할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 코드 샘플

* [사용자 지정 수집 Webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [수집 스케줄러 추가](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
