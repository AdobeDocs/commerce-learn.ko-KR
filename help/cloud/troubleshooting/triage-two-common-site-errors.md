---
title: Diagnose and fix a few common Commerce Cloud errors
description: Resolve two common Adobe Cloud project errors that prevent the site from loading.
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
TQID: https://experienceleague.adobe.com/-mN2UoNU6uKjUoHmZT59LgT4o7p4d4O2Zl1BR3x8y-8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 135
ht-degree: 0%

---

# Diagnose and fix service unavailable and an error occurred

Learn how to triage and resolve two common errors seen on Adobe Commerce Cloud projects.  Understand how and why these errors happen and what are the recommended steps to resolve them.

## 이 비디오는 누구의 것입니까

* Developers and IT Professionals
* System Administrators

## 비디오 콘텐츠

* Diagnose and Resolve Storage Issues:
* Manage Maintenance Mode
* Efficient Troubleshooting tips

>[!VIDEO](https://video.tv.adobe.com/v/3447700?captions=kor&learn=on)


## Commands used in the video

Find the last 5 lines of the exception log mentioned in the response message.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

To check hard drive space. Pay attention to the line dev/mapper/xxxx

```SHELL
df -h
```

Lets find the top 15 largest files

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Display the status of maintenance mode

```SHELL
php bin/magento maintenance:status
```

Disable the maintenance mode

```SHELL
php bin/magento maintenance:disable 
```
