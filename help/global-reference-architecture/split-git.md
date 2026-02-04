---
title: 분할 Git 글로벌 참조 아키텍처로 Adobe Commerce 설정
description: 효율적인 코드 관리 및 간소화된 배포를 위해 분할 Git 글로벌 참조 아키텍처를 사용하여 Adobe Commerce을 설정하는 방법에 대해 알아봅니다. ​
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# 분할 Git 글로벌 참조 아키텍처 패턴

{{only-for-on-prem-commerce-cloud}}

이 안내서에서는 분할 Git 글로벌 참조 아키텍처(GRA) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

분할 Git GRA 패턴에는 개발용 Git 저장소 2개와 Adobe Commerce 인스턴스당 Git 저장소 1개가 포함됩니다. 예제에서 각 인스턴스는 고유한 브랜드를 나타낸다고 가정합니다.

![코드가 분할된 GRA 패턴으로 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

- 공유 코드 저장소를 통해 코드 재사용
- 작성기 지식이 제한된 팀에도 적합한 간단한 GRA 패턴
- Adobe Commerce 모듈, 테마 및 언어 팩 외에도 composer-plugin, composer-metapackage, magento2-component 및 patches를 포함하여 이 모델을 통해 모든 유형의 Composer 패키지를 설치할 수 있습니다
- 단계별로 릴리스 가능, 자체 유지 관리 창에서 지역에 대한 릴리스 계획
- 배포 제어가 아닌 관리 목적으로 Git 태그 지원
- 프로덕션 배포에서 패키지의 조합이 이 정확한 구성에서 개발 및 테스트되도록 보장합니다

단점:

- 다른 GRA 패턴에 비해 유연성이 추가되지 않음
- 인스턴스당 개별 모듈을 업그레이드하거나 다운그레이드할 수 없습니다. 항상 GRA를 전체적으로 업그레이드하거나 다운그레이드하십시오.
- 대부분의 경우 벌크 패키지 패턴은 동일하게 간단하지만 보다 일반적이므로 더 적합합니다

## Git GRA 분할 패턴으로 Adobe Commerce 설정

### 디렉터리 구조

Git GRA 분할 패턴에는 개발 저장소와 설치 저장소, 이렇게 두 가지 유형의 저장소가 있습니다. 개발 저장소에는 전체 Adobe Commerce 설치 중 일부만 포함됩니다. 설치 저장소는 전체 Adobe Commerce 설치를 포함하며 배포에 사용되지만 개발에는 사용되지 않습니다.

분할 Git GRA 패턴을 사용하는 전체 Adobe Commerce 설치의 최종 디렉터리 구조는 다음과 같습니다.

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Composer에서 이러한 디렉터리의 코드를 평가하지 않으므로 `app/code`, `app/i18n` 및 `app/design` 디렉터리가 의도적으로 생략되었습니다. 따라서 패키지에 선언된 종속성은 자동으로 설치되지 않습니다. 분할 Git GRA 패턴은 `packages/`에 모든 사용자 지정 코드를 설치하고 해당 디렉터리를 작성기 저장소로 처리하여 이 문제를 해결합니다. 작성기 패키지가 `packages/`에서 `vendor/`(으)로 동기화됩니다.

### Git 저장소 준비

3개의 Git 저장소 생성 대상:

1. Adobe Commerce 인스턴스
2. Composer를 통해 설치되지 않은 타사 코드
3. 모듈, 테마, 언어 팩 등의 형태로 맞춤화한 사용자 정의, GRA

이 안내서에서는 이러한 저장소에 대해 다음 이름을 사용합니다.

1. gra-split-brand-x
2. gra-split-3rdparty
3. grasplit-gra

Git GRA 분할 패턴의 모든 저장소는 하나로 병합됩니다. Git이 여러 리포지토리를 병합할 수 있도록 하려면 세 리포지토리 모두 공유 기록이 필요합니다. 단일 커밋으로 Git 프로젝트를 만들고 모든 원격에 푸시합니다.

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

임시 파일 `.gitkeep`을(를) 모든 원격에 푸시하면 동일한 커밋 해시로 동일한 초기 커밋이 만들어져서 공유 기록이 만들어집니다. 하나의 원격에서 생성된 각 변경 사항을 다른 원격에 병합할 수 있습니다.

여기에서 저장소는 분기됩니다. gra-split-brand-x 저장소에는 브랜드별 코드가 들어 있습니다. gra-split-3rdparty 저장소에는 타사 코드만 포함됩니다. gra-split-gra 리포지토리에는 모든 사용자 지정 코드로 구성된 전역 참조 아키텍처 기반만 포함됩니다.

gra-split-brand-x 저장소에 Adobe Commerce을 설치합니다.

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

gra-split-3rdparty 및 gra-split-gra 리포지토리에서 초기 커밋을 생성합니다. 가장 쉬운 방법은 이러한 저장소를 별도의 디렉터리에서 체크 아웃하는 것입니다.

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

이 두 저장소는 서드파티 패키지와 GRA 패키지를 저장합니다. Adobe Commerce의 각 인스턴스에 배타적인 코드가 있을 수 있습니다. 이러한 로컬 패키지를 gra-split-brand-x 저장소에 저장할 위치를 만듭니다.

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### 다양한 유형의 코드를 저장할 위치

Adobe Commerce은 Composer 애플리케이션입니다. 설치하는 기본 방법은 항상 Composer 저장소를 통해 수행됩니다. 모듈 공급업체가 Composer 저장소를 통한 설치를 제공하지 않는 경우에만 타사 모듈을 타사 저장소에 저장할 수 있습니다. 사용자 지정 코드의 기본 위치는 GRA 저장소입니다. 특정 인스턴스에서 모듈만 사용하는 경우 로컬 코드가 됩니다.

요약:

- **Adobe Commerce**: Composer 저장소에 저장되었습니다.
- **타사 모듈**: Composer 저장소에 저장됩니다.
- **타사 모듈 대체 옵션**: gra-split-3rdparty Git 저장소에 저장됩니다.
- **GRA Foundation 코드**: gra-split-gra Git 저장소에 저장됩니다.
- **로컬 코드**: gra-split-brand-x Git 저장소에 저장됩니다.

### 패키지 저장소를 작성기에 연결

작성기는 패키지 디렉터리를 작성기 저장소로 처리할 수 있습니다. Composer에 패키지 디렉토리 내의 패키지 위치에 대해 알립니다.

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Composer는 세 개의 저장소 디렉터리에서 두 수준 깊은 composer.json 파일을 찾습니다. `vendor/` 디렉터리에 표시되는 것과 동일한 방식으로 세 개의 코드 저장소 디렉터리 내에 하위 디렉터리를 만듭니다.

예를 들어, 패키지가 일반적으로 `vendor/example-corp/module-example/`에 설치된 경우 `packages/3rdparty/example-corp/module-example/`에 저장합니다. 필요한 경우 작성기가 패키지를 `vendor/example-corp/module-example/`에 심볼릭 링크합니다.

작성기 패키지 네임스페이스와 이름을 디렉터리 구조로 사용합니다. 예를 들어, `app/code/MyCorp/MyCustomization/`에 일반적으로 존재하는 모듈의 이름은 composer.json에서 `my-corp/module-my-customization`입니다. 이 패키지를 `packages/gra/my-corp/module-my-customization`에 저장합니다.

### 인스턴스 저장소에 새 패키지 포함

서드파티 및 GRA 원격의 패키지를 gra-split-brand-x 저장소로 병합합니다.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

그 결과 다음과 같은 디렉토리 구조가 생성됩니다.

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

타사 및 GRA Foundation 저장소의 변경 사항은 브랜드 저장소에 병합됩니다. 이렇게 타사 및 GRA 코드는 한 곳에서만 유지 관리됩니다. Git 병합을 사용하여 변경 사항을 브랜드로 이동합니다.

Adobe Commerce은 새 모듈을 자동으로 인식하지 않습니다. 실행 작성기는 병합 후 새 패키지를 추가해야 합니다. 병합 후 패키지 중 하나를 업데이트할 때마다 작성기 업데이트를 실행합니다.

### 예제 모듈 설치

개념 증명으로 예제 모듈을 설치하여 GRA 패턴이 어떻게 작동하는지 확인합니다.

이동하기 전에 `composer install` 및 `bin/magento install`을(를) 실행하십시오.

GitHub에는 3개의 테스트 모듈이 있습니다.

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### 로컬 모듈 설치

로컬 코드 풀에 모듈을 추가하는 방법은 간단합니다. 모듈을 다운로드하고 추출합니다. 작성기에 필요합니다. `bin/magento`을(를) 사용하여 사용하도록 설정하고 브랜드 저장소에 파일을 커밋합니다.

```bash
cd gra-split-brand-x
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

위의 출력이 표시되면 브랜드 저장소에 안전하게 커밋할 수 있습니다.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### GRA 기반 모듈 설치 및 개발

GRA 저장소에 모듈을 추가하는 것은 로컬 모듈을 설치하는 것과 다릅니다. 기본적으로 커밋은 gra-split-brand-x 저장소인 origin/main에 추가됩니다. GRA 모듈의 변경 사항은 gra-split-gra 저장소로 푸시하고 나중에 gra-split-brand-x 저장소로 병합해야 합니다.

### 개발 환경 만들기

모든 코드 풀을 한 곳에 조합하여 개발 환경을 만듭니다. 심볼릭 링크를 통해 로컬, GRA 및 서드파티 리포지토리에 개별적으로 코드를 푸시할 수 있습니다. 브랜드, GRA 및 서드파티 저장소 디렉터리 옆에 새 개발 디렉터리를 만들어 시작합니다.

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

그 결과 다음과 같은 디렉토리 구조가 생성됩니다.

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

gra-development 디렉터리에서 `composer install` 및 `bin/magento install`을(를) 실행합니다.

이제 `packages/3rdparty`, `packages/gra` 및 `package/local` 디렉터리에서 직접 변경 내용을 커밋할 수 있습니다. Git은 디렉토리 심볼릭 링크가 있는 Git 저장소에 변경 사항을 커밋합니다. `packages/3rdparty`, `packages/gra` 또는 `package/local` 디렉터리 내에서 git commit 명령을 실행해야 합니다. 프로젝트 루트에서 git 커밋을 실행하지 마십시오.

### 예제 모듈 설치

패키지 디렉터리에 타사 및 GRA 예제 모듈을 설치합니다.

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

이 마지막 명령을 실행하면 모듈이 설치되고 작동하고 있음을 입증하기 위해 다음과 같은 출력이 발생합니다.

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

위의 출력이 표시되면 브랜드 저장소에 안전하게 커밋할 수 있습니다. `git remote -v`을(를) 실행하여 올바른 원격으로 커밋하고 있는지 확인하십시오.

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### 인스턴스에 코드 전달

GRA 및 서드파티 저장소를 gra-split-brand-x 저장소에 병합하여 코드를 Adobe Commerce 인스턴스에 전달합니다. `composer require`, `bin/magento module:enable`을(를) 실행하고 결과를 커밋하십시오.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## 분기 전략

이 GRA 패턴은 서드파티 및 GRA 저장소에 있는 저장소 저장소의 분기 전략을 미러링하는 경우 모든 분기 전략에서 작동합니다. 릴리스의 경우 세 개의 저장소 모두에서 동일한 이름을 가진 릴리스 분기를 만듭니다. 릴리스 준비 중 저장소 저장소에서 릴리스 분기를 함께 병합합니다.

때로 지역 코드와 타사 코드 또는 GRA 코드를 모두 변경해야 하는 티켓 분기가 있습니다. 이 경우 티켓 분기는 모든 관련 저장소에 만들어야 합니다.

서드파티 및 GRA는 티켓 분기 내의 브랜드 저장소에 커밋하지 않습니다. 대신, 개발 환경에서 각 코드 풀에 대해 올바른 분기를 확인하십시오. 릴리스를 작성하거나 QA 분기를 작성할 때만 브랜드 리포지토리에 병합이 수행됩니다.

## 코드 예

이 문서의 코드 예는 개념 증명을 테스트하는 데 사용할 수 있는 Git 저장소 세트로 사용할 수 있습니다.

- 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-split-brand-x>
- 타사 코드 리포지토리: <https://github.com/AntonEvers/gra-split-3rdparty>
- GRA 코드 리포지토리: <https://github.com/AntonEvers/gra-split-gra>
- 예제 로컬 모듈: <https://github.com/AntonEvers/module-example-local>
- 예제 GRA 모듈: <https://github.com/AntonEvers/module-example-gra>
- 예제 타사 모듈: <https://github.com/AntonEvers/module-example-3rdparty>
