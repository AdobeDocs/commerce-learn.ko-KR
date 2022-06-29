---
title: 데이터베이스에 테이블 추가
description: '"[!DNL Commerce] 에는 데이터베이스 테이블을 만들고 기존 테이블을 수정하고 데이터를 추가할 수 있는 특별한 메커니즘이 있습니다."'
topic: Development
kt: 5613
doc-type: video
activity: use
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: acee5ba84ea32e14a743cd269f77ced821548ad6
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# 데이터베이스에 테이블 추가

[!DNL Commerce] 에는 데이터베이스 테이블을 만들고, 기존 테이블을 수정하고, 모듈을 설치할 때 추가해야 하는 설정 데이터와 같은 일부 데이터를 데이터베이스 테이블에 추가할 수 있는 특별한 메커니즘이 있습니다. 이 메커니즘을 사용하면 다양한 설치 간에 이러한 변경 사항을 전송할 수 있습니다.

시스템을 다시 설치할 때 수동 SQL 작업을 반복적으로 수행하는 대신 데이터가 포함된 설치(또는 업그레이드) 스크립트를 만듭니다. 모듈이 설치될 때마다 스크립트가 실행됩니다.

## 이 비디오 누구?

- 개발자

## 비디오 컨텐츠

- 모듈 만들기
- InstallSchema 스크립트 만들기
- InstallData 스크립트 만들기
- 새 모듈을 추가하고 데이터가 있는 테이블이 만들어졌는지 확인합니다
- UpgradeSchema 스크립트 만들기
- UpgradeData 스크립트 만들기
- 업그레이드 스크립트를 실행하고 테이블이 변경되었는지 확인합니다

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## 유용한 리소스

- [마이그레이션 명령](https://devdocs.magento.com/guides/v2.4/extension-dev-guide/declarative-schema/migration-commands.html)
