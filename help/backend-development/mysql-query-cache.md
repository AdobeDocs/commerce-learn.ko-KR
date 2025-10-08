---
title: mysql 쿼리 캐싱 방법 알아보기
description: 경우에 따라 잠금을 기다리는 동안 mysql 쿼리가 백업됩니다. 이 튜토리얼에서는 쿼리 캐싱의 정의와 문제가 있는 경우 설정에 대한 몇 가지 권장 사항을 설명합니다.
kt: 13690
doc-type: video
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 8d3b0ec2-e80c-4457-b924-69e8b8cedf03
source-git-commit: 608196b8f68fcd299059907981ec673f2cc60e42
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# mysql 쿼리 캐싱에 대해 알아보기

MySQL 쿼리 캐시가 무엇인지 알아보고 작동 방식에 대한 몇 가지 기본 사항을 이해합니다. mysql 느린 쿼리 로그에서 높은 볼륨에 나타나는 &quot;쿼리 캐시 잠금 대기 중&quot;을 찾아 mysql 쿼리 캐싱 문제를 감지하는 방법을 알아봅니다.

## 이 비디오는 누구의 것입니까?

- 설계자
- 개발자
- DevOps

## 비디오 콘텐츠

- 쿼리 캐싱에 대해 알아보기
- &quot;쿼리 캐시 잠금 대기 중&quot;을 찾아 쿼리 캐시 설정이 문제가 될 수 있는지 검색하는 방법
- SQL을 저장하고 일치하는 쿼리 캐시를 찾는 데 사용하는 방법을 확인하십시오.
- 구성 설정에 대한 몇 가지 팁

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## 유용한 리소스

- [일반 MySQL 지침](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=en){target="_blank"}
- [Galera 복제 및 느린 쿼리](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html){target="_blank"}
