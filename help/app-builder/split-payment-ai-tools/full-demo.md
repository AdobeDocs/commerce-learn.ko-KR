---
title: 분할 결제 POC - App Builder 전체 데모
description: 이 Luma 데모에서 분할 결제, REST, App Builder I/O 및 연산자 수락/거부가 작동하는 방법과 장바구니를 차단할 수 있는 구성 가능한 사전 주문 합계를 알아봅니다.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 861
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# 분할 결제 POC 만들기: App Builder 전체 데모

Adobe Commerce 및 Adobe App Builder에 구축된 분할 결제 증명 개념에 대한 전체적인 연습입니다. 데모에서는 이미 AI 도구와 프롬프트를 사용하여 처리 중인 Commerce 확장 및 App Builder 앱을 제작했다고 가정합니다. 이 비디오는 해당 코드가 병합되고, 기본 Luma 테마를 사용하여 Adobe Commerce Cloud 웹 사이트에 배포되고, App Builder 프로젝트가 라이브된 후 어떻게 되는지 보여 줍니다.

쇼핑객이 현금 일부와 **[!UICONTROL Store Credit]** 일부를 지불합니다. Commerce은 스토어프론트에 필요한 동기 체크아웃 및 API를 소유하며 App Builder은 오케스트레이션, 운영자 워크플로 및 I/O 이벤트 소비자를 처리합니다. 참조 구현에서는 여전히 많은 판매자에게 일반적인 경로인 Edge Delivery Services 상점 대신 Commerce(PaaS) 프로젝트와 Luma 기본 체크아웃을 사용합니다. 다른 토폴로지에서 **Adobe Commerce as a Cloud service**&#x200B;을(를) 사용하는 경우 App Builder 코드는 비슷하지만 상점 및 진행 중인 작업이 다르게 보입니다. Luma의 클라우드에서 온-프레미스, 자체 호스팅 및 Commerce의 경우 이 비디오는 새로운 기능을 위해 처리 중인 코드와 App Builder 간의 실제 분할을 보여 줍니다.

## 비디오

>[!VIDEO](https://video.tv.adobe.com/v/3484087?learn=on)

## 이 비디오는 누구의 것입니까?

* Commerce 확장성을 계획하고 있는 기술 설계자
* 체크아웃 및 통합을 구현하는 개발자
* PHP와 App Builder 간의 현실적인 분할이 필요한 구현 팀

## 상점 첫 화면에서 시나리오 설정

데모 계정에 **[!UICONTROL Store Credit]**&#x200B;이(가) 있으며, 체크 아웃 시 잔액이 표시됩니다. 주문 합계가 약 $80가 될 때까지 제품을 추가합니다. 이 크기를 사용하면 수학과 싸우지 않고도 성공적인 허용 경로와 카드 감소와 유사한 감소 경로를 모두 표시할 수 있습니다.

**[!UICONTROL Split payment]**&#x200B;은(는) 세션이 스토어 크레딧을 노출해야 하므로 로그인한 고객에게만 제공됩니다. 게스트 체크아웃에 분할 옵션이 표시되지 않습니다.

1. **[!UICONTROL Payment]** 단계에서 현금 방법을 선택합니다. 데모는 **[!UICONTROL Cash on delivery]**&#x200B;의 이름을 **현금**(으)로 바꿉니다.
1. 결제 방법 아래에 분할 결제 영역이 나타납니다. 현금 필드가 주문 총계로 미리 채워지고 구성 요소가 세션에서 **[!UICONTROL Store Credit]**&#x200B;을(를) 읽습니다.
1. ~$80 장바구니의 경우 구성 요소에 $50 + $30 = $80이 표시되도록 **$50** 현금 및 **$30** 스토어 크레딧을 설정하십시오.
1. 금액이 유효하면 주문을 합니다.

UI가 분할에 대한 새 **[!UICONTROL Commerce]** REST API 호출을 트리거합니다. 체크아웃 시 동기 유지: Commerce은 스토어 크레딧을 적용하고 주문에 분할된 라인을 저장하며 App Builder이 나중에 사용하는 I/O 이벤트를 전송합니다. 성공 페이지는 즉시 고객에게 제공됩니다.

## Commerce 관리자의 순서 검토

**[!UICONTROL Sales]** > **[!UICONTROL Orders]**&#x200B;에서 새 주문을 엽니다. **[!UICONTROL Payment information]** 영역에는 분할이 표시됩니다(예: **$50** 현금 및 **$30** 스토어 크레딧). 현금이 아직 수집되지 않은 경우 상태는 **보류 중**&#x200B;입니다. 주문을 만들 때 이미 스토어 크레딧이 적용됩니다.

**[!UICONTROL Comments]**&#x200B;에는 App Builder이 추가한 텍스트가 포함될 수 있습니다. App Builder의 **[!UICONTROL Payment orchestrator action]**&#x200B;이(가) I/O 이벤트를 받고, 분할 금액을 확인하고, 인증된 REST를 사용하여 주석을 추가했습니다. **[!UICONTROL Commerce]**&#x200B;은(는) 이러한 호출을 다른 REST 호출과 비슷하게 취급하므로 작성자를 알 필요가 없습니다.

## App Builder 운영자 대시보드(ERP 또는 백오피스) 사용

간단한 HTML 경험은 **[!UICONTROL App Builder]** 웹 작업으로 실행됩니다(이 보기의 경우 **[!UICONTROL Admin]** 없음). 대시보드에서 **[!UICONTROL Commerce Order]** REST API를 호출하고 **보류 중**&#x200B;으로 필터링하여 $50 현금 구간을 찾을 수 있습니다.

일반적으로 이 패턴을 ERP 또는 사기 도구와 통합합니다. 다음 두 가지 작업이 표시됩니다.

* **수락** — 현금이 수금되었는지 확인합니다(생산 시 종종 현금 인출기나 상점 시스템에서 자동 우편).
* **거부** — 사기 행위나 실패한 수집 단계를 시뮬레이션합니다(프로덕션 환경에서는 일반적으로 위험 서비스에서 자동화).

**주문 수락 및 완료**

1. **[!UICONTROL App Builder]** 앱에서 **승인**&#x200B;을 사용하여 현금을 확인합니다.
1. 앱이 현금 라인을 완료하는 새 **[!UICONTROL Commerce]** 끝점을 호출합니다. 기본 주문 흐름(송장 발행 및 이 빌드에서 자동화된 배송)은 기본 **[!UICONTROL Commerce]** 동작을 사용하여 실행됩니다. **[!UICONTROL Admin]**&#x200B;에 사람이 필요한 경로가 없습니다. App Builder에 있는 오케스트레이션 및 엔드포인트가 라이브됩니다.

**[!UICONTROL Admin]**&#x200B;에서 동일한 주문 새로 고침: 송장과 함께 현금이 수락되면 상태가 **완료**(으)로 이동하며, 제품을 배송할 수 있는 경우 배송이 데모에서 전체 라이프사이클을 단축합니다.

**거부 및 취소**

1. 아직 거절 시나리오에 있는 미리 작성된 주문을 엽니다.
1. **[!UICONTROL App Builder]** 앱에서 **거부**&#x200B;를 사용하여 사기 또는 컬렉션 오류를 시뮬레이션하세요.
1. **[!UICONTROL Admin]**&#x200B;에서 순서는 **취소됨**&#x200B;입니다. 내역은 거부된 현금 상태를 보여 주며, 댓글은 결과를 설명하고, 쇼핑객은 최종 판매인 것처럼 스토어 신용 라인에 비용을 청구하지 않았으며, 스토어 신용 송장은 취소와 함께 무효화됩니다. 생산 시스템은 고객에게 통지하고 재고 또는 서비스에 대한 보류를 릴리즈합니다.

## 주문 총 임계값(주문이 만들어지기 전의 유효성 검사)

App Builder 또는 **[!UICONTROL Commerce]** 구성은 유효성 검사 규칙을 지원할 수 있습니다. 이 빌드는 장바구니 값에서 **$100**&#x200B;을(를) 초과하는 주문을 차단합니다(변경할 수 있는 데모 상한). 값 검사는 가장 일반적인 비즈니스 규칙이 아닙니다. 인벤토리 또는 가용성 검사가 더 일반적이지만, 주문을 만들기 전에 **[!UICONTROL Commerce]**&#x200B;의 **[!UICONTROL Payment]**&#x200B;에서 유효성 검사를 실행할 수 있음을 보여주고, App Builder에서는 여전히 후속 자동화를 처리합니다.

1. 합계가 **$100**&#x200B;을(를) 초과할 때까지 제품을 추가합니다.
1. 분할 **[!UICONTROL UI]**&#x200B;이(가) 여전히 나타납니다. 한도 초과 결과는 **주문**&#x200B;에서만 나타납니다.
1. 구매자에게 **결제를 처리할 수 없는 일반적인 오류가 표시됩니다. 다시 시도하거나 지원팀에 문의하십시오.**
1. **[!UICONTROL cart]**&#x200B;은(는) 변경되지 않으므로 고객이 합계를 낮추거나 다른 방법을 선택하거나 지원 팀에 문의할 수 있습니다.

위험 점수, 지역 규칙 등이 여기에 연결될 수 있습니다. **[!UICONTROL Commerce]**&#x200B;은(는) 순서를 차단하고 **[!UICONTROL App Builder]**&#x200B;은(는) 알림 및 사례 작업을 나중에 소유할 수 있습니다.

## Commerce 온-프레미스, 클라우드의 Commerce 및 **클라우드 서비스로서의 Adobe Commerce**

이 연습에서는 관리하는 인프라의 **[!UICONTROL Adobe Commerce]** 또는 **[!UICONTROL Commerce in the cloud]**(PaaS)를 기존 상점과 일치시킵니다. 이 셰이프에 다른 프런트 엔드가 있고 처리 중인 모듈이 없는 **[!UICONTROL Adobe Commerce as a Cloud service]**(SaaS)의 경우 App Builder 조각은 대부분 동일하지만 상점 및 확장 작업은 다릅니다. 모든 경우에 동일한 원칙이 적용됩니다. **[!UICONTROL Commerce]**&#x200B;이(가) **[!UICONTROL Commerce]** 요청 내에서 발생해야 하는 작업을 수행하고 나머지 경험에는 **[!UICONTROL App Builder]**&#x200B;을(를) 사용합니다.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
