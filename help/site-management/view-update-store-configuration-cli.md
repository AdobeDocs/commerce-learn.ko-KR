---
title: 명령줄을 사용하여 관리자 구성을 보고 설정하는 방법에 대해 알아봅니다
description: 명령줄을 사용하여 구성 값을 보고 설정하는 방법에 대한 데모
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
source-git-commit: 9d4b01d383e5492ccc0cbd27636a49581e8ffb5b
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 0%

---


# 명령줄을 사용하여 관리자 구성을 보고 설정하는 방법에 대해 알아봅니다

Commerce CLI를 사용하여 구성 값을 보고, 설정하고, 찾는 방법에 대한 데모입니다. 값이 저장되는 위치와 기본값이 생성되는 위치를 파악합니다.

## 이 비디오는 누구의 것입니까?

- Adobe Commerce 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## 자습서에서 사용되는 일부 명령

암호 보안 설정을 권장됨으로 변경합니다.

`$ php bin/magento config:set admin/security/password_is_forced 0`

판매 주문 자동 복사 기능의 이메일 주소 표시

`$ php bin/magento config:show sales_email/order/copy_to`

관리자에 값이 있는 구성에 대한 빈 결과를 표시합니다.

`php bin/magento config:show trans_email/ident_sales/email`

## 자습서에 사용된 Mysql 쿼리

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## 기본 판매 이메일을 찾을 위치

코드 베이스의 어딘가에 정의된 구성 값을 찾는 방법
`grep -rnw vendor/magento/ -e 'sales@example.com'`

터미널에서 페이지를 보고 줄 번호를 표시하려면 `cat -n vendor/magento/module-email/etc/config.xml`

## 추가 리소스

- [명령줄 도구](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [관리자 보안 구성](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
