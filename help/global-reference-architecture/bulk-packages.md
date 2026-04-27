---
title: 벌크 패키지 전역 참조 아키텍처로 Adobe Commerce 최적화
description: 효율적인 코드 관리 및 버전 제어를 위해 벌크 패키지 글로벌 참조 아키텍처를 사용하여 Adobe Commerce을 설정하는 방법에 대해 알아봅니다.
jira: KT-16726
doc-type: tutorial
duration: 391
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
TQID: https://experienceleague.adobe.com/q4NzQxc7XJDB-TNv2pU7ghDr6bahliY6soUGPu7fhfg
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1188
ht-degree: 0%

---

# 벌크 패키지 글로벌 참조 아키텍처 패턴

{{only-for-on-prem-commerce-cloud}}

이 안내서에서는 벌크 패키지 글로벌 참조 아키텍처(GRA) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

벌크 패키지 GRA 패턴에는 모든 공통 사용자 지정을 호스팅하기 위한 단일 Git 저장소가 포함됩니다. 이 단일 Git 저장소는 여러 Adobe Commerce 모듈이 포함된 단일 작성기 패키지로 Composer를 통해 노출됩니다.

![코드가 대량 패키지의 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

* 공유 코드 저장소를 통해 코드 재사용
* 서로 다른 인스턴스에 서로 다른 이전 버전의 GRA를 설치할 수 있는 유연성, 단계별 릴리스 활성화
* GRA의 여러 주요 버전을 백업 및 유지 관리할 수 있는 유연성
* GRA의 시맨틱 버전 관리 지원
* 단순성, 개발자는 일반적인 단일 스토어 개발 패턴보다 더 많은 기술이 필요하지 않습니다
* 특별한 도구, 복잡한 인프라 또는 특별한 분기 전략이 필요하지 않음
* 릴리스의 패키지 조합은 항상 함께 개발 및 테스트됩니다

단점:

* 포함된 모든 패키지를 포함하여 전체 GRA를 업그레이드할 수만 있습니다.
* Adobe Commerce 모듈, 언어 팩 및 테마 이외의 작성기 패키지에 대한 GRA 벌크 패키지는 지원되지 않으므로 메타패키지, magento2 구성 요소 패키지, 작성기 플러그인 및 패치가 없습니다.

## Git GRA 분할 패턴으로 Adobe Commerce 설정

### 디렉터리 구조

벌크 패키지 GRA는 Composer 저장소를 통해 재사용 가능한 모든 코드를 설치합니다. 단일 인스턴스에 고유한 코드는 해당 인스턴스의 Git 저장소 내에 있습니다. 인스턴스별 코드는 Adobe Commerce의 다른 인스턴스에서 재사용되지 않습니다.

벌크 패키지 GRA 패턴이 있는 전체 Adobe Commerce 설치의 최종 디렉터리 구조는 다음과 같습니다.

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Composer에서 이러한 디렉터리의 코드를 평가하지 않으므로 `app/code`, `app/i18n` 및 `app/design` 디렉터리가 의도적으로 생략되었습니다. 따라서 패키지에 선언된 종속성은 자동으로 설치되지 않습니다. 대량 패키지 GRA 패턴은 `packages/`에 일부 사용자 지정 코드를 설치하고 해당 디렉터리를 작성기 저장소로 처리하여 이 문제를 해결합니다. 작성기 패키지가 `packages/`에서 `vendor/`(으)로 동기화됩니다.

### Git 저장소 준비

공유 GRA 코드와 첫 번째 저장소에 대한 2개의 Git 저장소를 생성합니다. 다음과 같은 파일 구조를 갖는 GRA 저장소로 시작합니다.

그 결과 다음과 같은 디렉토리 구조가 생성됩니다.

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

이 디렉터리 구조는 GraOne 및 GraTwo 모듈의 composer.json 파일과 registration.php 파일만 언급하고 있습니다. 실제로는 이러한 모듈 내에 더 많은 파일이 있습니다.

다음 명령을 실행하여 Git 저장소를 시작합니다.

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

composer.json 파일 컨텐츠:

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

composer.json 패키지의 네임스페이스를 고유한 네임스페이스로 변경합니다.

src/registration.php 파일 내용:

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

registration.php 파일은 Adobe Commerce 모듈 내에서 다른 registration.php 파일을 찾아 실행합니다.

<https://github.com/AntonEvers/gra-bulk-foundation>의 코드를 사용하여 두 개의 샘플 모듈을 만듭니다. 작성기가 샘플 모듈에서 composer.json 파일을 평가하지 않습니다. 그들은 습관처럼 그곳에 있습니다. 모듈을 다른 위치로 이동하기로 결정한 경우 composer.json 파일이 다시 필요하게 됩니다.

### 저장소 저장소 설정

배포 저장소에는 GRA 코드를 포함한 전체 Adobe Commerce 설치가 포함됩니다. 배포 저장소를 만듭니다.

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install`(으)로 Adobe Commerce을 설치합니다. Composer를 사용하여 배포 저장소에 GRA 샘플 모듈을 설치합니다.

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

이 마지막 명령을 실행하면 모듈이 설치되고 작동하고 있음을 입증하기 위해 다음과 같은 출력이 발생합니다.

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

여러 벌크 패키지를 만들어 코드를 구성할 수 있습니다. 예를 들어, Composer를 통해 사용할 수 없는 타사 코드에 대한 타사 벌크 패키지는 입니다. 일반적으로 `app/code`에 설치하는 모든 항목이 이제 대량 패키지의 `src/` 디렉터리에 있어야 합니다. 이 규칙의 예외는 단일 인스턴스에서만 사용되는 코드입니다. 이러한 패키지를 로컬 패키지라고 합니다.

### 로컬 패키지 설치

배포 저장소는 로컬 패키지를 호스팅합니다. GRA 벌크 패키지에 거주하지 않습니다. 로컬 패키지의 위치가 `app/code`이(가) 아니라 `packages/local`입니다. 작성기에 이 디렉터리를 저장소로 처리하도록 지시합니다.

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

<https://github.com/AntonEvers/module-example-local>에서 호스팅되는 예제 모듈을 추가합니다.

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

이 마지막 명령을 실행하면 모듈이 설치되고 작동하고 있음을 입증하기 위해 다음과 같은 출력이 발생합니다.

```bash
Local module is installed successfully and working!
```

로컬 모듈을 브랜드 저장소에 커밋합니다.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## 코드 위치 개요

타사에서 Composer 리포지토리를 통한 설치를 제공하지 않는 경우에만 Foundation 리포지토리의 `src/` 디렉터리 또는 전용 타사 벌크 패키지에 타사 모듈을 저장할 수 있습니다.

* **Adobe Commerce 코어**: repo.magento.com을 통해 사용할 수 있습니다.
* **타사 모듈**: 마켓플레이스 또는 공급업체의 자체 Composer 리포지토리를 통해 사용할 수 있습니다.
* **타사 모듈 대체 옵션**: 대량 패키지의 `src/`에 저장됨.
* **GRA 기초 코드**: 기초 벌크 패키지의 `src/`에 저장됩니다.
* **로컬 코드**: 배포 저장소의 `packages/local` 디렉터리에 저장됩니다.

## GRA 모듈 개발

소스에서 벌크 패키지를 설치하여 벌크 패키지 디렉터리에서 Git을 활성화합니다.

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

The bulk package has been checked out using Git. When you enter the `vendor/antonevers/gra-bulk-foundation` directory, you are also entering the gra-bulk-foundation Git repository. You can create, checkout and merge branches in this directory.

Add Composer dependencies to the composer.json file at the root of the GRA bulk package, which is the only file in the bulk package that Composer evaluates.

## Include third-party modules to the GRA bulk package

Add third-party packages in the require section of the composer.json at the root of the GRA foundation to add them to your GRA. That way, the packages are always installed in all your instances through composer.

## Deliver your code

To deliver code to the main branch, there are 2 paths. First the local modules, which are merged to the main branch. Run Composer update for those modules. Do not allow developers to update composer.lock in their ticket branches to reduce conflicts. Only update the composer.lock file in staging and production branches, which reduces the risk of conflicts.

Secondly, the GRA bulk packages, which are merged into the main branch of the GRA bulk repository. Then you can add a Git tag to the main branch, versioning the Composer package. Require your new version of the GRA bulk package in the composer.json of the deployment repository to install it.

## Branching strategy

This GRA pattern works with all branching strategies so long as you mirror the branching strategy of the deployment repositories in your GRA bulk repository. For releases, create a release branch with the same name in both repositories. For development, create a ticket branch in both repositories.

In ticket branches, you should almost never have to update the composer.lock file. Just check out the right branches in your development environment for both the store and the GRA foundation repository with Git. The exception is when you update requirements in the GRA foundation composer.json file. Upgrading the GRA foundation in the deployment repository is only done when building the release, or when building a QA branch.

## Code examples

이 문서의 코드 예는 개념 증명을 테스트하는 데 사용할 수 있는 Git 저장소 세트로 사용할 수 있습니다.

* 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-bulk-brand-x>
* GRA 코드 리포지토리: <https://github.com/AntonEvers/gra-bulk-foundation>
* 예제 로컬 모듈: <https://github.com/AntonEvers/module-example-local>
