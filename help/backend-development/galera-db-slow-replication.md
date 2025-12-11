---
title: mysql 느린 쿼리 로그에서 느린 쿼리를 찾는 방법과 Galera DB 복제 디자인 방법이 그 원인일 수 있는 이유에 대해 알아봅니다
description: Galera DB는 데이터를 보조 데이터베이스로 복제하는 데 운영 데이터베이스보다 시간이 더 오래 걸리는 설계 방식을 가지고 있습니다. mysql 느린 쿼리 로그에서 이러한 이벤트를 찾는 방법과 느린 쿼리 로그에 항목이 표시되는 근본 이유 및 향후 이러한 이벤트를 방지하는 방법에 대해 알아봅니다.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Galera DB 복제 및 관련 MySQL 느린 쿼리에 대해 알아보기

Galera 클러스터는 성능과 확장성에 도움이 됩니다. 보조 데이터베이스를 고려할 때 운영 데이터베이스와 다른 데이터 복제 방법을 이해하는 것이 중요합니다. 기본 데이터베이스는 대량 작업을 수행할 수 있습니다. 모든 보조 데이터베이스에 대해 복제가 수행되면 한 번에 하나씩 작업을 수행합니다. 예를 들어, 삭제에 67,000,000개의 항목이 있는 경우 보조 데이터베이스에서 각각 한 번에 하나씩 발생합니다. Mysql 느린 쿼리 로그를 검토하는 경우 이 작업은 시간이 오래 걸릴 수 있습니다. 보조 데이터베이스가 한 번에 하나씩 수행되기 때문에 작업이 동기화되지 않고 성능에 대한 영향을 감지할 수 있습니다.

가능한 경우 대규모 작업을 일괄 처리하여 보조 데이터베이스가 기본 데이터베이스와 계속 동기화되도록 합니다. 일괄 처리를 통해 작업을 적시에 실행하고 성능 영향을 최소화합니다.

## 이 비디오는 누구의 것입니까?

- 설계자
- 개발자
- DevOps

## 비디오 콘텐츠

- 보조 데이터베이스에 대한 Galera 복제
- 흐름 제어에 대해 알아보기
- mysql 느린 쿼리 로그에서 스레드 번호 찾기
- 대량 실행은 기본 실행에서만 발생합니다. 복제는 한 번에 1개씩 수행됩니다.
- 대규모 커밋을 일괄 처리하여 복제가 기본 커밋을 따라갈 수 있도록 지원

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## 유용한 리소스

- [갤러리 클러스터](https://galeracluster.com/)
