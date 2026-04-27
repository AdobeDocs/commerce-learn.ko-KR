---
title: 인벤토리 상태를 통해 개발 및 성능 고려 사항 확인
description: Adobe Commerce에 대한 인벤토리 상태 검사를 수행하기 위한 몇 가지 아이디어 및 고려 사항에 대해 알아봅니다.
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 498
last-substantial-update: 2024-05-09T00:00:00.000Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: bd989d82-1e15-4534-88db-f1f51dd77ffaid: c1256247-af4b-46d8-9dca-0c654ecfa157id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
subfeature_v2: id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1982
ht-degree: 0%

---

# 인벤토리 상태를 통해 개발 및 성능 고려 사항 확인

재고에 대한 정확성은 매우 중요한 고려사항입니다. 이월 주문 및 재고 부족 임계값 설정과 같이 이 위험이 가능한 한 낮은지 확인하는 데 도움이 되는 몇 가지 기본 기능이 있습니다. 자세한 내용은 [Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders)에서 읽을 수 있습니다.

Adobe Commerce 스토어에 대한 실시간 재고 상태 확인이 요청되는 프로젝트 및 사용 사례가 있습니다. 이 튜토리얼에서는 개발 및 성능 고려 사항을 사용하여 이 대화를 처리하는 insight을 제공합니다.

## 이 요청이 반드시 필요한지 확인

가능한 많은 정보를 가지고 대화에 참여할 준비를 하십시오. 가장 중요한 것은 이 프로젝트에서 기본 기능을 사용할 수 없는지 확인하는 것입니다. 이 요청의 이유를 찾아 Adobe Commerce의 기본 기능이 이 요청을 충족하지 못하는지 확인하십시오.

또 다른 고려 사항은 이 기능을 개발, 테스트 및 유지하는 비용입니다. 일부 이해관계자가 필요하다고 생각한다고 하여 그것이 요건인 것은 아니다. Adobe Commerce의 핵심 기능 외에 인벤토리 유효성 검사를 수행하는 데에는 관련 비용이 있습니다. 이러한 비용은 기술적 부담, 더 많은 테스트 및 유효성 검사, 아키텍처에 대한 사용 설명서 및 지원 문서에서 발생합니다.

## 허용 가능한 재고 업데이트 케이던스 결정

재고 조사와 3가지 접근 방식으로 어떻게 수행되는지 고려해 보십시오. 각각 장점과 제한 사항이 있습니다. 또한 복잡성이 증가하며 오류 처리에 대한 더 많은 테스트 및 고려가 필요합니다. 사용자 지정 경로로 이동하기로 결정하는 경우 책임과 고려 사항이 추가된다는 점을 기억하십시오. 몇 가지 예에는 대체 프로세스, 모니터링, 테스트 및 문제 해결이 개발 팀에 해당되는 경우가 포함됩니다. 개발 팀이 전체 기능을 지원할 수 있도록 새로운 지원 설명서, 교육 및 모니터링이 포함된 몇 가지 유용한 항목입니다. 한 가지 추가 부작용은 개발 팀이 프로세스를 완전히 소유하고 더 이상 핵심 Adobe Commerce 애플리케이션에서 제공하는 기본 기능을 활용하지 않는다는 것입니다. Adobe 지원에서는 이 수준의 맞춤화를 지원할 수 없습니다.

첫 번째 접근 방식은 기본 기능을 사용하는 것입니다. 네이티브 기능을 사용하는 것은 최소한의 위험이며 많은 이점이 있습니다. 이 접근 방식을 사용하면 Adobe Commerce에서 해당 기능의 사용을 위해 제공하는 모든 기존 설명서 및 튜토리얼을 사용할 수 있습니다. 재고 관리에 여러 측면이 있으므로 애플리케이션과 함께 제공되는 것을 사용하는 것이 가장 우선적인 고려사항입니다. 그러나 주문 당시 상거래에서 발견된 데이터가 완전히 정확하지 않을 수 있는 사용 사례가 있습니다. 상황이 동기화되지 않는 방법의 예로는 주문 관리 시스템에서 직접 Adobe Commerce 애플리케이션 외부에서 판매가 허용된다는 것입니다. Adobe Commerce에 정확한 인벤토리 수준을 나타내려면 Adobe Commerce 정보를 가능한 한 정확하게 유지하기 위한 일종의 통합이 필요하기 때문입니다. 과다 판매가 허용되지 않는 경우 재고 부족 임계값을 추가하는 것이 0이 되기 전에 품목 판매를 중지하는 좋은 방법입니다. Adobe Commerce의 기본 동기화 기능은 하루에 최대 1회입니다. 일부 사용 사례에는 충분하지만, 다른 사용 사례에는 충분하지 않을 수 있습니다. 자세한 내용은 [예약된 가져오기 및 내보내기](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export)를 참조하십시오.

두 번째 방법은 `near real-time`입니다. 거의 실시간으로 여전히 기본 기능을 사용합니다. 하지만 여기에는 일정에 따라 인벤토리를 업데이트하기 위해 상거래를 자주 제공하는 통합을 제공하기 위한 몇 가지 추가 작업이 포함됩니다. 예를 들어, 매 시간마다. 이 옵션을 사용하려면 통합이 작동하는 방식에 대한 몇 가지 고려가 필요하지만, &quot;대량 api&quot;를 사용하고 일부 미들웨어를 사용하여 데이터의 변환을 수행하고 이를 상거래에 푸시하도록 하는 것은 훌륭한 접근 방법입니다. Adobe App Builder 또는 이와 유사한 플랫폼을 사용하여 대부분의 작업을 수행하고 보다 빈번한 케이던스로 정보를 Adobe Commerce에 푸시할 수 있습니다.

세 번째 접근 방식이며 가장 많은 위험과 책임을 갖는 가장 복잡한 것은 외부 API 또는 데이터 소스에 대한 실시간 라이브 인벤토리 검사입니다. 외부 시스템에 대한 실시간 인벤토리 검사를 수행하는 것은 위험하며 고려해야 할 몇 가지 다른 요소가 있습니다. 다음은 평가해야 할 몇 가지 다른 사항입니다.

* 외부 시스템이 REST 또는 GraphQL 요청을 수락할 수 있습니까?
* 웹 사이트 트래픽과 일치하지 않는 분당 요청 수 X와 같이 끝점에 대한 제한이 있습니까
* What happens to the response time under load
* What happens when the response times are long, do you terminate this automatically and use a fallback option such as the native inventory.
* What type of monitoring is available to ensure that API requests are within the tolerance limits

## Considerations when considering non-native inventory management

Keep the customizations as non-complex as possible.
How flat can the organization of the inventory be, is it just 1 sku and the total amount of available stock OR are there other attributes that need to be considered.

If the inventory information is fairly flat, for example a sku and the total available quantity, the options for near-real-time are expanded. The concept of near real-time means that there is a background operation that gathers the inventory from the source, and then populates a storage engine to be used to respond to the request. For this you can use things such as Redis, Mongo, or other non-relational databases. These options are nice because they are very fast and work great for key/value pairs. If the data is a bit more complex, then using a relation database, either inside or outside the commerce application, is likely to be required. By offloading this from the commerce database, you keep the core commerce application isolated from these transactions. Another set of benefits are saving the I/O from the commerce application, CPU, RAM and others from use. Instead of using the resources from the Adobe Commerce application servers take advantage of the new APIs to pull the data from the off site storage.  This will likely need a middleware to help transform any data. Then ensure that the calling application can get the result as expected. By using Adobe App Builder with API mesh the data can be transformed and returned properly formatted.

Using Adobe App Builder with API mesh is also a great option when there are multiple sources of inventory.


## Moving the execution logic to an out-of-process location

Adobe Developer App Builder provides a unified third-party extensibility framework for integrating and creating custom experiences to extend Adobe solutions. Adobe Commerce can use Adobe Developer App Builder. This would be an excellent use case for extending some functionality that normally occurs in the core application and moves it off-site. By removing functionality from the Commerce application this reduces the number of modules and complexity to the Commerce application. In turn, lower numbers of in-process customizations reduce the complexity for upgrading and maintenance.

For inspiration for how this might be accomplished, the team at Adobe has created some documentation that can be a great source of inspiration and provide working code samples. 쇼핑객이 장바구니에 제품을 추가하면 서드파티 재고 관리 시스템이 해당 품목의 재고 여부를 확인합니다. 있는 경우 제품을 추가할 수 있도록 허용합니다. 그렇지 않으면 오류 메시지를 표시합니다.  코드 샘플 및 추가 정보는 [Webhook 사용 사례](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart)로 이동하십시오.

## 인벤토리 검사 시기

인벤토리가 아직 사용 가능한지 확인하는 시점은 결국 비즈니스 이해 당사자인 소프트웨어 설계자가 다른 주요 관련자로부터 일부 정보를 제공받아 결정합니다. 몇 가지 적절한 시간에는 장바구니에 품목을 추가할 때와 체크아웃 워크플로우를 시작할 때가 포함됩니다. 다른 모든 이벤트는 필요하지 않을 경우 백엔드 시스템에 로드를 추가하기만 합니다. 가장 중요할 때만 재고 문제를 파악하는 것이 목표라는 점을 명심한다. 다른 검사를 하는 것이 좋을 수 있지만 재고 상태 검사의 전체 목표에 영향을 줄 수 있는 경우에는 비즈니스 이해 당사자나 다른 사람이 백엔드 시스템의 추가 로드에 대한 잠재적 위험을 알고 있는 경우에만 신중하게 고려하여 허용해야 합니다.

## 인벤토리 소스 조사

외부 재고원에 대한 종합적인 조사가 필요하다. 평가해야 하는 항목은 사용 가능한 API 옵션, GraphQL 지원 및 예상 응답 시간입니다. 인벤토리 소스에 제한된 연결 대역폭이 있거나 실시간 요청에서 사용할 의도가 없는 경우 사용 기능이 제외되므로 설계자는 대신 실시간에 가깝게 고려해야 합니다.  API 요청 시간이 정의된 매개 변수를 초과하는 경우 이 역시 실행 가능한 옵션에서 제외됩니다.  예를 들어 API 응답은 한 번에 200MS®이지만 중간 로드 하에서는 500~900MS®까지 가능합니다.  이 문제는 더 많은 로드와 실시간 인벤토리 호출을 사용할 수 없도록 하는 규칙으로 인해 더 악화될 수 있습니다.

라이브 웹 사이트의 예상 트래픽과 유사한 높은 볼륨뿐만 아니라 간단한 요청으로 API 응답 시간을 테스트해야 합니다. 실제 시나리오를 시뮬레이션하려면 상업의 모든 영역을 동시에 테스트해야 합니다. 제품 페이지에서 실시간 인벤토리 호출이 예상될 경우 장바구니를 보고 편집할 때 및 체크아웃 중에 로드 테스트가 이러한 모든 작업을 동시에 시뮬레이션하여 실제 고객 동작과 스토어에 대한 예상치를 모방해야 합니다.

## 대체 옵션

인벤토리 소스가 다운되고 모니터링을 사용할 수 있는 경우 Adobe Commerce의 기본 기능을 사용하는 것이 좋습니다. 그러나 적절한 모니터링을 통해 실시간 인벤토리 검사의 손실을 반영하도록 고객 환경이 동적으로 변경될 수 있습니다. 이는 판매 또는 이벤트가 조기에 취소되거나 초과 판매를 방지하기 위해 디스플레이에서 제거됨을 의미할 수 있습니다. 어떤 이유로든 재고 출처가 다운되었을 때 어떻게 해야 하는지에 대한 기대는 매장 소유자와 함께 고려되고 논의되어야 하며, 그래서 모든 사람은 그러한 불행한 상황에서 접수되는 모든 자동 프로세스를 인지합니다.

## Conclusion

The decision to do real-time inventory checks is a significant one. Ensuring that the website owner, development team, and others are fully educated and aware of all gains and potential pitfalls rest on the developer lead or architect. By providing a thoughtful plan that covers reasons and a fallback process is key to success.

Live inventory checks can be accomplished but requires research and thought around testing and validation during the QA cycle. Ensuring that load testing and end to end automated tests help ensure that all potential issues are caught and triaged.

By considering what actions to perform if monitoring detects a trend of failed calls to the external system or if response times are above acceptable thresholds allow the site to remain online and offer the least amount of irritation to the customers. Fallback options can be as simple as turning off the external inventory checks and using the native functionality or can be as advanced as disabling a promotion, notifying the dev team of the escalating issues or even perhaps re-routing requests to a second or third backend system if available. How the fallback mechanism is exerted should be just a thought out as the actual implementation because every integration has issues at some point. Anything that is automated or needs manual action should be clearly documented.
