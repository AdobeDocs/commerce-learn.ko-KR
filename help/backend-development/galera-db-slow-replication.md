---
title: MySQL 느린 쿼리 로그에서 Galera DB 복제 진단
description: Galera DB의 복제 설계가 보조 데이터베이스 동기화를 늦추는 방법, MySQL 느린 쿼리 로그에서 이러한 이벤트를 식별하는 방법 및 영향을 최소화하는 방법에 대해 알아봅니다.
doc-type: Technical Video
duration: 452
last-substantial-update: 2023-07-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13635
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
TQID: https://experienceleague.adobe.com/NYapiIjnRv5RAS1glm8do16M4jUPmbgfVCs6ICQwbUc
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 262
ht-degree: 0%

---

# Galera DB 복제 및 관련 MySQL 느린 쿼리에 대해 알아보기

Galera 클러스터는 성능과 확장성에 도움이 됩니다. 복제본 데이터베이스를 고려할 때 데이터 복제 방식이 운영 데이터베이스와 다르다는 것을 이해하는 것이 중요합니다. 기본 데이터베이스는 대량 작업을 수행할 수 있습니다. 모든 복제본 데이터베이스에 대해 복제가 수행되면 한 번에 하나씩 작업을 수행합니다. 예를 들어, 삭제에 67,000,000개의 항목이 있는 경우 복제본 데이터베이스에서 한 번에 하나씩 발생합니다. MySQL 느린 쿼리 로그를 검토하는 경우 이 작업은 시간이 오래 걸릴 수 있습니다. 복제본 데이터베이스가 순차적으로 작업을 수행하고 있다는 사실은 동기화되지 않는 이유이며 성능 영향을 감지할 수 있습니다.

복제본 데이터베이스가 기본 데이터베이스와 계속 동기화되도록 하려면 가능한 한 대규모 작업을 일괄 처리합니다. 일괄 처리를 통해 작업을 적시에 실행하고 성능 영향을 최소화합니다.

## 의도한 대상

* 설계자
* 개발자
* DevOps

## 비디오 콘텐츠

* 복제본 데이터베이스에 대한 Galera 복제
* 흐름 제어에 대해 알아보기
* mysql 느린 쿼리 로그에서 스레드 번호 찾기
* 대량 실행은 기본 실행에서만 발생합니다. 복제는 한 번에 1개씩 수행됩니다.
* 복제가 기본 커밋을 유지할 수 있도록 대용량 커밋을 배치합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3423543?captions=kor&learn=on)

## 유용한 리소스

* [갈레라 은하단](https://mariadb.com/products/enterprise/galera-cluster/)
