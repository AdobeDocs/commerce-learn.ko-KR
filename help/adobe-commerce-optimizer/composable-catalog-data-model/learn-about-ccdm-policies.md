---
title: 구성 가능한 카탈로그 데이터 모델의 CCDM 정책에 대해 알아보기
description: Adobe 구성 가능한 카탈로그 데이터 모델의 STATIC 및 TRIGGER 정책이 카탈로그를 다시 빌드하지 않고 카탈로그 보기 간에 제품 가시성을 제어하는 방법에 대해 알아봅니다.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 349
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Adobe 구성 가능 카탈로그 데이터 모델의 정책

**카탈로그 보기**&#x200B;가 구매자가 통합 기본 카탈로그에서 보는 모양을 형성하는 렌즈인 경우 **정책**&#x200B;이 렌즈로 만들어집니다. 이 튜토리얼에서는 정책의 정의, **Carvelo Automoals** 데모 시나리오에서 **STATIC**&#x200B;과(와) **TRIGGER** 정책이 함께 작동하는 방식 및 카탈로그를 다시 작성하지 않고 정책 업데이트가 즉시 적용되는 이유를 설명합니다.

## 이 비디오는 누구의 것입니까?

* Commerce에서 카탈로그 보기 및 머천다이징 규칙을 구성하는 Adobe Commerce Optimizer 솔루션 설계자 및 개발자

## 비디오 콘텐츠

* 제품 특성에 대한 데이터 액세스 필터로서의 정책
* Celport 카탈로그 보기에서 여러 정책이 결합하는 방법(브랜드 및 부품 범주)
* 영구 비즈니스 규칙에 대한 정적 정책
* API 요청 헤더에 의해 활성화된 트리거 정책(예: `AC-Policy-Brand`)
* 카탈로그 재구축 없이 일상적인 작업에서 정책 업데이트

>[!VIDEO](https://video.tv.adobe.com/v/3491413?learn=on)

**정책**&#x200B;은(는) **데이터 액세스 필터**&#x200B;입니다. 제품 속성을 검사하고 카탈로그 보기에서 노출할 수 있는 제품을 결정하는 규칙을 적용합니다. 정책은 공유 구성 가능한 카탈로그의 맨 위에 있으며 카탈로그 데이터를 복제하지 않습니다.

## 정적 정책

**STATIC** 정책은 **always on**&#x200B;입니다. 쇼핑객 동작 또는 세션 상태에 관계없이 영구 비즈니스 규칙을 적용합니다.

Celport 시나리오에서 STATIC 규칙은 다음과 같습니다.

* Celport는 **Bolt** 및 **Cruz** 브랜드만 판매합니다.
* Celport는 **브레이크** 및 **일시 중단** 부분만 표시합니다.

이러한 규칙은 절대 해제되지 않습니다. STATIC 정책은 **라이선스 계약**, **영역 제한** 및 **브랜드 권한**&#x200B;이 있는 곳입니다. 한 번 설정하면 Adobe Commerce Optimizer에서 모든 요청에 자동으로 적용합니다.

## 트리거 정책

값 원본이 **TRIGGER**&#x200B;인 정책을 **배타 정책**&#x200B;이라고도 합니다. 카탈로그 보기는 API 호출의 헤더에 트리거가 지정&#x200B;**된 경우에만 해당 정책**&#x200B;을(를) 실행합니다.

예를 들어 EDS(Experience Delivery Services) 상점에서는 **모든 브랜드**, **Aurora**, **볼트** 및 **Cruz**(으)로 드롭다운을 제공할 수 있습니다.

* 초기 페이지 보기에서는 아무것도 선택되지 않으므로 추가 브랜드 헤더가 전송되지 않습니다.
* 구매자가 **볼트**&#x200B;를 선택하면 기본 API가 `AC-Policy-Brand` 헤더를 `Bolt`(으)로 설정합니다.
* 쇼핑객이 **Cruz**&#x200B;을(를) 선택하면 동일한 헤더가 `Cruz`(으)로 설정됩니다.

카탈로그 보기는 해당 헤더가 있는 경우에만 TRIGGER 정책을 적용하며, 영구 STATIC 규칙을 변경하지 않고 대화형 필터링을 지원합니다.

## 정적 및 트리거 동시 실행

**STATIC** 정책은 영구 규칙을 처리합니다. **TRIGGER** 정책은 대화형 정책을 처리합니다. 단일 카탈로그 보기에서 함께 사용하면 **규정 준수** 및 **유연성**&#x200B;을 모두 제공합니다. 고정된 분류 경계와 구매자 중심의 세분화가 제공됩니다.

## 카탈로그를 다시 작성하지 않고 정책 업데이트

일상적인 작업의 경우, 정책 변경이 즉시 이루어집니다. Celport의 라이선스 계약에 따라 이제 **타이어**&#x200B;와 브레이크 및 일시 중지가 허용된다고 가정해 보십시오.

1. 관련 정책을 엽니다.
1. 값 목록에 **타이어**&#x200B;을(를) 추가합니다.
1. 저장.

변경 내용은 **즉시 실행**&#x200B;입니다. 카탈로그 다시 빌드, 다시 배포 또는 대기 시간이 없습니다. Celport 상점에 대한 다음 요청은 업데이트를 반영합니다.

정책은 **공유 카탈로그**&#x200B;의 간단한 필터이며 별도의 카탈로그 복사본으로 만들어진 규칙이 아닙니다. **데이터가 아닌 규칙을 변경합니다.**

## 관련 컨텐츠

* [구성 가능한 카탈로그 데이터 모델이 있는 이유](./why-ccdm-exists.md)
* [카탈로그 보기에 대해 알아보기](./learn-about-the-ccdm-feature-catalog-views.md)
* [머천다이징 서비스에 대한 카탈로그 보기](https://experienceleague.adobe.com/ko/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [[!DNL Adobe Commerce Optimizer] 안내서](https://experienceleague.adobe.com/ko/docs/commerce/optimizer/overview){target="_blank"}
* [머천다이징 API 시작하기](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

