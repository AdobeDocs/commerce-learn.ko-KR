---
title: 데이터베이스에 테이블 추가
description: "[!DNL Commerce] 에는 데이터베이스 테이블을 만들고, 기존 테이블을 수정하고, 일부 데이터를 추가할 수 있는 특수 메커니즘이 있습니다."
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# 데이터베이스에 테이블 추가

>[!IMPORTANT]
>
>더 이상 권장되지 않습니다. https://developer.adobe.com/commerce/php/development/components/declarative-schema/을 참조하십시오.


[!DNL Commerce] 에는 데이터베이스 테이블을 만들고, 기존 테이블을 수정하고, 거기에 모듈 설치 시 추가해야 하는 설치 데이터와 같은 일부 데이터를 추가할 수 있는 특수 메커니즘이 있습니다. 이 메커니즘을 사용하면 이러한 변경 사항을 다른 설치 간에 전송할 수 있습니다.

개발자는 시스템을 다시 설치할 때 수동 SQL 작업을 반복적으로 수행하지 않고 데이터가 포함된 설치(또는 업그레이드) 스크립트를 만듭니다. 이 스크립트는 모듈이 설치될 때마다 실행됩니다.

## 이 비디오는 누구의 것입니까?

- 개발자

## 비디오 콘텐츠

- 모듈 만들기
- InstallSchema 스크립트 만들기
- InstallData 스크립트 만들기
- 새 모듈을 추가하고 데이터가 있는 테이블이 생성되었는지 확인합니다
- UpgradeSchema 스크립트 만들기
- UpgradeData 스크립트 만들기
- 업그레이드 스크립트를 실행하고 테이블이 변경되었는지 확인합니다.

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 유용한 리소스

- [설치/업그레이드 스크립트를 선언 스키마로 마이그레이션](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
