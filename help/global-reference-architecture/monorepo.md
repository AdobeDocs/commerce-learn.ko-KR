---
title: 글로벌 참조 아키텍처 Monorepo
description: 글로벌 참조 아키텍처에 대한 모노레포 접근 방식을 사용하여 확장 가능하고 탄력적인 상거래 경험을 구축하는 방법에 대해 알아봅니다
jira: KT-16728
doc-type: tutorial
duration: 441
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
TQID: https://experienceleague.adobe.com/22ayPTG7ZgpcWr5l53Ide2sZeGTN-9P0jept0ZQ6njQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1418
ht-degree: 0%

---

# Monorepo 글로벌 참조 아키텍처 패턴

{{only-for-on-prem-commerce-cloud}}

이 안내서에서는 Monorepo GRA(글로벌 참조 아키텍처) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

모노레포 GRA 패턴에는 모든 공통 사용자 정의를 호스팅하기 위한 단일 Git 저장소가 포함됩니다. 이 단일 Git 저장소는 Composer를 통해 별도의 Composer 패키지로 표시됩니다.

![코드가 모노레포 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

* 기능 테스트에 적합
* 공유 코드 저장소를 통해 코드 재사용
* 패키지 설치의 완벽한 유연성, 각 GRA 패키지를 개별적으로 업그레이드, 다운그레이드 또는 백포팅할 수 있습니다.
* 시맨틱 버전 관리를 위한 완벽한 지원
* 특별한 도구, 복잡한 인프라 또는 특별한 분기 전략이 필요하지 않음
* Composer가 지원하는 모든 패키지 유형 지원
* 사용 후 삭제되는 환경에 적합하지만, 대량 게재 팀의 경우 매우 유용합니다.

단점:

* 동일한 구성에서 개발되지 않은 패키지 조합을 배포할 수 있으므로 엄격한 테스트 절차가 필요합니다.
* 모노레포 GRA 패턴은 시작 부분에서 복잡할 수 있다. 팀이 시스템과 협력하는 데 도움이 되는 리드 할당

## 개별 패키지 GRA 패턴으로 Adobe Commerce 설정

### 디렉터리 구조

개별 패키지 GRA 패턴이 있는 전체 Adobe Commerce 설치의 최종 디렉터리 구조는 다음 디렉터리 구조를 가집니다.

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

프로덕션 Git 저장소에는 다음과 같은 디렉터리 구조가 있습니다.

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

차이점은 프로덕션 인스턴스가 Composer에서 설치되고, 여기서 모노레포는 패키지 디렉터리 내에 모든 패키지의 자체 복사본을 보유한다는 것입니다.

### 프로덕션 저장소 준비

Brand X의 웹 스토어를 나타내는 첫 번째 Adobe Commerce 인스턴스에 대한 저장소를 만듭니다.

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install`(으)로 Adobe Commerce을 설치합니다. 결과 `app/etc/config.php` 및 작성기 파일을 커밋합니다. Composer는 다른 모든 것을 관리하므로 Git에 다른 것이 없어야 합니다.

### 모노레포 저장소 준비

Monorepo 저장소는 동일한 단계로 시작합니다.

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

`bin/magento setup:install`(으)로 Adobe Commerce 설치, 커밋 및 푸시.

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### 모노레포 개발 준비

모노레포 개발의 경우 composer.json 파일을 다음과 같이 변경합니다.

1. 이 패키지가 Monorepo 패키지임을 명확히 알 수 있도록 패키지의 이름과 설명을 변경합니다.
1. 버전이 이 저장소의 Git 태그를 사용하여 관리되므로 composer.json에서 버전 속성을 삭제합니다.
1. 필수 섹션을 나중에 생성되는 메타 패키지로 바꿉니다.
1. 최소 안정성을 [개발]로 변경합니다.
1. 포함된 각 패키지에 대한 분기 별칭을 포함하여 호스트 Monorepo 패키지에 대해 `packages/*/*`을(를) 가리키는 경로 유형 저장소를 추가합니다.
1. 프로젝트 자체에 대한 분기 별칭 추가

다음 Git diff는 깔끔한 Adobe Commerce 설치와 위에서 언급한 변경 사항의 차이를 보여줍니다.

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### 메타패키지 사용

GitHub의 [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)에서 예제 코드를 다운로드하여 이 예제에 사용된 메타패키지 및 샘플 모듈을 가져옵니다.

작성기 메타패키지는 여러 작성기 패키지를 단일 패키지로 묶습니다. 메타패키지가 필요한 경우 Composer를 통해 번들로 제공되는 모든 패키지는 메타패키지의 필수 섹션에 자동으로 설치됩니다.

이 예에는 두 개의 메타패키지가 있습니다.

1. **antonevers/gra-meta-brand-x**: &quot;Brand X&quot;를 구성하는 모든 항목이 포함된 메타패키지
1. **antonevers/gra-meta-foundation**: 모든 브랜드에 항상 설치된 모든 항목이 포함된 메타패키지입니다.

브랜드 메타패키지에는 기초 메타패키지가 필요합니다. 브랜드 메타패키지가 필요한 경우 기초 메타패키지도 자동으로 필요합니다. 메타패키지의 두 composer.json 파일을 참조하여 관련 방법을 확인하십시오.

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

메타패키지는 함께 속한 코드를 구성하는 좋은 방법입니다. 메타패키지를 사용하여 지역별, 글로벌, 브랜드별 또는 자신에게 적합한 그룹화된 패키지 그룹을 정의합니다. Adobe Commerce의 여러 설치를 유지 관리하는 경우 matappackages는 패키지가 예상되는 컨텍스트를 정의하는 안전하고 다양한 방법을 제공합니다.

메타패키지가 `packages` 디렉터리 내의 모노레포에 있습니다. `vendor`의 디렉터리 구조가 미러링되었습니다.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### 모듈 추가 및 개발

`packages` 디렉터리에 모노포에 있는 모듈이 있습니다. 이렇게 하면 Composer가 경로 유형 저장소를 통해 해당 경로를 찾을 수 있습니다.

GitHub의 [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation)에서 예제 코드를 다운로드하여 이 예제에 사용된 메타패키지 및 샘플 모듈을 가져옵니다.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

필요한 경우 `packages` 디렉터리 내에 여러 네임스페이스를 사용할 수 있습니다.

개발은 패키지 디렉터리에서 수행됩니다. `composer update`을(를) 실행하면 `packages` 디렉터리 내의 패키지에 대한 심볼릭 링크가 `vendor` 디렉터리에 생성됩니다. 이렇게 하면 코드가 Adobe Commerce 설치의 일부가 됩니다.

`bin/magento module:enable --all` 또는 특정 모듈에 대해서만 실행하여 추가된 모듈을 사용하도록 설정하십시오.

이제 3개의 샘플 모듈이 설치되고 작동하는 Adobe Commerce 설치 작업이 수행되어야 합니다. 다음 명령을 실행하여 모듈이 설치되어 있고 작동하는지 확인할 수 있습니다.

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### 자동화된 패키지 생성 달성

자동화된 패키지 생성을 위해서는 여러 가지 옵션이 있습니다. 일부 옵션은 다음과 같습니다.

1. [비공개 패키지 사용자](https://packagist.com/)
1. [Monorepo Builder 단순화](https://github.com/symplify/monorepo-builder)
1. 자체 솔루션 구축

[개인 패키지 목록](https://packagist.com/)은(는) Git monorepo에서 패키지를 인식하도록 자동화하고 Composer를 통해 표시합니다. 이 안내서는 Adobe Commerce과 호환되며, 빠르고, 유지 관리가 용이하며 오류가 발생하기 쉽습니다. 따라서 이 안내서는 Private Packagist 옵션에 중점을 둡니다.

개인 패키지 관리자를 설정하는 방법을 설명하는 것은 이 안내서의 범위를 벗어납니다. [docs](https://packagist.com/docs)을(를) 참조하십시오.

조직 동기화를 설정하고 Git 저장소가 자동으로 개인 패키지 관리자로 동기화되면 패키지를 모노레포로 만들 수 있습니다.

먼저 패키지 탭으로 이동하여 모노레포를 찾습니다.

![패키지 화면에 모노레포 패키지가 표시된 비공개 패키지 목록 화면](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

Monorepo 패키지를 클릭하고 세부 정보 화면에서 &quot;편집&quot;을 클릭하면 다음 페이지로 이동합니다.

![Monorepo 패키지 편집 페이지로 찍은 비공개 패키지 목록](/help/assets/global-reference-architecture/packagist-packages-edit.png)

첫 번째 입력 필드 아래에는 &quot;다중 패키지 저장소 만들기&quot;라는 링크가 있습니다. 이 링크를 클릭합니다.

![다중 패키지 구성으로 비공개 패키지 목록 화면 촬영](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Monorepo 내에서 작성기 패키지를 찾을 수 있는 위치를 정의합니다. 이 예제에서 위치는 `packages/**/composer.json`입니다. Private Packagist가 추출할 패키지를 검색하는 위치를 제한하거나 확장합니다.

The packages tab will show all found packages after saving, and the monorepo itself will no longer be visible as a Composer package:

![Private Packagist screen shot with all monorepo packages visible in the packages screen](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

A version is created in Composer for each package inside the monorepo, for every tag or branch that is created on the monorepo in Git.

## Install the packages to the production environment

Follow the instructions from Private Packagist to add Private Packagist as a composer repository. Private Packagist can and should be used as a mirror for all your Composer repositories and Git repositories, including packagist.org. That way credentials do not have to be shared with developers and you have complete control over each package. This example does not follow this best practice as it would expose the Adobe Commerce codebase publicly.

Download [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) from GitHub to see an example of a production store.

In the production store, there is no `packages` directory, and all packages are installed through Composer. The only package that is required is:

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

Yet all of Adobe Commerce and the entire GRA is installed through this metapackage&#39;s requirements.

## Versioning

All packages in the monorepo receive the same version as the monorepo itself. Think of it as publishing new versions of the entire application. On production, however, you can install a mix of packages from different monorepo versions.

## Ephemeral environments

If you use ephemeral environments or you plan to use them, the monorepo is an excellent choice. Every version and branch of the monorepo contains all Adobe Commerce, third party and custom module files. With a complete installation in every branch, it is possible to run every type of test including functional tests. With other GRA setups such as the separate packages or bulk packages GRA, you would first need to build a working Adobe Commerce environment before being able to run functional tests. From a DevOps perspective, monorepo makes it much simpler.

## 코드 예

The code examples of this article have been combined in a set of Git repositories, which you can use to play around with the proof of concept.

* An example monorepo repository: <https://github.com/AntonEvers/gra-monorepo>
* 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-monorepo-brand-x>
