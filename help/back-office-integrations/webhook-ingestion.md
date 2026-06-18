---
title: 수집 Webhook 구성, 배포 및 사용자 정의
description: 수집 웹후크를 구성, 배포 및 사용자 정의하여 Adobe Commerce을 서드파티 백 오피스 시스템과 연결하고 이벤트 번역을 처리하는 방법에 대해 알아봅니다.
doc-type: Technical Video
duration: 697
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15870
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 412
ht-degree: 0%

---

# 수집 Webhook 구성, 배포 및 사용자 정의

Commerce과 타사 백오피스 시스템을 통합하기 위한 수집 웹후크의 설정 및 사용자 정의에 대해 알아봅니다. &#x200B; 이 비디오에서는 타사 시스템의 메시지를 Adobe IO 이벤트 API로 조정하기 위해 공개적으로 사용 가능한 끝점을 제공하여 웹후크가 시스템 간 이벤트 통신의 제한을 해결하는 방법에 대해 설명합니다. 이 프로세스에는 `actions.config.yaml` 파일에서 Webhook를 구성하고 `app.config.yaml` 파일에서 활성화한 다음 배포하여 적절한 기능을 유지하는 작업이 포함됩니다.

이 비디오에서는 타사 이벤트를 통합의 구독 이벤트 유형과 호환되는 형식으로 변환하는 웹후크 코드를 수정하는 단계를 다룹니다. 이 비디오에서는 이 번역을 용이하게 하기 위한 `event-mapping.json` 파일 추가에 대해 설명하고 변경 후 런타임 작업을 다시 배포하는 것의 중요성을 강조합니다. 또한&#x200B;, 들어오는 이벤트 페이로드를 확인하고 변환하여 예상 스키마에 맞게 함으로써 성공적인 처리와 고객 생성을 위한 Commerce API와의 통합을 보장합니다.

## 대상자

* 수집 웹후크를 설정하려는 개발자
* 이벤트 번역을 위해 코드를 사용자 정의하려는 모든 사용자
* 인증 및 페이로드 관리의 중요성을 이해하고자 하는 개발자 및 설계자

## 비디오 콘텐츠

* 구성 및 배포: 이 비디오에서는 `actions.config.yaml` 파일에서 수집 웹후크를 구성하고 `app.config.yaml` 파일에서 이를 활성화하는 것이 중요하다고 강조합니다. 또한 Webhook이 올바르게 작동하도록 변경한 후 프로젝트를 재배포해야 한다는 점도 강조합니다.
* 호환성을 위한 사용자 지정: 타사 이벤트를 통합의 구독한 이벤트 유형에 맞는 형식으로 변환하려면 웹후크 코드를 사용자 지정하는 것이 중요합니다. 이러한 사용자 지정&#x200B;을 통해 시스템 간의 원활한 커뮤니케이션과 성공적인 이벤트 처리를 보장합니다.
* 인증 구현: 기업은 수집 webhook을 사용할 때 권한 없는 요청을 방지하기 위해 요구 사항에 적합한 인증 메커니즘을 구현 할 책임이 있습니다. 이 단계는 통합의 보안 및 무결성을 유지하는 데 필수적입니다.
* 페이로드 유효성 검사 및 변형: 들어오는 이벤트 페이로드를 예상 스키마와 일치하도록 확인 및 변형하는 것은 성공적인 처리 및 Commerce API와의 통합에 중요합니다. 필드&#x200B;을 적절하게 트리밍하고 매핑하여 필요한 데이터로 통합을 효율적으로 수행할 수 있습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## 코드 샘플

* [사용자 정의 수집 Webhook](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [수집 스케줄러 추가](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
