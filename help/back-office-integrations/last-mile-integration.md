---
title: Commerce 스타터 키트의 마지막 마일 통합
description: 유효성 검사, 변환, 전처리, 전송 및 후처리에 확장성 후크를 사용하는 Commerce의 마지막 마일 통합에 대해 알아봅니다.
doc-type: Technical Video
duration: 557
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15869
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
TQID: https://experienceleague.adobe.com/TCR23A98L8XrVDEQeqLQoOXKQPBQu-Wb7YnGUkBXgak
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 342
ht-degree: 0%

---

# Adobe 스타터 키트를 사용한 마지막 마일 통합

타사 시스템과의 연결을 향상시키기 위한 확장성 후크에 중점을 두고 Adobe Commerce과의 마지막 마일 통합을 시작할 때 고려해야 할 항목에 대해 알아봅니다. 이 비디오에서는 유효성 검사, 변환, 전처리, 전송 및 후처리와 같은 다양한 후크를 통해 원활한 데이터 흐름과 시스템 동기화를 보장하는 구조화된 접근 방식에 대해 설명합니다. 각 후크는 다음을 포함한 고유한 목적을 제공합니다.

* 스키마에 대해 들어오는 데이터의 유효성을 검사하는 중
* 시스템 간 데이터 객체 변환
* 관련 정보를 보내기 전에 계산 수행
* 데이터를 대상 시스템으로 전송

각 블록마다 개별 JavaScript 파일을 유지 관리하여 비즈니스 로직 무결성을 유지하고 향후 프레임워크 업그레이드를 용이하게 하여 강력하고 적응력 있는 통합 설정을 보장하는 것이 중요합니다.

사용자가 데이터 동기화 후 주문에 주석을 추가하거나 외부 ID를 저장하는 등의 추가 작업을 수행할 수 있도록 하는 사후 프로세스 후크를 통한 사후 처리 활동의 중요성에 대해 알아봅니다. 이 비디오에는 서드파티 시스템과의 연결을 간소화할 수 있도록 특정 라이브러리 내에 API 요청을 캡슐화하는 것과 같은 모범 사례가 포함되어 있습니다. 또한 각 후크의 일반적인 사용 사례와 다양한 시나리오 처리에 대한 지침을 살펴보게 됩니다.

## 대상자

* 확장성 후크의 구조와 기능 및 이러한 후크가 서드파티 시스템과의 연결을 향상시키는 방법에 대해 알아보고자 하는 개발자입니다.
* 원활한 데이터 흐름, 시스템 동기화 및 효율적인 통합 설정 유지 관리를 용이하게 하기 위해 유효성 검사, 변형, 전처리, 전송 및 후처리와 같은 각 확장성 후크와 관련된 일반적인 사용 사례와 모범 사례를 배우고자 하는 개발자입니다. &#x200B;

## 비디오 콘텐츠

* 마지막 마일 통합에서 호출된 작업의 구조에 대해 알아봅니다.
* 스키마에 대해 들어오는 데이터의 유효성을 검사하고 특정 기준에 따라 특정 이벤트를 건너뛸 수 있는 등 유효성 검사 후크 내의 일반적인 사용 사례를 이해합니다. &#x200B;
* 원본 시스템과 대상 시스템 간에 데이터 개체를 변환할 때 사용되는 변환 후크의 역할에 대해 알아봅니다.
* 대상 시스템으로 실제 데이터를 전송하는 데 사용되는 전송 후크의 중요성에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3451931?captions=kor&learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
