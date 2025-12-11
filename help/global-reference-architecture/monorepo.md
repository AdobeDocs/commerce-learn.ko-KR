---
title: 글로벌 참조 아키텍처 Monorepo
description: 글로벌 참조 아키텍처에 대한 모노레포 접근 방식을 사용하여 확장 가능하고 탄력적인 상거래 경험을 구축하는 방법에 대해 알아봅니다
jira: KT-16728
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 0%

---

# Monorepo 글로벌 참조 아키텍처 패턴

이 안내서에서는 Monorepo GRA(글로벌 참조 아키텍처) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

모노레포 GRA 패턴에는 모든 공통 사용자 정의를 호스팅하기 위한 단일 Git 저장소가 포함됩니다. 이 단일 Git 저장소는 Composer를 통해 별도의 Composer 패키지로 표시됩니다.

![코드가 모노레포 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

- 기능 테스트에 적합
- 공유 코드 저장소를 통해 코드 재사용
- 패키지 설치의 완벽한 유연성, 각 GRA 패키지를 개별적으로 업그레이드, 다운그레이드 또는 백포팅할 수 있습니다.
- 시맨틱 버전 관리를 위한 완벽한 지원
- 특별한 도구, 복잡한 인프라 또는 특별한 분기 전략이 필요하지 않음
- Composer가 지원하는 모든 패키지 유형 지원
- 사용 후 삭제되는 환경에 적합하지만, 대량 게재 팀의 경우 매우 유용합니다.

단점:

- 동일한 구성에서 개발되지 않은 패키지 조합을 배포할 수 있으므로 엄격한 테스트 절차가 필요합니다.
- 모노레포 GRA 패턴은 시작 부분에서 복잡할 수 있다. 팀이 시스템과 협력하는 데 도움이 되는 리드 할당

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

개발은 패키지 디렉터리에서 수행됩니다. `packages`을(를) 실행하면 `vendor` 디렉터리 내의 패키지에 대한 심볼릭 링크가 `composer update` 디렉터리에 생성됩니다. 이렇게 하면 코드가 Adobe Commerce 설치의 일부가 됩니다.

`bin/magento module:enable --all` 또는 특정 모듈에 대해서만 실행하여 추가된 모듈을 사용하도록 설정하십시오.

이제 3개의 샘플 모듈이 설치되고 작동하는 Adobe Commerce 설치 작업이 수행되어야 합니다. 다음 명령을 실행하여 모듈이 설치되어 있고 작동하는지 확인할 수 있습니다.

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### 자동화된 패키지 생성 달성

자동화된 패키지 생성을 위해서는 여러 가지 옵션이 있습니다. 일부 옵션은 다음과 같습니다.

1. [비공개 패키지 목록](https://packagist.com/)
1. [Monorepo 빌더 단순화](https://github.com/symplify/monorepo-builder)
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

패키지 탭에는 저장 후 발견된 모든 패키지가 표시되며, 모노레포 자체는 더 이상 작성기 패키지로 표시되지 않습니다.

![패키지 화면에 모든 모노레포 패키지가 표시된 비공개 패키지 목록 화면](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

버전은 Composer에서 Git의 모노레포에서 생성된 모든 태그 또는 분기에 대해 모노레포 내의 각 패키지에 대해 생성됩니다.

## 프로덕션 환경에 패키지 설치

개인 패키지 작성자의 지침에 따라 개인 패키지 작성자를 작성기 저장소로 추가합니다. 비공개 패키지 리스트는 packagist.org을 포함한 모든 Composer 저장소 및 Git 저장소에 대한 미러로 사용될 수 있으며 사용해야 합니다. 이러한 방식으로 개발자와 자격 증명을 공유할 필요가 없으며 각 패키지를 완벽하게 제어할 수 있습니다. 이 예는 Adobe Commerce 코드베이스를 공개적으로 노출하므로 이 모범 사례를 따르지 않습니다.

GitHub에서 [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x)을(를) 다운로드하여 프로덕션 스토어의 예를 봅니다.

프로덕션 저장소에 `packages` 디렉터리가 없고 모든 패키지가 작성기를 통해 설치됩니다. 필요한 유일한 패키지는 다음과 같습니다.

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

그러나 모든 Adobe Commerce 및 전체 GRA는 이 메타패키지의 요구 사항을 통해 설치됩니다.

## 버전 관리

모노리포에 있는 모든 패키지는 모노리포와 동일한 버전을 받습니다. 전체 애플리케이션의 새 버전을 게시하는 것으로 간주합니다. 그러나 프로덕션에서는 다른 모노레포 버전의 패키지를 혼합하여 설치할 수 있습니다.

## 덧없는 환경

만약 여러분이 단기적인 환경을 사용하거나 그것을 사용할 계획이라면, 모노레포는 훌륭한 선택입니다. Monorepo의 모든 버전 및 분기에는 모든 Adobe Commerce, 타사 및 사용자 정의 모듈 파일이 포함되어 있습니다. 모든 분기에 전체 설치를 수행하면 기능 테스트를 포함한 모든 유형의 테스트를 실행할 수 있습니다. 개별 패키지 또는 벌크 패키지 GRA와 같은 다른 GRA 설정을 사용하면 기능 테스트를 실행하기 전에 먼저 작업 중인 Adobe Commerce 환경을 빌드해야 합니다. DevOps 관점에서 모노레포는 훨씬 더 단순합니다.

## 코드 예

이 문서의 코드 예는 개념 증명으로 재생하는 데 사용할 수 있는 Git 저장소 집합에 결합되었습니다.

- 예제 Monorepo 리포지토리: <https://github.com/AntonEvers/gra-monorepo>
- 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-monorepo-brand-x>
