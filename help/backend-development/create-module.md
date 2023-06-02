---
title: 모듈 만들기
description: 하나의 매개 변수와 함께 json을 반환하는 페이지를 만듭니다.
topic: Development
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: fb2cb5b844c4156802e8627e71fc18f8e7fa981d
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# 모듈 만들기

모듈은 의 구조적 요소입니다 [!DNL Commerce] - 전체 시스템이 모듈을 기반으로 구축됩니다. 일반적으로 사용자 지정을 만드는 첫 번째 단계는 모듈을 빌드하는 것입니다.

## 이 비디오는 누구의 것입니까?

- 개발자

## 모듈 추가 단계

- 모듈 폴더를 만듭니다.
- etc/module.xml 파일을 만듭니다.
- registration.php 파일을 만듭니다.
- bin/magento 설정을 실행합니다.
- 스크립트를 업그레이드하여 새 모듈을 설치합니다.
- 모듈이 작동하는지 확인합니다.

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## 유용한 리소스

- [모듈 참조 안내서](https://developer.adobe.com/commerce/php/module-reference/)
