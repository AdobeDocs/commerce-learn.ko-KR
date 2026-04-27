---
title: 명령줄을 사용하여 관리자 구성 보기 및 설정
description: 명령줄을 사용하여 관리자 구성을 보고 설정하는 방법에 대해 알아봅니다.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# 명령줄을 사용하여 관리자 구성 보기 및 설정

Commerce CLI를 사용하여 구성 값을 보고, 설정하고, 찾는 방법에 대한 데모입니다. 값이 저장되는 위치와 기본값이 생성되는 위치를 파악합니다.

## 이 비디오는 누구의 것입니까?

* Adobe Commerce 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

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

터미널에서 페이지를 보고 줄 번호 `cat -n vendor/magento/module-email/etc/config.xml`을(를) 표시하려면

## 추가 리소스

* [명령줄 도구](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html?lang=ko){target="_blank"}
* [관리자 보안 구성](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html?lang=ko){target="_blank"}
