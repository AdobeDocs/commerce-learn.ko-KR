---
title: Adobe Commerce을 사용하여 코드 재사용 최적화
description: 글로벌 참조 아키텍처 패턴을 사용하여 Adobe Commerce에서 코드 재사용을 최적화하여 여러 인스턴스에서 성능과 규정 준수를 향상시키는 방법에 대해 알아봅니다.
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Tony Evers, 수석 기술 설계자, Adobe 제공" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="토니 에버스의 기고문"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# 글로벌 참조 아키텍처 구현 기술

Adobe Commerce에서 코드 재사용을 최적화하는 방법에는 몇 가지가 있습니다. 이 네 가지 구현 기술에는 각각 고유한 장점이 있습니다. 이 문서의 예는 간단명료한 것부터 더 복잡한 것까지 나열되어 있다. 프로젝트 및 향후 로드맵에 가장 적합한 전략을 선택하십시오. 한 전략에서 다른 전략으로의 마이그레이션은 시간이 많이 걸릴 수 있습니다.

## 글로벌 참조 아키텍처를 사용해야 하는 경우

소유한 인스턴스의 수에 따라 글로벌 참조 아키텍처가 유용할 수 있습니다. 인스턴스는 자체 데이터베이스를 사용하는 Adobe Commerce의 독립 실행형 설치입니다. 소유한 인스턴스의 수를 알기 위해 프로덕션 데이터베이스의 수를 카운트합니다. 두 개 이상의 인스턴스를 유지 관리하는 경우 또는 나중에 이 시나리오를 예측하는 경우 글로벌 참조 아키텍처의 이점을 얻을 수 있습니다. 인스턴스가 공유하는 기능이 많을수록 글로벌 참조 아키텍처에 더 많은 가치를 부여합니다.

이러한 시나리오에서는 여러 Adobe Commerce 인스턴스를 사용하여 탐색하는 것이 좋습니다.

1. **다른 저장소 소유자**: 각각 고유한 저장소가 있는 여러 저장소 소유자에 대한 코드를 유지 관리하는 경우 개별 요구 사항을 효과적으로 유지하기 위해 별도의 인스턴스가 필요할 수 있습니다.
2. **국가 규정 준수**: 특정 규정에서는 고객 데이터를 특정 지역에 저장해야 합니다. 이러한 경우 이러한 규정을 준수하기 위해서는 별도의 인스턴스가 필수적입니다.
3. **지리적 영역에 따른 운영 차이**: 여러 지역에서 운영하는 경우 유지 관리 일정과 요구 사항이 다를 수 있습니다. 별도의 인스턴스를 사용하면 이러한 변형을 효율적으로 관리할 수 있습니다.
4. **고강도 플래시 판매**: 대규모 플래시 판매를 수행하는 스토어에는 최적화된 서버 성능이 필요한 경우가 많습니다. 별도의 인스턴스가 제공하는 전용 인프라는 이러한 높은 수요 기간 동안 최적의 성능을 보장합니다.
5. **브랜드 또는 국가 간의 중요한 차이점**: 브랜드 또는 국가 간의 차이가 큰 경우 단일 인스턴스를 사용하면 일부 브랜드 또는 국가에만 사용되는 코드가 발생합니다. 별도의 인스턴스를 사용하면 불필요한 코드를 불필요한 브랜드와 국가에서 제거하여 성능과 안정성을 향상시킬 수 있습니다.

## 글로벌 참조 아키텍처 패턴

&quot;GRA 패턴 없음&quot; 옆에는 네 가지 스타일의 GRA 패턴이 있습니다.

![GRA 패턴 아이콘: GRA 없음, 분할, 벌크, 분리 및 모노리포입니다.](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### GRA 패턴 없음

![GRA 없음을 나타내는 아이콘](/help/assets/global-reference-architecture/no-gra.png){align="center"}

GRA 패턴을 사용하지 않는 경우 각 Adobe Commerce 인스턴스는 고유한 애플리케이션입니다. 코드를 한 인스턴스에서 다른 인스턴스로 수동으로 이동하는 것 외에는 코드를 재사용할 수 없습니다. 이 사본들은 항상 발산한다. 각 인스턴스가 동일한 변경 사항을 갖지만 여전히 예상대로 작동하는지 확인하기 위한 노력의 양은 압도적일 수 있습니다. 이 시나리오에서는 세 인스턴스가 한 인스턴스의 유지 관리 노력의 세 배를 필요로 합니다.

![3개의 저장소를 설명하는 다이어그램입니다. 각 저장소는 이전 저장소에서 복사한 복사본이며, 3개의 복사본에서 고유한 개발이 이루어집니다.](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### 분할 Git GRA 패턴

![분할된 GRA 패턴을 나타내는 아이콘](/help/assets/global-reference-architecture/split-git.png){align="center"}

이 패턴은 개발을 위한 Git 저장소와 인스턴스당 하나의 Git 저장소로 구성됩니다. 인스턴스의 각 파일은 개발 저장소 중 하나에서 유지 관리됩니다. 그들은 전체 GRA를 형성하는 땋은 모양으로 함께 모인다. 각 코드 행은 단일 개발 저장소에만 존재하며 브레이딩 기술을 사용하여 인스턴스에 설치되므로 코드를 재사용할 수 있습니다.

![코드가 분할된 GRA 패턴으로 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### 벌크 패키지는 GRA 패턴을

![대량 GRA 패턴을 나타내는 아이콘](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Adobe Commerce 핵심 및 타사 모듈은 작성기 저장소를 통해 직접 설치됩니다. Git 저장소는 작성기 저장소로 사용할 수 있습니다. 이 패턴에서는 전체 GRA 공유 코드베이스가 단일 또는 몇 개의 Git 저장소에 호스팅되고 Composer를 통해 설치됩니다. 주요 특징은 여러 모듈, 언어 팩 또는 테마가 단일 작성기 패키지로 호스팅되므로 개발이 간소화된다는 것입니다.

![코드가 대량 패키지의 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### 개별 패키지 GRA 패턴

![&quot;개별 패키지&quot; GRA 패턴을 나타내는 아이콘](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

각 Adobe Commerce 모듈, 언어 팩 또는 테마는 별도의 작성기 패키지로 설치됩니다. 각 사용자 지정에는 자체 Git 저장소가 있습니다. 즉, 인스턴스 구성이 최적으로 유연하며 안정적인 작성기 종속성 관리가 가능합니다. 성능 최적화를 위해 모든 패키지가 단일 개인 작성기 저장소에 미러링됩니다.

![코드가 별도의 패키지 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### 모노레포 GRA 패턴

![&quot;모노레포&quot; GRA 패턴을 나타내는 아이콘](/help/assets/global-reference-architecture/monorepo.png){align="center"}

모든 개발은 단일 코드 저장소에서 수행됩니다. 자동화는 새 버전에 대한 패키지를 생성하여 작성기 저장소에 게시합니다. 이 패턴은 벌크 패키지 접근 방식의 낮은 개발 오버헤드와 개별 패키지 접근 방식의 유연성을 결합합니다. 모노레포 패턴은 자동화된 기능 테스트를 실행하는 데에도 이상적입니다.

![코드가 모노레포 GRA 패턴에 저장되는 위치를 보여 주는 다이어그램](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## GRA 패턴 선택

프로젝트 복잡성, 유연성의 필요성, 개발 팀의 적응 능력을 평가하여 GRA 패턴을 선택합니다.

Adobe Commerce 경험이 적은 팀이 가장 간단하게 시작할 수 있습니다. 단, 프로젝트의 특성상 보다 복잡한 GRA 패턴이 필요한 경우 절충하지 않는다.

각 패턴과 관련된 일반적인 프로젝트 특성:

1. **GRA 패턴 없음**: 확장 계획이 없는 Adobe Commerce의 단일 인스턴스. 서로 최소한의 공통성을 갖는 Adobe Commerce의 여러 인스턴스.

2. **Git GRA 패턴 분할**: 사용자 지정에 대한 작성기를 피하려는 팀입니다. 대부분의 경우 벌크 패키지 패턴은 Git 분할에 선호되는 패턴입니다.

3. **대량 패키지 GRA 패턴**: 상호 종속성이 높은 사용자 지정 코드베이스입니다. 인스턴스는 모두 사용자 정의 패키지의 조합이 매우 유사합니다. 개별 패키지의 승격 또는 강등이 자주 발생하지 않습니다. 코드 관리에 대한 경험이 적고 단순성이 필요한 팀.

4. **개별 패키지 GRA 패턴**: 유연한 릴리스 범위 관리가 필요합니다. 향후 5년 이내에 50개 이하의 맞춤형 패키지가 제공될 것으로 예상됩니다. 잠재적으로 공통 코드의 전역 및 지역 계층입니다. Monorepo 패턴으로 마이그레이션할 계획이 없습니다. 그 팀은 기술적으로 숙련되어 있고 엄격한 프로세스 준수를 가지고 있다.

5. **Monorepo GRA 패턴**: 개별 패키지 GRA 패턴의 모든 특성. 향후 5년 이내에 50개 이상의 패키지가 제공될 것으로 예상됩니다. 광범위한 자동화된 테스트 필요. 임시 환경 지원. 동일한 속도로 유지 관리 비용을 확장하지 않고 확장해야 하는 엔터프라이즈 규모의 복잡한 코드베이스

### 잘못된 선택으로 인한 비용

한 패턴에서 다른 패턴으로 마이그레이션이 가능합니다. 아래 다이어그램은 한 패턴에서 다른 패턴으로 이동할 때의 영향 수준을 보여 줍니다. 녹색 선은 영향력이 낮고 노란색 및 주황색 선은 적당히 복잡하거나 복잡한 마이그레이션을 나타냅니다.

![4개의 GRA 패턴 사이의 색 화살표를 보여주는 다이어그램으로, 한 패턴에서 다른 패턴으로의 이동의 어려움 수준을 나타냅니다.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
