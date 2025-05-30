---
title: 제품 속성 만들기
description: 하나의 매개 변수와 함께 json을 반환하는 페이지를 만듭니다.
kt: 14131
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
source-git-commit: d6aeac0c4c66bd8117cc9ef1e0186bbb19cf23e9
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 0%

---

# 제품 속성 만들기

제품 특성을 추가하는 작업은 [!DNL Commerce]에서 가장 많이 사용되는 작업 중 하나입니다. 속성은 제품과 관련된 다양한 실질적인 과제를 해결할 수 있는 강력한 방법입니다. 제품에 드롭다운 유형 속성을 추가하는 간단한 프로세스를 사용할 수 있습니다.

이 비디오에서:

- cotton, Leather, Silk, Denim, Fur 및 Wool 과 같은 값을 사용하여 clothing_material 이라는 속성을 추가합니다.
- 이 속성을 제품 보기 페이지에 굵은 텍스트로 표시합니다.
- Default 속성 세트에 지정하고 제한을 추가합니다.
- 새 속성 추가

## 이 비디오는 누구의 것입니까?

- 프로그래밍 방식으로 제품 특성을 만드는 방법을 배워야 하는 상거래를 처음 사용하는 개발자

## 비디오 콘텐츠

>[!VIDEO](https://video.tv.adobe.com/v/3412442?quality=12&learn=on&captions=kor)

## 코드 샘플

먼저 필요한 폴더, xml 및 PHP 파일을 만듭니다.

- app/code/Learning/ClothingMaterial/registration.php
- app/code/Learning/ClothingMaterial/etc/module.xml
- app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
- app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
- app/code/Learning/ClothingMaterial/Setup/InstallData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

>[!NOTE]
>
>모듈에서 선언적 스키마를 사용하는 경우 및 대부분의 모듈에 2.3.0 이후 버전이 있는 경우 setup_version 을 생략해야 합니다. 그러나 일부 레거시 프로젝트가 있는 경우 이 메서드가 사용되는 것을 볼 수 있습니다.  자세한 내용은 [developer.adobe.com](https://developer.adobe.com/commerce/php/development/build/component-name/#add-a-modulexml-file){target="_blank"}을(를) 참조하십시오.\
>참고: 이 예제 코드가 작동하려면 setup_version을 포함해야 합니다. 그렇지 않으면 InstallData.php가 실행되지 않습니다.



```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

>[!NOTE]
>
>프로젝트에 있는 속성 세트 ID를 사용해야 합니다. 이 예에서는 숫자 9입니다.

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/InstallData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'Product Details',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## 유용한 리소스

[사용자 지정 텍스트 필드 특성 추가](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)
