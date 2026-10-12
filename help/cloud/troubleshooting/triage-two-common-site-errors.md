---
title: 몇 가지 일반적인 Commerce Cloud 오류 진단 및 수정
description: 사이트가 로드되지 않도록 하는 두 가지 일반적인 Adobe Cloud 프로젝트 오류를 해결합니다.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 297
last-substantial-update: 2024-10-30T00:00:00.000Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
TQID: 'https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 938d2364-5176-55ec-80f1-9415253e5e51
    internal-label: Site Management
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%
---
# 진단 및 해결 서비스를 사용할 수 없으며 오류가 발생했습니다.

Adobe Commerce Cloud 프로젝트에 표시되는 두 가지 일반적인 오류를 분류하고 해결하는 방법에 대해 알아봅니다.  이러한 오류가 발생하는 방법과 이유 및 이러한 오류를 해결하기 위한 권장 단계는 무엇인지 이해합니다.

## 이 비디오는 누구의 것입니까

* 개발자 및 IT 전문가
* 시스템 관리자

## 비디오 콘텐츠

* 스토리지 문제 진단 및 해결:
* 유지 관리 모드
* 효율적인 문제 해결 팁

>[!VIDEO](https://video.tv.adobe.com/v/3447700?captions=kor&learn=on)


## 비디오에 사용된 명령

응답 메시지에 언급된 예외 로그의 마지막 5줄을 찾습니다.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

하드 드라이브 공간을 확인합니다. dev/mapper/xxxx 행에 유의하십시오.

```SHELL
df -h
```

가장 큰 상위 15개 파일 검색

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

유지 관리 모드 상태 표시

```SHELL
php bin/magento maintenance:status
```

유지 관리 모드 비활성화

```SHELL
php bin/magento maintenance:disable 
```
