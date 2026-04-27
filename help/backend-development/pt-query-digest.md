---
title: Percona Toolkit pt-query-digest의 작동 방식과 사용 이유를 알아봅니다
description: 느린, 일반 및 이진 로그 파일에서 MySQL 쿼리를 분석합니다. 또한 'SHOW PROCESSLIST'의 쿼리와 tcpdump의 MySQL 프로토콜 데이터를 분석할 수 있습니다.
kt: 13846
doc-type: video
duration: 510
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
TQID: https://experienceleague.adobe.com/lh-fBjlhZO6W-K08KNb-KaG-N2slLZVpNOSg6LAp0n8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 113
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

pt-query-digest와 몇 가지 실제 사례를 사용하여 추론을 심화하는 데 도움이 되는 이유를 알아보십시오.

## 이 비디오는 누구의 것입니까?

* 설계자
* 개발자
* DevOps

## 비디오 콘텐츠

* pt-query-digest 사용에 대해 알아보기
* 이 Percona Toolkit 기능의 장점과 단점에 대해 알아보십시오
* 결과를 이해하고 가능한 성능 단계를 고려해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3452301?captions=kor&learn=on)

## 코드 참조

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 유용한 리소스

* [페르코나 툴킷](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
