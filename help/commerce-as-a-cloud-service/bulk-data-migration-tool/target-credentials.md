---
title: 대량 데이터 마이그레이션 도구 - Target 자격 증명
description: 대량 데이터 마이그레이션 도구를 실행하기 전에 .env 파일에서 대상 인스턴스 URL, Adobe IMS 자격 증명 및 CDMS 설정을 구성하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# 대량 데이터 마이그레이션 도구에 대한 대상 자격 증명 구성

대량 데이터 마이그레이션 도구를 실행하기 전에 `.env` 파일에서 대상 인스턴스 URL, Adobe IMS 자격 증명 및 CDMS 설정을 지정하십시오. Adobe IMS URL, 대상 URL 및 CDMS 호스트가 모두 동일한 환경 계층(단계 또는 프로덕션)과 일치하는지 확인합니다.

## 이 비디오는 누구의 것입니까?

* 솔루션 설계자
* DevOps 엔지니어
* 백엔드 개발자

## 비디오 콘텐츠

* experience.adobe.com에 있는 인스턴스 정보 패널의 값을 사용하여 `.env` 파일의 대상 인스턴스 REST, GraphQL URL 및 대상 테넌트 ID를 설정합니다.
* 환경 계층(단계 또는 프로덕션) 및 지역과 일치하도록 Adobe IMS URL을 설정합니다.
* Adobe Developer Console의 **프로젝트** > **OAuth 서버 간**&#x200B;에서 Adobe IMS 클라이언트 ID 및 클라이언트 암호를 검색합니다.
* 대상 조직 ID를 복사하고 사용자 환경에 맞게 CDMS 호스트, 포트 및 로컬 서버 설정을 구성합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)
