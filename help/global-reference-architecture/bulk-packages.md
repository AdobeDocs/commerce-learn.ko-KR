---
title: 벌크 패키지 전역 참조 아키텍처로 Adobe Commerce 최적화
description: 효율적인 코드 관리 및 버전 제어를 위해 벌크 패키지 글로벌 참조 아키텍처를 사용하여 Adobe Commerce을 설정하는 방법에 대해 알아봅니다.
jira: KT-16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# 벌크 패키지 글로벌 참조 아키텍처 패턴

{{only-for-on-prem-commerce-cloud}}

이 안내서에서는 벌크 패키지 글로벌 참조 아키텍처(GRA) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

벌크 패키지 GRA 패턴에는 모든 공통 사용자 지정을 호스팅하기 위한 단일 Git 저장소가 포함됩니다. 이 단일 Git 저장소는 여러 Adobe Commerce 모듈이 포함된 단일 작성기 패키지로 Composer를 통해 노출됩니다.

![코드가 대량 패키지의 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

- 공유 코드 저장소를 통해 코드 재사용
- 서로 다른 인스턴스에 서로 다른 이전 버전의 GRA를 설치할 수 있는 유연성, 단계별 릴리스 활성화
- GRA의 여러 주요 버전을 백업 및 유지 관리할 수 있는 유연성
- GRA의 시맨틱 버전 관리 지원
- 단순성, 개발자는 일반적인 단일 스토어 개발 패턴보다 더 많은 기술이 필요하지 않습니다
- 특별한 도구, 복잡한 인프라 또는 특별한 분기 전략이 필요하지 않음
- 릴리스의 패키지 조합은 항상 함께 개발 및 테스트됩니다

단점:

- 포함된 모든 패키지를 포함하여 전체 GRA를 업그레이드할 수만 있습니다.
- Adobe Commerce 모듈, 언어 팩 및 테마 이외의 작성기 패키지에 대한 GRA 벌크 패키지는 지원되지 않으므로 메타패키지, magento2 구성 요소 패키지, 작성기 플러그인 및 패치가 없습니다.

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

- **Adobe Commerce 코어**: repo.magento.com을 통해 사용할 수 있습니다.
- **타사 모듈**: 마켓플레이스 또는 공급업체의 자체 Composer 리포지토리를 통해 사용할 수 있습니다.
- **타사 모듈 대체 옵션**: 대량 패키지의 `src/`에 저장됨.
- **GRA 기초 코드**: 기초 벌크 패키지의 `src/`에 저장됩니다.
- **로컬 코드**: 배포 저장소의 `packages/local` 디렉터리에 저장됩니다.

## GRA 모듈 개발

소스에서 벌크 패키지를 설치하여 벌크 패키지 디렉터리에서 Git을 활성화합니다.

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

벌크 패키지는 Git을 사용하여 체크아웃되었습니다. `vendor/antonevers/gra-bulk-foundation` 디렉터리를 입력하면 gra-bulk-foundation Git 저장소도 입력됩니다. 이 디렉터리에서 분기를 만들고, 체크아웃하고, 병합할 수 있습니다.

작성기 종속성을 작성기가 평가하는 벌크 패키지의 유일한 파일인 GRA 벌크 패키지의 루트에 있는 composer.json 파일에 추가합니다.

## GRA 벌크 패키지에 서드파티 모듈 포함

GRA 기반의 루트에 있는 composer.json의 필수 섹션에 서드파티 패키지를 추가하여 GRA에 추가합니다. 이렇게 하면 모든 인스턴스에 항상 작성기를 통해 패키지가 설치됩니다.

## 코드 전달

코드를 주 분기로 전달하려면 2개의 경로가 있습니다. 먼저 주 분기에 병합되는 로컬 모듈을 사용합니다. 해당 모듈에 대해 작성기 업데이트를 실행합니다. 개발자가 충돌을 줄이기 위해 티켓 분기에서 composer.lock을 업데이트할 수 없도록 하십시오. 스테이징 및 프로덕션 분기에서 composer.lock 파일만 업데이트하면 충돌 위험이 줄어듭니다.

두 번째로, GRA 벌크 패키지는 GRA 벌크 저장소의 주 분기로 병합됩니다. 그런 다음 주 분기에 Git 태그를 추가하고 Composer 패키지 버전을 지정할 수 있습니다. 설치하려면 배포 저장소의 composer.json에 새 버전의 GRA 벌크 패키지가 필요합니다.

## 분기 전략

이 GRA 패턴은 GRA 벌크 저장소에 있는 배포 저장소의 분기 전략을 미러링하는 한 모든 분기 전략에서 작동합니다. 릴리스의 경우 두 저장소에서 이름이 같은 릴리스 분기를 만듭니다. 개발을 위해 두 저장소에 티켓 분기를 만듭니다.

티켓 분기에서 composer.lock 파일을 업데이트할 필요가 거의 없습니다. Git을 사용하여 스토어와 GRA 기반 리포지토리 모두에 대해 개발 환경에서 올바른 분기를 확인하십시오. 단, GRA foundation composer.json 파일에서 요구 사항을 업데이트할 때는 예외입니다. 배포 저장소에서 GRA 기반 업그레이드는 릴리스를 빌드하거나 QA 분기를 빌드할 때만 수행됩니다.

## 코드 예

이 문서의 코드 예는 개념 증명을 테스트하는 데 사용할 수 있는 Git 저장소 세트로 사용할 수 있습니다.

- 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-bulk-brand-x>
- GRA 코드 리포지토리: <https://github.com/AntonEvers/gra-bulk-foundation>
- 예제 로컬 모듈: <https://github.com/AntonEvers/module-example-local>
