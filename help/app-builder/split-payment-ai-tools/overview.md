---
title: '분할 결제 POC 만들기: App Builder 및 AI 도구'
description: 목표, 아키텍처 및 이 첫 번째 세션에서 다루는 내용을 포함하여 App Builder 및 Commerce PaaS를 통한 분할 결제 개념 증명에 대해 알아봅니다.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 260
jira: KT-20791
source-git-commit: 9add0b4bfa1eba33ec359adaa766b64711df25ba
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# 분할 결제 POC 만들기: App Builder 및 AI 도구

이것은 분할 결제 개념 증명을 구축하는 데 도움이 되는 AI 지원 개발 사용을 소개하는 튜토리얼 세트의 첫 번째입니다. PaaS(클라우드) 또는 온프레미스에서 Adobe App Builder 및 Adobe Commerce과 함께 작업하며, 이 세션의 개요에서 이어지는 실습형 튜토리얼로 이동합니다. 나머지 시리즈에서 다루는 목표, 높은 수준의 아키텍처 및 로드맵을 기대하십시오.

## 비디오

>[!VIDEO](https://video.tv.adobe.com/v/3483933?learn=on)

## 중요 세부 정보


이 튜토리얼은 오늘날 대부분의 Adobe Commerce 팀이 PHP가 깊은 곳에서 사용하고 있는 위치와 Adobe이 원하는 위치 간의 차이를 메웁니다. Adobe App Builder에서 Commerce 외부에서 실행되는 이벤트 기반, 서버리스 비즈니스 논리입니다. 이것은 장난감 본보기가 아닌 실제 작동 기능을 통해 이루어집니다.

### 실제로 빌드할 내용

고객이 배달 시 현금과 매장 크레딧을 조합하여 지급하는 분할 결제 시스템. 주문이 이루어진 후, 운영자(또는 ERP 시스템)는 Commerce 관리자를 열지 않고도 독립형 대시보드를 통해 현금 결제를 확인하거나 거부합니다. 전체 수락/거절 워크플로우는 App Builder에서 실행됩니다.

#### 건축 교육(핵심 교육)

이 자습서에서는 의도적이고 반복 가능한 결정 프레임워크를 보여 줍니다.

* **PHP에서 유지해야 하는 사항:** Commerce 요청 주기에서 동기적으로 실행되거나 깔끔한 외부 표면이 없는 Commerce 내부 API를 호출하는 모든 항목
* **App Builder으로 이동하는 항목:** 이벤트 처리, 연산자 워크플로, 외부 통합 및 연산자 연결 도구 등 기타 모든 항목

이것은 &quot;모든 것을 App Builder으로 이동&quot;이 아닙니다. 다시 작성하지 않고 전환을 시작해야 하는 팀에게 실용적이고 정직한 시작점입니다.

### PHP 코드가 포함되지 않은 이유

AI 프롬프트 접근 방식은 프롬프트가 샘플 코드만으로는 전달할 수 없는 동작 계약과 아키텍처 추론을 캡처하므로 이 사용 사례에 대해서는 샘플 코드보다 실제로 더 좋습니다. 프롬프트를 실행하고 출력을 읽는 개발자는 코드가 단순히 표시되는 모양이 아니라 코드 모양을 형성하는 이유를 이해합니다.

### 포함된 항목

* 전체 App Builder 애플리케이션 코드(프로젝트 간 일관됨 - 직접 또는 참조로 사용)
* Commerce 모듈, App Builder orchestrator, 운영자 대시보드, 테스트 및 프로덕션 경로를 포함하여 Cursor 및 Claude용으로 설계된 번호가 매겨진 전체 AI 프롬프트 세트
* 실제 ERP가 필요 없이 라이브 Commerce 사이트에 대해 수락/거절 흐름을 테스트하는 시뮬레이션 스크립트
* 모든 의사 결정을 설명하는 아키텍처 설명서

### 이 대상이 누구입니까

첫 번째 실제 Adobe Commerce 통합을 시작하는 Commerce Cloud 온-프레미스 또는 App Builder의 개발자. 완전히 헤드리스 API 우선 배포가 아닌 기존 PHP 투자를 포기하지 않고 App Builder에 실제 비즈니스 논리를 오프로드하는 방법을 보려는 전환기 팀을 위해 특히 기존 상점 전면을 운영하고 있습니다.

### 필수 구성 요소 설명선

Adobe Commerce 2.4.5 이상, App Builder 프로젝트가 있고 I/O 이벤트가 활성화된 Adobe Developer Console 조직에 액세스 체크아웃 UI에 대해 깨끗한 Luma 테마가 가정되어 있습니다. 사용자 정의 테마는 실행하기 전에 프롬프트를 조정해야 합니다.

### 마지막 생각

이는 AI 기반 개발에 대한 진입 차원의 논의로 간주되어야 할 것이다. 이 튜토리얼은 분할 결제에 대한 튜토리얼뿐만 아니라 Commerce 개발 워크플로에서 AI 도구를 사용하기 위한 데모입니다.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
