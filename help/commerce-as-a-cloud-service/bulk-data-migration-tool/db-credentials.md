---
title: 대량 데이터 마이그레이션 도구 - DB 자격 증명
description: 마이그레이션 도구를 실행하기 전에 Magento Cloud CLI 또는 프로젝트 ID를 사용하여 .my.cnf 파일에서 소스 데이터베이스 연결을 구성하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
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
source-wordcount: '151'
ht-degree: 0%
---
# 대량 데이터 마이그레이션 도구에 대한 데이터베이스 자격 증명 구성

대량 데이터 마이그레이션 도구를 실행하기 전에 `.my.cnf` 파일에서 원본 데이터베이스 연결을 설정하십시오. 소스 환경이 온프레미스인지 또는 Adobe Commerce as a Cloud Service(PaaS)인지에 따라 단계가 다릅니다.

## 이 비디오는 누구의 것입니까?

* 솔루션 설계자
* DevOps 엔지니어
* 백엔드 개발자

## 비디오 콘텐츠

* `.my.cnf.example`을(를) `.my.cnf`에 복사하고 원본 연결에 대해 이름이 인 새 섹션을 만드십시오.
* 소스가 Adobe Commerce as a Cloud Service(PaaS)인 경우 `.my.cnf`에서 프로젝트 ID를 설정합니다.
* Magento Cloud CLI 터널 명령을 사용하여 호스트, 사용자, 암호, 포트 및 데이터베이스 값을 가져옵니다.
* 소스가 온-프레미스에 있는 경우 도구를 실행하기 전에 호스트 및 포트 연결을 확인하십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3496162?captions=kor&learn=on)
