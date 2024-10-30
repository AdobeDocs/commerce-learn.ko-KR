---
title: 몇 가지 일반적인 Commerce Cloud 오류 진단 및 해결
description: 사이트를 로드할 수 없는 두 가지 일반적인 Adobe 클라우드 프로젝트 오류를 해결합니다.
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---


# 진단 및 해결 서비스를 사용할 수 없으며 오류가 발생했습니다.

Adobe Commerce Cloud 프로젝트에 표시되는 두 가지 일반적인 오류를 분류하고 해결하는 방법을 알아봅니다.  이러한 오류가 발생하는 방법과 이유 및 이러한 오류를 해결하기 위한 권장 단계는 무엇인지 이해합니다.

## 이 비디오는 누구의 것입니까

- 개발자 및 IT 전문가
- 시스템 관리자

## 비디오 콘텐츠

- 스토리지 문제 진단 및 해결:
- 유지 관리 모드
- 효율적인 문제 해결 팁

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


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
