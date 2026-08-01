---
title: 대량 데이터 마이그레이션 도구 - 다단계 마이그레이션
description: 프로덕션 전환 중에 소스가 고정된 상태를 유지해야 할 때 유지 관리 모드를 사용하여 대량 데이터 마이그레이션 도구로 다단계 마이그레이션을 실행하는 방법에 대해 알아봅니다.
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# 대량 데이터 마이그레이션 도구를 사용하여 다단계 마이그레이션 실행

추출 중에 소스 환경을 동결해야 하는 경우 다단계 마이그레이션을 실행합니다. 마이그레이션 중간에 새로운 주문이 나올 수 없는 프로덕션 전환 환경에 적합합니다. 유지 관리 모드를 사용하며 순서대로 실행해야 하는 5가지 단계가 있습니다. 소스가 활성 상태를 유지할 수 있는 경우 이 시리즈의 단상 마이그레이션 비디오를 대신 참조하십시오.

## 이 비디오는 누구의 것입니까?

* 솔루션 설계자
* DevOps 엔지니어
* 백엔드 개발자

## 비디오 콘텐츠

* 시작하기 전에 한 가지 주요 구분: 마이그레이션 도구 자체에 대해 `bin/console` 명령이 실행되고, 원본 Commerce 서버에서 `bin/magento maintenance` 명령이 실행됩니다. 이 도구는 유지 관리 모드를 활성화하거나 비활성화하지 않습니다. 이는 수동 단계입니다.
* 소스가 아직 라이브 상태인 동안 1단계가 실행됩니다. `bin console migration:before-maintenance`이(가) 구성을 확인하고 환경을 초기화하며 CDMS에 연결하고 마이그레이션을 등록하고 기능 테스트를 실행하고 합성 테스트 데이터를 만듭니다. 이 단계가 완료될 때까지 유지 관리 모드를 활성화하지 마십시오.
* 3단계는 동결 환경에서 추출하는 것입니다. `bin/console migration:during-maintenance`은(는) 필요한 경우 PaaS 터널을 다시 열고, 소스에서 추출하고, 스테이징 보기를 정리하고, ACCS 대상으로 로드하고, 확인을 실행하고, 대상의 테스트 데이터를 정리합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3496418?captions=kor&learn=on)
