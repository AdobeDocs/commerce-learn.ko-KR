---
title: Percona Toolkit pt-query-digest를 사용하여 MySQL 쿼리 분석
description: pt-query-digest를 사용하여 느린 로그, 일반 로그 및 이진 로그 파일에서 MySQL 쿼리, SHOW PROCESSLIST 및 tcpdump의 MySQL 프로토콜 데이터를 분석하는 방법에 대해 알아봅니다.
doc-type: Technical Video
duration: 506
last-substantial-update: 2023-08-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13846
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
TQID: https://experienceleague.adobe.com/lh-fBjlhZO6W-K08KNb-KaG-N2slLZVpNOSg6LAp0n8
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: bd989d82-1e15-4534-88db-f1f51dd77ffa
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 105
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

pt-query-digest를 사용하는 이유와 분석을 개선하는 데 도움이 되는 몇 가지 실용적인 예를 알아보십시오.

## 의도한 대상

* 설계자
* 개발자
* DevOps

## 비디오 콘텐츠

* pt-query-digest 사용에 대해 알아보기
* 이 Percona Toolkit 기능의 장점과 단점에 대해 알아보십시오
* 결과를 이해하고 가능한 성능 단계를 고려해야 합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## 코드 참조

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## 유용한 리소스

* [페르코나 툴킷](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
