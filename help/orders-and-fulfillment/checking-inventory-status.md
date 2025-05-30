---
title: 인벤토리 상태를 통해 개발 및 성능 고려 사항 확인
description: Adobe Commerce에 대한 인벤토리 상태 검사를 수행하기 위한 몇 가지 아이디어 및 고려 사항에 대해 알아봅니다.
feature: Best Practices, Inventory
topic: Development, Performance
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: 84f9139181bf57d967494da97d4d27d297513c31
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---

# 인벤토리 상태를 통해 개발 및 성능 고려 사항 확인

재고에 대한 정확성은 매우 중요한 고려사항입니다. 이월 주문 및 재고 부족 임계값 설정과 같이 이 위험이 가능한 한 낮은지 확인하는 데 도움이 되는 몇 가지 기본 기능이 있습니다. 자세한 내용은 [Experience League](https://experienceleague.adobe.com/ko/docs/commerce-admin/inventory/configuration/backorders)에서 읽을 수 있습니다.

Adobe Commerce 스토어에 대한 실시간 재고 상태 확인이 요청되는 프로젝트 및 사용 사례가 있습니다. 이 튜토리얼에서는 개발 및 성능 고려 사항을 사용하여 이 대화를 처리할 수 있는 통찰력을 제공합니다.

## 이 요청이 반드시 필요한지 확인

가능한 많은 정보를 가지고 대화에 참여할 준비를 하십시오. 가장 중요한 것은 이 프로젝트에서 기본 기능을 사용할 수 없는지 확인하는 것입니다. 이 요청의 이유를 찾아 Adobe Commerce의 기본 기능이 이 요청을 충족하지 못하는지 확인하십시오.

또 다른 고려 사항은 이 기능을 개발, 테스트 및 유지하는 비용입니다. 일부 이해관계자가 필요하다고 생각한다고 하여 그것이 요건인 것은 아니다. Adobe Commerce의 핵심 기능 외에 인벤토리 유효성 검사를 수행하는 데에는 관련 비용이 있습니다. 이러한 비용은 기술적 부담, 더 많은 테스트 및 유효성 검사, 아키텍처에 대한 사용 설명서 및 지원 문서에서 발생합니다.

## 허용 가능한 재고 업데이트 케이던스 결정

재고 조사와 3가지 접근 방식으로 어떻게 수행되는지 고려해 보십시오. 각각 장점과 제한 사항이 있습니다. 또한 복잡성이 증가하며 오류 처리에 대한 더 많은 테스트 및 고려가 필요합니다. 사용자 지정 경로로 이동하기로 결정하는 경우 책임과 고려 사항이 추가된다는 점을 기억하십시오. 몇 가지 예에는 대체 프로세스, 모니터링, 테스트 및 문제 해결이 개발 팀에 해당되는 경우가 포함됩니다. 개발 팀이 전체 기능을 지원할 수 있도록 새로운 지원 설명서, 교육 및 모니터링이 포함된 몇 가지 유용한 항목입니다. 한 가지 추가 부작용은 개발 팀이 프로세스를 완전히 소유하고 더 이상 핵심 Adobe Commerce 애플리케이션에서 제공하는 기본 기능을 활용하지 않는다는 것입니다. Adobe 지원에서 이 수준의 사용자 지정을 지원할 수 없습니다.

첫 번째 접근 방식은 기본 기능을 사용하는 것입니다. 네이티브 기능을 사용하는 것은 최소한의 위험이며 많은 이점이 있습니다. 이 접근 방식을 사용하면 Adobe Commerce에서 해당 기능의 사용을 위해 제공하는 모든 기존 설명서 및 튜토리얼을 사용할 수 있습니다. 재고 관리에 여러 측면이 있으므로 애플리케이션과 함께 제공되는 것을 사용하는 것이 가장 우선적인 고려사항입니다. 그러나 주문 당시 상거래에서 발견된 데이터가 완전히 정확하지 않을 수 있는 사용 사례가 있습니다. 상황이 동기화되지 않는 방법의 예로는 주문 관리 시스템에서 직접 Adobe Commerce 애플리케이션 외부에서 판매가 허용된다는 것입니다. Adobe Commerce에 정확한 인벤토리 수준을 나타내려면 Adobe Commerce 정보를 가능한 한 정확하게 유지하기 위한 일종의 통합이 필요하기 때문입니다. 과다 판매가 허용되지 않는 경우 재고 부족 임계값을 추가하는 것이 0이 되기 전에 품목 판매를 중지하는 좋은 방법입니다. Adobe Commerce의 기본 동기화 기능은 하루에 최대 1회입니다. 일부 사용 사례에는 충분하지만, 다른 사용 사례에는 충분하지 않을 수 있습니다. 자세한 내용은 [예약된 가져오기 및 내보내기](https://experienceleague.adobe.com/ko/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)를 참조하십시오.

두 번째 방법은 `near real-time`입니다. 거의 실시간으로 여전히 기본 기능을 사용합니다. 하지만 여기에는 일정에 따라 인벤토리를 업데이트하기 위해 상거래를 자주 제공하는 통합을 제공하기 위한 몇 가지 추가 작업이 포함됩니다. 예를 들어, 매 시간마다. 이 옵션을 사용하려면 통합이 작동하는 방식에 대한 몇 가지 고려가 필요하지만, &quot;대량 api&quot;를 사용하고 일부 미들웨어를 사용하여 데이터의 변환을 수행하고 이를 상거래에 푸시하도록 하는 것은 훌륭한 접근 방법입니다. Adobe App Builder 또는 유사한 플랫폼을 사용하여 대부분의 작업을 수행하고 보다 빈번한 케이던스로 정보를 Adobe Commerce에 푸시할 수 있습니다.

세 번째 접근 방식이며 가장 많은 위험과 책임을 갖는 가장 복잡한 것은 외부 API 또는 데이터 소스에 대한 실시간 라이브 인벤토리 검사입니다. 외부 시스템에 대한 실시간 인벤토리 검사를 수행하는 것은 위험하며 고려해야 할 몇 가지 다른 요소가 있습니다. 다음은 평가해야 할 몇 가지 다른 사항입니다.

* 외부 시스템이 REST 또는 GraphQL 요청을 수락할 수 있습니까?
* 웹 사이트 트래픽과 일치하지 않는 분당 요청 수 X와 같이 끝점에 대한 제한이 있습니까
* 로드 중 응답 시간에 발생하는 결과
* 응답 시간이 길면 어떻게 됩니까? 자동으로 종료하고 기본 인벤토리와 같은 대체 옵션을 사용합니다.
* API 요청이 허용 한도 내에 있는지 확인하는 데 사용할 수 있는 모니터링 유형

## 비원시 재고 관리를 고려할 때 고려 사항

맞춤화는 가능한 한 복잡하지 않게 유지합니다.
재고 조직이 얼마나 균일한지, 1sku와 총 가용 재고량인지 또는 고려해야 하는 다른 속성이 있는지 여부.

재고 정보가 매우 일정한 경우(예: sku 및 총 가용 수량), 실시간에 가까운 옵션이 확장됩니다. 실시간에 가까운 개념은 소스에서 인벤토리를 수집한 다음 요청에 응답할 수 있도록 스토리지 엔진을 채우는 백그라운드 작업을 의미합니다. 이를 위해 Redis, Mongo 또는 기타 비관계형 데이터베이스와 같은 것을 사용할 수 있습니다. 이러한 옵션은 매우 빠르고 키/값 쌍에 적합합니다. 데이터가 좀 더 복잡한 경우 상거래 애플리케이션 내부 또는 외부에서 관계 데이터베이스를 사용해야 할 수 있습니다. 상거래 데이터베이스에서 이 항목을 오프로드함으로써 핵심 상거래 응용 프로그램을 이러한 거래에서 격리시킵니다. 또 다른 이점은 상거래 애플리케이션, CPU, RAM 등의 I/O를 사용하지 못하도록 하는 것입니다. Adobe Commerce 애플리케이션 서버의 리소스를 사용하는 대신 새 API를 사용하여 오프사이트 스토리지에서 데이터를 가져옵니다.  여기에는 모든 데이터를 변환하는 데 도움이 되는 미들웨어가 필요할 수 있습니다. 그런 다음 호출 응용 프로그램이 예상대로 결과를 얻을 수 있는지 확인합니다. API 메쉬와 함께 App Builder Adobe을 사용하면 데이터를 변환하고 적절한 형식으로 반환할 수 있습니다.

여러 인벤토리 소스가 있는 경우 API 메시와 함께 App Builder Adobe 를 사용하는 것도 좋은 옵션입니다.


## 실행 논리를 프로세스 외부 위치로 이동

Adobe Developer App Builder은 Adobe 솔루션을 확장하기 위해 사용자 지정 경험을 통합하고 만들기 위한 통합 타사 확장성 프레임워크를 제공합니다. Adobe Commerce은 Adobe Developer App Builder을 사용할 수 있습니다. 이는 핵심 애플리케이션에서 일반적으로 발생하는 일부 기능을 확장하고 오프사이트로 이동하는 데 유용한 사용 사례입니다. Commerce 애플리케이션에서 기능을 제거하면 Commerce 애플리케이션의 모듈 수와 복잡성이 줄어듭니다. 따라서 처리 중인 사용자 지정 횟수가 적으면 업그레이드 및 유지 관리의 복잡성이 줄어듭니다.

이를 수행하는 방법에 대한 영감을 얻기 위해 Adobe 팀은 뛰어난 영감 소스일 수 있는 몇 가지 설명서를 만들고 작업 코드 샘플을 제공합니다. 쇼핑객이 장바구니에 제품을 추가하면 서드파티 재고 관리 시스템이 해당 품목의 재고 여부를 확인합니다. 있는 경우 제품을 추가할 수 있도록 허용합니다. 그렇지 않으면 오류 메시지를 표시합니다.  코드 샘플 및 추가 정보는 [Webhook 사용 사례](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart)로 이동하십시오.

## 인벤토리 검사 시기

인벤토리가 아직 사용 가능한지 확인하는 시점은 결국 비즈니스 이해 당사자인 소프트웨어 설계자가 다른 주요 관련자로부터 일부 정보를 제공받아 결정합니다. 몇 가지 적절한 시간에는 장바구니에 품목을 추가할 때와 체크아웃 워크플로우를 시작할 때가 포함됩니다. 다른 모든 이벤트는 필요하지 않을 경우 백엔드 시스템에 로드를 추가하기만 합니다. 가장 중요할 때만 재고 문제를 파악하는 것이 목표라는 점을 명심한다. 다른 검사를 하는 것이 좋을 수 있지만 재고 상태 검사의 전체 목표에 영향을 줄 수 있는 경우에는 비즈니스 이해 당사자나 다른 사람이 백엔드 시스템의 추가 로드에 대한 잠재적 위험을 알고 있는 경우에만 신중하게 고려하여 허용해야 합니다.

## 인벤토리 소스 조사

외부 재고원에 대한 종합적인 조사가 필요하다. 평가해야 하는 항목은 사용 가능한 API 옵션, GraphQL 지원 및 예상 응답 시간입니다. 인벤토리 소스에 제한된 연결 대역폭이 있거나 실시간 요청에서 사용할 의도가 없는 경우 사용 기능이 제외되므로 설계자는 대신 실시간에 가깝게 고려해야 합니다.  API 요청 시간이 정의된 매개 변수를 초과하는 경우 이 역시 실행 가능한 옵션에서 제외됩니다.  예를 들어 API 응답은 한 번에 200MS®이지만 중간 로드 하에서는 500~900MS®까지 가능합니다.  이 문제는 더 많은 로드와 실시간 인벤토리 호출을 사용할 수 없도록 하는 규칙으로 인해 더 악화될 수 있습니다.

라이브 웹 사이트의 예상 트래픽과 유사한 높은 볼륨뿐만 아니라 간단한 요청으로 API 응답 시간을 테스트해야 합니다. 실제 시나리오를 시뮬레이션하려면 상업의 모든 영역을 동시에 테스트해야 합니다. 제품 페이지에서 실시간 인벤토리 호출이 예상될 경우 장바구니를 보고 편집할 때 및 체크아웃 중에 로드 테스트가 이러한 모든 작업을 동시에 시뮬레이션하여 실제 고객 동작과 스토어에 대한 예상치를 모방해야 합니다.

## 대체 옵션

인벤토리 소스가 다운되고 모니터링을 사용할 수 있는 경우 Adobe Commerce의 기본 기능을 사용하는 것이 좋습니다. 그러나 적절한 모니터링을 통해 실시간 인벤토리 검사의 손실을 반영하도록 고객 환경이 동적으로 변경될 수 있습니다. 이는 판매 또는 이벤트가 조기에 취소되거나 초과 판매를 방지하기 위해 디스플레이에서 제거됨을 의미할 수 있습니다. 어떤 이유로든 재고 출처가 다운되었을 때 어떻게 해야 하는지에 대한 기대는 매장 소유자와 함께 고려되고 논의되어야 하며, 그래서 모든 사람은 그러한 불행한 상황에서 접수되는 모든 자동 프로세스를 인지합니다.

## 결론

실시간 재고 조사를 하기로 한 결정은 중요한 것입니다. 웹 사이트 소유자, 개발 팀 및 기타 사용자가 충분한 교육을 받고 모든 이익과 잠재적인 위험을 개발자 리더 또는 설계자에게 인지하도록 합니다. 사유와 대체 프로세스를 다루는 사려 깊은 계획을 제공하는 것은 성공의 핵심입니다.

실시간 인벤토리 검사는 수행할 수 있지만 QA 주기 동안 테스트 및 유효성 검사에 대한 조사 및 고려가 필요합니다. 로드 테스트와 엔드 투 엔드 자동화된 테스트를 확인하는 것은 모든 잠재적 문제를 포착하고 평가하는 데 도움이 됩니다.

모니터링에서 외부 시스템에 대한 실패한 호출의 추세를 감지하거나 응답 시간이 허용 가능한 임계값을 초과하는 경우 수행할 작업을 고려하여 사이트를 온라인 상태로 유지하여 고객에게 가장 적은 양의 자극을 제공할 수 있습니다. 대체 옵션은 외부 인벤토리 검사를 끄고 기본 기능을 사용하는 것만큼 간단하거나, 프로모션을 사용하지 않도록 설정하여 에스컬레이션되는 문제를 개발팀에 알리거나 가능한 경우 요청을 두 번째 또는 세 번째 백엔드 시스템으로 다시 라우팅하는 것만큼 간단할 수 있습니다. 모든 통합에는 특정 시점에 문제가 있으므로 폴백 메커니즘이 어떻게 적용되는지 실제 구현으로 생각해 보십시오. 자동화되었거나 수동 작업이 필요한 모든 사항은 명확하게 문서화해야 합니다.
