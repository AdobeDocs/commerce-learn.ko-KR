---
title: 로그 자르기
description: 큰 로그 파일을 잘라내어 전체 하드 드라이브로 인해 실패한 배포를 평가하는 방법에 대해 알아봅니다.
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
source-git-commit: b90aa9eb8759391a16dfb29ca25b0d2d271956ed
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# 로그 자르기

전체 하드 드라이브로 인해 실패한 배포 및 등급을 지정하는 방법에 대해 알아봅니다. Adobe Commerce Cloud 환경에서 공간을 확보하기 위해 실행할 수 있는 명령과 검색 방법을 알아봅니다.

이러한 로그 파일이 필요할 수 있는 경우 잘라내기 전에 해당 파일을 `rsync`하거나 다른 방법을 사용하여 서버에서 사용할 수 있는 복사본을 가져올 수 있습니다.

## 이 비디오는 누구의 것입니까

- 개발자 및 IT 전문가
- 시스템 관리자

## 비디오 콘텐츠

- 실패한 배포 진단 및 해결
- 일반적인 대용량 로그 파일이 있는 경우
- 로그 파일을 자르는 빠른 방법

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## 비디오에 사용된 명령

하드 드라이브 공간 `df -h`을(를) 확인합니다. dev/mapper/xxxx 행에 유의하십시오.

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


`ls -lah` 명령을 사용하여 kb, mb, gb와 같이 사람이 읽을 수 있는 형식으로 파일 및 파일 크기를 표시합니다.

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## 로그 자르기 예제

올바른 프로젝트 및 환경에 ssh를 입력한 후 `var/log` 디렉터리로 변경합니다. 그런 다음 `> some-log-file.log`과(와) 유사한 파일을 자를 수 있습니다.

```BASH
> support_report.log 
> cron.log 
```

## 관련 설명서

- [상태 알림](https://experienceleague.adobe.com/ko/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
