---
title: 효과적인 지원 요청 만들기
description: 요청의 효율성을 극대화하기 위해 지원 티켓을 만드는 방법에 대해 알아봅니다.
feature: Best Practices, Customer Service, Support
topic: Commerce
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 126
last-substantial-update: 2024-08-23T00:00:00.000Z
jira: KT-15165
exl-id: cea62272-c7b9-44f7-9c39-5ad3d9122382
TQID: 'https://experienceleague.adobe.com/Si-izuH329pVtctIZ71FLXHfo48YaFtOGXGtAisu-tw'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: cdfd3bc1-dc23-5cf0-b965-d3c0c55cde67
    internal-label: Best Practices
  - id: da76473c-f99b-5ad0-9b14-896aed473f8a
    internal-label: Services
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: deedbb4d-f1b7-58ea-a34a-de1f481f9d4c
    internal-label: Customer Service
  - id: a59f76dc-e003-5617-951e-dffa5bd3de81
    internal-label: Support
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
    internal-label: Insights
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 0%
---
# 효과적인 지원 요청

지원 티켓을 생성할 때는 적절한 채널을 통해 제출하고, 문제에 대한 정확하고 자세한 정보를 제공하고, 올바른 조직 및 연락처 사유를 선택하고, 적절한 제품 및 버전을 선택하고, 잠재적인 솔루션에 대해 제안된 문서를 검토하고, 제출하기 전에 모든 정보를 다시 확인하고, 티켓의 진행 상황을 추적하고 지원 팀과 대화하고, 문제가 해결되면 티켓을 해결된 것으로 표시하고, 추가 지원이 필요한 경우 후속 티켓을 여는 것이 중요합니다. 적절한 채널을 통해 티켓을 제출하고, 정확하고 자세한 정보를 제공하고, 올바른 조직과 연락처 사유를 선택하고, 적절한 제품과 버전을 선택하고, 제안된 문서를 검토하고, 제출하기 전에 모든 정보를 다시 확인하고, 티켓의 진행 상황을 추적하고, 지원 팀과 대화에 참여하고, 문제가 해결되면 티켓을 해결된 것으로 표시하고, 필요한 경우 후속 티켓을 여는 것을 잊지 마십시오&#x200B;. &#x200B;

## 로그 또는 스크린샷 포함

현재 문제에 따라 도움이 되는 로그가 몇 개 있습니다. 가능하면 로그의 섹션을 찾아 지원 요청의 일부로 포함하십시오. 이 로그 스니펫은 엔지니어가 표시되는 오류를 이해하는 데 도움이 되며 분류 프로세스를 신속하게 진행합니다. 모든 앱 서버에는 개별 로그가 있으며 이러한 로그는 집계로서 New Relic에 제공됩니다.  즉, 각 애플리케이션 서버의 개별 로그 파일을 읽는 대신 한 위치에서 모든 오류를 검색할 수 있습니다. 최종 옵션으로 New Relic에 항목이 있는 경우 스크린샷을 추가 정보로 사용하여 프로세스를 신속하게 수행할 수 있지만 NRQL 문이 있는 것이 좋습니다.

## 표준 시간대가 참조되었는지 확인

사람들이 지원 문제에 기여할 때 사건을 참조할 때 자신의 시간대를 명확히 해야 함을 상기시킵니다. 시간대를 참조하면 지원 엔지니어링 팀이 로그 및 이벤트를 조사할 때 실제로 관련이 있는 시간대에 집중할 수 있습니다. 누군가 서버 로그 시간보다 몇 시간 앞서거나 늦어질 수 있습니다.

## 초기의 분류 및 결과 설명

지금까지 수행한 모든 평가 단계를 논의하고 문서화함으로써 지원 엔지니어가 초기 가정을 검증하는 데 도움이 됩니다. 지원 대상 단계 및 결과가 제공되면 전체 프로세스를 신속하게 진행할 수 있습니다. 이는 또한 노력의 중복을 줄이고 결국 검증 수준으로 결과를 문서화하는 방법을 제공하는 데 도움이 될 것입니다.

## New Relic 보고서 링크 또는 NRQL 문 제공

Adobe Commerce의 많은 문제는 New Relic을 통해 추적할 수 있습니다. New Relic 대시보드 또는 사용자 지정 NRQL 문을 보면 일부 문제가 발생하는 위치에 대한 통찰력을 얻을 수 있습니다. 동일한 대시보드 및 사용자 지정 New Relic 쿼리를 공유할 수 있습니다. 지원 티켓에 이러한 링크를 제공하면 엔지니어가 기자가 무엇인지 정확하게 확인할 수 있습니다.

>[!MORELIKETHIS]
> 
> * [Adobe Commerce 도움말 사용 안내서](https://experienceleague.adobe.com/ko/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide){target="_blank"}
