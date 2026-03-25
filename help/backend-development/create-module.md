---
title: 모듈 만들기
description: Adobe Commerce에서 모듈을 생성 및 등록하고, 설정을 실행하고, 관리 영역, 상점 및 REST API 컨텍스트의 PSR 로거에 로그인하는 플러그인을 추가합니다.
jira: KT-5614
doc-type: Technical Video
duration: 1113
activity: use
last-substantial-update: 2026-03-23T00:00:00Z
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 1e67193c9b80c929ec391acef771562fb930cc67
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 0%

---

# 모듈 만들기

모듈은 [!DNL Commerce]의 구조적 요소이며, 모듈은 시스템의 백본을 형성합니다. 일반적으로 모듈을 빌드하여 사용자 지정을 시작합니다.

## 이 비디오는 누구의 것입니까?

* 백엔드 개발자

## 모듈 추가 단계

1. 모듈 폴더를 만듭니다.
2. `etc/module.xml` 파일을 만듭니다.
3. `registration.php` 파일을 만듭니다.
4. 모듈을 등록 및 설치하려면 `bin/magento setup:upgrade`을(를) 실행하십시오.
5. 모듈이 작동하는지 확인합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3412457?captions=kor&learn=on)

### module.xml 파일

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

### registration.php 파일

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

### 플러그인 추가

그런 다음 기본 모듈에 기능을 추가합니다. Adobe Commerce 개발에서 플러그인을 필수 도구로 사용합니다. 이 비디오 및 튜토리얼에서는 플러그인을 만드는 방법을 보여 줍니다.

>[!VIDEO](https://video.tv.adobe.com/v/3420255?learn=on)

### 플러그인에 대해 기억해야 할 사항

* `di.xml`에서 모든 플러그인을 선언합니다.
* 각 플러그인에 고유한 이름을 지정합니다.
* 선택적으로 `disabled` 및 `sortOrder` 특성을 설정할 수 있습니다.
* `di.xml` 파일이 포함된 폴더를 선택하여 플러그 인 범위를 설정합니다.
* 대상 메서드 호출 전, 후 또는 주변에서 플러그인을 실행합니다.
* `around` 플러그인은 사용하지 마십시오. 유혹을 받지만 잘못된 선택을 나타내고 성능 문제를 일으키는 경우가 많습니다.

### 플러그인 코드 샘플

이 자습서에서는 다음 XML 및 PHP 클래스를 사용하여 첫 번째 모듈에 플러그인을 추가합니다.

### app/code/Training/Sales/etc/adminhtml/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A Plugin that executes when the admin user places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="admin-training-sales-add-logging" type="Training\Sales\Plugin\AdminAddLoggingAfterOrderPlacePlugin" disabled="false" sortOrder="0"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/frontend/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when a customer uses the LoginPost controller from the Luma frontend -->
    <type name="Magento\Customer\Controller\Account\LoginPost">
        <plugin name="training-customer-loginpost-plugin"
                type="Training\Sales\Plugin\CustomerLoginPostPlugin" sortOrder="20"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/webapi_rest/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when the REST API is used OR when the Luma frontend places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="rest-training-sales-add-logging" type="Training\Sales\Plugin\RestAddLoggingAfterOrderPlacePlugin"/>
    </type>
</config>
```

### app/code/Training/Sales/Plugin/AdminAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class AdminAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this admin area order
        $this->logger->notice('An ADMIN User placed an Order - $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

### app/code/Training/Sales/Plugin/CustomerLoginPostPlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Psr\Log\LoggerInterface;
use Magento\Framework\App\RequestInterface;

/**
 * Introduces Context information for ActionInterface of Customer Action
 */
class CustomerLoginPostPlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @var RequestInterface
     */
    private RequestInterface $request;

    /**
     * @param LoggerInterface $logger
     * @param RequestInterface $request
     */
    public function __construct(LoggerInterface $logger, RequestInterface $request)
    {
        $this->logger = $logger;
        $this->request = $request;
    }

    /**
     * Simple example of a before Plugin on a public method in a Controller
     */
    public function beforeExecute()
    {
        $login = $this->request->getParam('login');
        $username = $login['username'];
        $this->logger->notice( "Login Post controller was used for " . $username );
    }
}
```

### app/code/Training/Sales/Plugin/RestAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class RestAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this customer driven order
        $this->logger->notice('A Customer placed an Order for $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

## 유용한 리소스

* [모듈 참조 안내서](https://developer.adobe.com/commerce/php/module-reference/){target="_blank"}
* [플러그인](https://developer.adobe.com/commerce/php/development/components/plugins/){target="_blank"}
