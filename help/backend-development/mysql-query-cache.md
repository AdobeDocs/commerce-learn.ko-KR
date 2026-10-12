---
title: MySQL 쿼리 캐싱 작동 방식 알아보기
description: MySQL 쿼리 캐싱의 작동 방식, 느린 쿼리 로그에서 "쿼리 캐시 잠금 대기 중"을 찾아 캐싱 문제를 감지하는 방법 및 구성 팁에 대해 알아봅니다.
doc-type: Technical Video
duration: 444
last-substantial-update: 2023-07-27T00:00:00.000Z
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13690
exl-id: 8d3b0ec2-e80c-4457-b924-69e8b8cedf03
TQID: 'https://experienceleague.adobe.com/W91-fJGZtgfpp03ZtYmSh97oNqmCFpF8AvkTYDaOB-g'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
subfeature_v2:
  - id: b673188e-f9fa-492a-b470-c8f949bf7827
    internal-label: Cache
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 0%
---
# MySQL 쿼리 캐싱에 대해 알아보기

MySQL 쿼리 캐시가 무엇이며 작동 방식에 대한 기본적인 이해를 알아봅니다. MySQL 느린 쿼리 로그에서 &quot;쿼리 캐시 잠금 대기 중&quot;을 찾아 MySQL 쿼리 캐싱 문제를 감지하는 방법을 알아봅니다.

## 의도한 대상

* 설계자
* 개발자
* DevOps

## 비디오 콘텐츠

* 쿼리 캐싱에 대해 알아보기
* &quot;쿼리 캐시 잠금 대기 중&quot;을 찾아 쿼리 캐시 설정이 문제인지 감지하는 방법
* SQL을 저장하고 일치하는 쿼리 캐시를 찾는 데 사용하는 방법을 확인하십시오.
* 구성 설정에 대한 몇 가지 팁

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## 유용한 리소스

* [일반 MySQL 지침](https://experienceleague.adobe.com/ko/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql){target="_blank"}
* [Galera 복제 및 느린 쿼리](https://experienceleague.adobe.com/ko/docs/commerce-learn/tutorials/extensibility/backend-development/galera-db-slow-replication){target="_blank"}
