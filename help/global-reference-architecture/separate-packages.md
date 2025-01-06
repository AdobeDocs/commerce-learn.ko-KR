---
title: 개별 패키지 글로벌 참조 아키텍처
description: 별도의 패키지 GRA로 Adobe Commerce을 최적화합니다. 유연한 버전 패키지 관리를 위한 설정, 이점 및 모범 사례에 대해 알아봅니다.
kt: 16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
source-git-commit: 916586d3b71b8b74baa04af2ab39abc86ec94f5b
workflow-type: tm+mt
source-wordcount: '2088'
ht-degree: 0%

---


# 개별 패키지 글로벌 참조 아키텍처 패턴

이 안내서에서는 개별 패키지 글로벌 참조 아키텍처(GRA) 패턴을 사용하여 Adobe Commerce을 설정하는 방법을 설명합니다.

개별 패키지 GRA 패턴에는 각 공통 패키지에 대한 Git 저장소와 각 Adobe Commerce 인스턴스에 대한 Git 저장소가 포함됩니다. 일반 패키지는 Composer를 통해 비공개 Composer 리포지토리를 통해 노출됩니다.

이 전역 참조 아키텍처 패턴은 완전히 작성기 기반이며 모든 작성기 기능을 최대한 활용할 수 있도록 설계되었습니다.

![코드가 개별 패키지 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## 이 패턴의 장단점

장점:

- 공유 코드 저장소를 통해 코드 재사용
- 패키지 설치의 완벽한 유연성, 각 GRA 패키지를 개별적으로 업그레이드, 다운그레이드 또는 백포팅할 수 있습니다.
- 시맨틱 버전 관리를 위한 완벽한 지원
- 특별한 도구, 복잡한 인프라 또는 특별한 분기 전략이 필요하지 않음
- Composer가 지원하는 모든 패키지 유형 지원

단점:

- 이러한 GRA 패턴 내에서의 개발은 처음에는 약간 더 어려우며, 학습곡선이 작다
- 동일한 구성에서 개발되지 않은 패키지 조합을 배포할 수 있으므로 엄격한 테스트 절차가 필요합니다.

## 개별 패키지 GRA 패턴으로 Adobe Commerce 설정

### 디렉터리 구조

개별 패키지 GRA 패턴이 있는 전체 Adobe Commerce 설치의 최종 디렉터리 구조는 다음과 같습니다.

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

`app/code`, `app/i18n` 및 `app/design` 디렉터리는 의도적으로 생략되었습니다. 개별 패키지 GRA는 Composer에서 모든 단일 패키지를 설치합니다. 패키지가 단일 Adobe Commerce 인스턴스에만 설치된 경우에도

### 저장소 저장소 준비

Brand X의 웹 스토어를 나타내는 첫 번째 Adobe Commerce 인스턴스에 대한 저장소를 만듭니다.

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

`bin/magento setup:install`(으)로 Adobe Commerce을 설치합니다. 결과 `app/etc/config.php` 커밋.

### 패키지 저장소 생성

이 전역 참조 아키텍처 패턴의 각 패키지에는 고유한 Git 저장소가 있습니다. 다음은 GRA 모듈, 서드파티 모듈 및 로컬 모듈을 나타내는 Adobe Commerce 모듈이 포함된 예제 패키지입니다.

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

예를 사용하여 나만의 패키지를 만듭니다.

### 메타패키지 저장소 생성

메타패키지는 이 GRA 패턴에서 GRA 공통 코드베이스의 범위를 제어한다. 이 관리자는 항상 함께 설치되는 패키지 버전의 조합을 기반으로 하는 항목을 정의합니다. 예:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

위의 스니펫은 메타패키지의 composer.json입니다. 메타패키지에는 composer.json 파일만 포함되고 다른 코드는 포함되지 않기 때문입니다. 위의 코드도 전체 메타패키지입니다. Git 저장소에 호스팅할 수 있으며 설치할 수 있는 메타패키지 작성기 저장소가 있습니다. 여기에는 한 가지 GRA 모듈 및 서드파티 모듈과 Adobe Commerce 코어가 필요합니다. 또한 다음 장에서 설명할 gra-component-foundation도 필요합니다.

메타패키지는 패키지 간에 종속성을 만들지 않고 패키지를 번들로 만드는 방법입니다. 따라서 패키지 간에 기술적 종속성이 없더라도 메타패키지를 사용하면 함께 설치할 수 있습니다. 이 메타패키지가 프로젝트에 필요한 경우 메타패키지에 필요한 모든 패키지 또는 메타패키지가 설치됩니다. 따라서 빈 Composer 프로젝트를 만들고 이 패키지만 필요한 경우 Composer는 Adobe Commerce과 GRA 및 서드파티 모듈을 설치합니다.

이렇게 하면 각 스토어에 동일한 기본 패키지 세트가 포함되어 있는지 확인할 수 있습니다.

마찬가지로 스토어 x를 정의하는 메타패키지를 정의할 수 있습니다. 이를 위해서는 기초 메타패키지가 필요하며, 전체 GRA 기초와 로컬 모듈이 필요합니다.

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Brand-X 메타패키지는 선택 사항입니다. 브랜드 메타패키지를 건너뛰고 스토어 작성기 프로젝트에서 이러한 종속성을 직접 요구할 수도 있습니다. 로컬 모듈용 메타패키지를 만들면 패키지 저장소에만 있는 저장소 Git 저장소에 기능 분기 및 기능 가져오기 요청이 없는 이점이 있습니다. 그것은 안전 조치이다. 또한 패키지 저장소에 시맨틱 버전 관리를 적용하고 주 프로젝트에서 다른 Git 태그를 사용하여 지정된 릴리스를 추적할 수 있습니다. 당신에게 달렸어요.

### 공급업체 디렉토리 외부의 GRA 기반 파일

경우에 따라 공급업체 디렉터리 외부에 파일을 저장해야 합니다. `.gitignore`과(와) 같이 `dev/` 디렉터리 또는 도메인 확인 파일에 있는 파일입니다. magento2 구성 요소 패키지 유형은 이 용도로 디자인되었습니다. <https://github.com/AntonEvers/gra-component-foundation> 보기.

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

이 패키지에는 magento2-component 유형이 있으며 Adobe Commerce 루트 디렉토리에 복사된 파일을 호스팅하는 src 디렉토리가 포함되어 있습니다. 이 파일의 매핑은 기본 작성기 프로젝트의 `/src/gitignore`을(를) `/.gitignore`에 복사합니다.

이렇게 하면 공급업체 디렉토리 외부의 파일도 GRA 기반의 일부로 만들 수 있습니다.

### GRA 기반 모듈 개발

공급업체 디렉토리 내에서 개발이 이루어집니다. Composer에 소스에서 Foundation 패키지를 설치하도록 요청하십시오. 이렇게 하면 다운로드한 아카이브에서 패키지를 설치하는 대신 Git에서 패키지를 체크아웃합니다.

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

이 명령을 사용하면 antonevers 네임스페이스의 패키지가 Git을 사용하여 체크아웃됩니다. vendor/antonevers/module-gra 디렉터리를 입력하면 module-git 저장소도 입력하게 됩니다. 이제 공급업체 디렉터리에서 바로 이러한 방식으로 분기를 만들고, 체크아웃하고, 병합할 수 있으며 개발할 수 있습니다.

### GRA 재단에 타사 모듈 포함

GRA 메타패키지에 서드파티 패키지를 추가합니다. 작성기 저장소에서 서드파티 코드를 설치할 수 없는 경우 해당 패키지를 만듭니다. Git 저장소를 만들고 패키지 콘텐츠(앱/코드/공급업체/패키지에 있는 모든 것)를 추가한 다음 저장소의 루트에 유효한 composer.json 파일이 있는지 확인합니다. 이제 Composer를 통해 이 패키지를 설치할 수 있습니다.

## 개인 작성기 저장소 설정

개인 저장소는 글로벌 참조 아키텍처에서 필수가 아닙니다. 따라서 배포 및 설치 속도가 빨라지고 composer.json의 저장소 구성이 줄어들고 보안이 강화됩니다. 다른 Composer 저장소 및 Adobe Commerce 마켓플레이스에 대한 자격 증명은 개인 저장소에 저장됩니다. 코드 또는 개발자 시스템과 번들로 제공되는 더 이상 중요한 자격 증명이 없습니다.

또한 일부 개인 저장소는 저장소 중 하나에 종속성 중 하나에 보안 취약점이 포함되어 있는 경우 이메일 알림과 같은 추가 기능을 제공합니다.

composer.json에 여러 VCS 저장소가 있는 경우 느린 문제가 발생합니다. 업그레이드를 수행할 때 각 Composer 저장소를 읽어야 하며 50개 패키지에 대해 50개의 저장소를 사용하면 단일 Composer 저장소보다 최소 50배 많은 오버헤드가 발생합니다.

![작성기 저장소가 없을 때 속도가 느려지는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

개인 Composer 저장소 형태로 Composer 미러를 포함합니다. 미러에는 다른 Composer 리포지토리의 모든 패키지 사본과 호스팅된 모든 Git 패키지가 포함됩니다. 개인 Composer 저장소를 사용하면 세분화된 액세스 제어를 추가로 사용할 수 있습니다.

Git 동기화를 사용하면 개인 Composer 저장소가 Git 저장소의 새 패키지와 기존 패키지의 새 버전을 자동으로 검색합니다.

Satis를 사용하여 개인 리포지토리를 호스팅할 수 있습니다. <https://composer.github.io/satis/>. <https://antonevers.github.io/gra-composer-repository/>의 예제 공용 리포지토리를 참조하십시오. 이 리포지토리는 코드 예에서 작성기 리포지토리로 사용됩니다. Satis 저장소를 비공개로 설정하려면 추가 조치가 필요합니다.

Composer 또는 JFrog Artifactory <https://jfrog.com/artifactory/>을(를) 작성한 동일한 사용자가 만든 비공개 패키지 목록 <https://packagist.com/>을(를) 구성하고 잊어버릴 수 있는 해결 방법이 있습니다.

## 코드 전달

메타패키지를 사용하면 코드를 전달하는 세 가지 단계가 있습니다.

1. 변경 사항을 패키지로 병합하고 변경된 패키지의 버전을 지정합니다.
2. (선택 사항, 새 패키지가 추가되는 경우에만) 메타패키지 및 버전에서 새 패키지가 필요합니다.
3. (선택 사항, 새 패키지가 추가되는 경우에만) Adobe Commerce 및 배포에 새 메타패키지가 필요합니다.

배포 범위는 패키지 버전으로 제어됩니다. 안정적인 버전의 패키지를 생성하면 이 패키지가 프로덕션 배포에 준비되었음을 의미합니다.

새 릴리스를 빌드하려면 전체 스토어 설치가 포함된 기본 Composer 프로젝트에서 Composer 업데이트를 실행합니다. 최신 버전의 패키지가 모두 설치되어 있습니다.

## 버전 관리

개별 패키지에서 버전 관리 GRA는 Git의 태그 지정 모듈과 동의어입니다. Git 태그는 Composer가 설치하는 패키지의 번호 버전을 만듭니다.
올바른 버전 관리 접근 방식을 사용하면 패키지가 자동으로 흐르면서 안전성도 유지할 수 있습니다.

두 가지 예:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

이 예는 종속성에 대한 엄격한 정의를 보여 줍니다. 정확한 버전에는 3개의 패키지가 필요합니다. 설치에서 이 메타패키지를 사용하여 작성기를 업데이트해도 아무 작업도 수행되지 않습니다. 최신 버전을 사용할 수 있는 경우에도 이러한 정확한 버전에 항상 이 3개의 패키지를 설치합니다.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

이 예는 종속성에 대한 느슨한 정의를 보여 줍니다. `~1.0`을(를) 사용하면 `1.0.0`보다 크거나 같고 `2.0.0`보다 낮은 패키지 버전을 설치할 수 있으며, 사용 가능한 가장 높은 버전을 기본 설정으로 사용합니다. <https://getcomposer.org/doc/articles/versions.md>에서 버전 종속성을 정의하는 방법에 대해 자세히 알아볼 수 있습니다.

> ~ 연산자는 예제에서 가장 잘 설명합니다. `~1.2`은(는) `>=1.2 <2.0.0`과(와) 같고 `~1.2.3`은(는) `>=1.2.3 <1.3.0`과(와) 같습니다.

언급된 패키지의 새 버전을 릴리스하는 즉시 Composer 업데이트와 함께 자동으로 설치됩니다.

시맨틱 버전 관리를 적용합니다. <https://semver.org/>에서 시맨틱 버전 관리에 대한 모든 것을 배울 수 있습니다. 특히 FAQ는 반드시 읽어야 합니다. 시맨틱 버전 관리를 사용할 때 &quot;1.0.0&quot;의 숫자는 MAJOR.MINOR.PATCH으로 불립니다. 패키지의 마이너 및 패치 릴리스는 애플리케이션을 중단하지 않고 안전하게 제공되어야 합니다.
패치를 자동으로 포함하고 작은 업그레이드를 수동으로 선택할 수 있습니다. 이렇게 하면 각 사소한 변경 사항을 수동으로 선택하여 추가 오버헤드가 발생합니다.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

물론 이 모든 것은 시맨틱 버전 관리를 항상 일관되게 적용할 경우에만 작동합니다. 메타패키지뿐만 아니라 일반 패키지의 요구 사항도 종속성을 느슨하게 정의해야 합니다. 시스템에 하나의 엄격한 종속성이 있는 경우 해당 패키지는 엄격한 정의로 제한됩니다.

composer depends \&lt;package name\>을 입력하여 이러한 엄격한 종속성을 찾으십시오. 자세한 내용은 <https://getcomposer.org/doc/03-cli.md#depends-why>을(를) 참조하십시오.

## 분기 전략

주 분기가 패키지 버전을 지정하는 유일한 분기인 경우 다양한 분기 전략을 사용하여 이 글로벌 참조 전략 패턴을 지원할 수 있습니다. 여러 분기에 걸쳐 버전을 지정하는 경우 버전 간에 기능이 임의로 손실될 위험이 있습니다. 주 분기에서는 안정적인 버전만 만듭니다.

패키지 저장소에서는 기능 분기만 생성합니다. 스토어 설치 저장소에는 없습니다. Composer를 사용하여 스토어에 변경 사항을 도입할 수 있습니다. 배포 저장소에서 Git 병합이 필요하지 않습니다.

분기 전략 및 저장소에서 일반적인 분기 유형은 다음과 같습니다.

**기능 분기**: 패키지 저장소에 없습니다.

**릴리스 분기**: 패키지, 메타패키지, 저장소 설치 저장소 등 모든 저장소에 만들어집니다. 릴리스를 계획할 때 버전 지정을 하기 전에 패키지의 릴리스 분기에 있는 변경 사항을 그룹화합니다. 코드 이름 &quot;유니콘&quot;을 사용하여 릴리스를 준비한다고 가정해 봅시다. 변경 사항이 포함된 패키지에서 릴리스 유니콘 분기를 만들 수 있습니다. 모든 항목을 병합한 다음 메타패키지에 &quot;dev-release-unicorn as 1.4.0&quot;이 필요합니다. 별칭에 대해 자세히 알아보세요. <https://getcomposer.org/doc/articles/aliases.md>.

**QA/개발 분기**: 릴리스 분기와 유사합니다.

**주 분기**: 모든 저장소에 있어야 하며 항상 프로덕션 또는 프로덕션 준비 상태를 나타내는 분기여야 합니다. 주 분기는 릴리스 버전에 코드를 태깅하는 위치입니다.
유지 관리 오버헤드가 적은 분기 전략을 선택해야 합니다. 예를 들어 핫픽스 릴리스 이후 QA, UAT, 릴리스 또는 개발 분기에 주 분기를 다시 병합하는 것은 오버헤드 유지 관리 작업입니다. 패키지가 많을수록 저장소가 많아지고 오버헤드가 발생하는 작업이 반복됩니다.

mixu/gr과 같은 도구를 사용하여 일괄 처리의 여러 Git 저장소에서 일상적인 작업을 수행합니다. <https://github.com/mixu/gr>

## 이름 지정 규칙

개별 패키지 GRA 패턴을 사용하면 기초 메타패키지에 패키지가 필요한 경우 패키지는 GRA 기초의 일부입니다. 메타패키지에서 패키지를 추가하거나 제거하여 기초 내부 및 외부로 이동합니다.

메타패키지는 패키지의 설치 범위를 유연하게 합니다. 패키지 이름에 패키지의 의도된 사용과 관련된 단어가 포함되어 있지 않은 것이 더욱 중요합니다. GRA 재단에서 해당 패키지를 제거하기로 결정할 때 antonevers/module-gra-store-locator라는 이름이 혼란스러울 수 있습니다. 범위(GRA, foundation, local)를 사용하지 마십시오. 지역(EMEA, 스페인, 글로벌)은 피하십시오. 대부분의 경우 패키지를 빌드하는 상점의 이름은 피해야 합니다. 패키지에 추가된 기능에만 관련된 이름을 선택합니다. 이렇게 하면 원하는 곳에서나 예측할 수 없는 미래 시나리오에서도 재사용할 수 있습니다. antonevers/module-store-locator라는 이름은 매우 훌륭합니다.

관련 패키지가 개요에 함께 표시되는지 확인하십시오. 제네릭에서 특정으로 이름을 빌드합니다. 따라서 antonevers/module-b2b-tax-exempt 대신 antonevers/tax-exempt-module-b2b가 면제됩니다.

## 코드 예

이 블로그 게시물의 코드 예는 개념 증명으로 재생하는 데 사용할 수 있는 Git 저장소 집합에 결합되었습니다.

- 예제 프로덕션 저장소: <https://github.com/AntonEvers/gra-separate-brand-x>
- 예제 foundation 모듈: <https://github.com/AntonEvers/module-example-gra>
- 예제 타사 모듈: <https://github.com/AntonEvers/module-example-3rdparty>
- 예제 로컬 모듈: <https://github.com/AntonEvers/module-example-local>
- 예제 foundation 메타패키지: <https://github.com/AntonEvers/gra-meta-foundation>
- 예제 로컬 메타패키지(선택 사항): <https://github.com/AntonEvers/gra-meta-brand-x>
- 예제 작성기 리포지토리: <https://github.com/AntonEvers/gra-composer-repository>