---
title: 대량 데이터 마이그레이션 도구 - Source 자격 증명
description: 대량 데이터 마이그레이션 도구를 실행하기 전에 .env 파일에서 소스 인스턴스 URL 및 인증 자격 증명을 구성하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 238
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22095
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 0%
---
# 대량 데이터 마이그레이션 도구에 대한 소스 자격 증명 구성

대량 데이터 마이그레이션 도구를 실행하기 전에 `.env` 파일에서 원본 인스턴스 URL 및 인증 자격 증명을 설정하십시오. 인증 단계는 소스 환경이 온프레미스인지 또는 Adobe Commerce as a Cloud Service(PaaS)인지에 따라 약간 다릅니다.

## 이 비디오는 누구의 것입니까?

* 솔루션 설계자
* DevOps 엔지니어
* 백엔드 개발자

## 비디오 콘텐츠

* `.env` 파일에서 원본 인스턴스 URL과 REST 및 GraphQL URL을 설정합니다.
* Adobe Commerce 관리자의 **시스템** > **확장** > **통합**&#x200B;에서 통합 키를 검색하거나 만듭니다.
* 4개의 필수 토큰을 생성하려면 통합을 활성화합니다.
* 소스가 Adobe Commerce as a Cloud Service(PaaS)인 경우 account.magento.cloud에서 Magento CLI 토큰을 검색합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3496147?captions=kor&learn=on)
