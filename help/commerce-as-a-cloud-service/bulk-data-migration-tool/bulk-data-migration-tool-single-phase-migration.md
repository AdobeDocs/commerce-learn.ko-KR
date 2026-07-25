---
title: 대량 데이터 마이그레이션 도구 - 단일 단계 마이그레이션
description: 일괄 데이터 마이그레이션 툴을 사용하여 추출 중에 소스가 활성 상태를 유지할 수 있는 환경에서 단일 단계 마이그레이션을 실행하는 방법에 대해 알아봅니다.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00Z
jira: KT-22139
source-git-commit: 838387ffddbd8bee3ef3ec22694818eb2de5fe2d
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# 일괄 데이터 마이그레이션 도구를 사용하여 단일 단계 마이그레이션 실행

소스 환경이 추출 중에 활성 상태를 유지할 수 있는 경우 단단계 마이그레이션을 실행합니다. 이는 드라이 실행 및 개발 또는 샌드박스 환경에 적합합니다. 마이그레이션 중간에 새 주문이 들어올 수 없는 프로덕션 전환 같은 고정된 소스가 필요한 경우 이 시리즈의 단계별 마이그레이션 비디오를 대신 참조하십시오.

## 이 비디오는 누구의 것입니까?

* 솔루션 설계자
* DevOps 엔지니어
* 백엔드 개발자

## 비디오 콘텐츠

* `bin console build`을(를) 사용하여 도커 이미지를 빌드합니다. 도커 파일이 변경되는 경우에만 다시 실행합니다.
* CDMS CLI 컨테이너 관리자를 시작하려면 `bin console start`을(를) 실행한 다음 컨테이너에서 셸을 한 번 열어 해당 종속성을 다운로드하십시오.
* 전체 10단계 파이프라인을 실행하려면 `bin console migration`을(를) 실행하십시오. 구성 확인, 환경 초기화, PaaS 터널 열기, 통합 테스트 실행, CDMS 등록, 대상 스키마 분석, 테스트 데이터 생성, 소스 데이터 추출, ACCS로 로드, 체크섬 확인, 정리 및 요약.
* 마이그레이션 요약 보고서 확인 — 8단계(데이터 무결성 확인)에서는 파이프라인을 중지하지 않고 오류를 기록하므로 완료된 실행이 깔끔한 확인을 보장하지는 않습니다.
* 이 단일 단계 명령은 완벽한 자체 포함 파이프라인이며, 자체 전용 명령이 있는 유지 관리 모드(단계별 마이그레이션) 워크플로우의 내부 단계로 사용하지 마십시오.

>[!VIDEO](https://video.tv.adobe.com/v/3496321?captions=kor&learn=on)
