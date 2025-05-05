---
title: 품질 패치 도구
description: 문제 진단, 솔루션 찾기 및 사용 가능한 기존 패치 목록에 있는 패치 적용 시 품질 패치 도구를 사용하는 방법에 대해 알아봅니다.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: e306b2cd26506f6a7ef37c2d416be7172dc3c0d2
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# 품질 패치 도구

문제 진단, 솔루션 찾기 및 사용 가능한 기존 패치 목록에 있는 패치 적용 시 품질 패치 도구를 사용하는 방법에 대해 알아봅니다.

## 배울 내용

문제를 평가한 다음 몇 가지 기본 기술을 사용하여 수정 사항을 적용할 품질 패치를 찾는 방법을 알아봅니다.

## 대상자

* 문제를 찾고 이 도구를 활용하여 알려진 문제에 대한 GIT 패치를 적용하는 방법을 학습하는 개발자

## 비디오 콘텐츠

품질 패치 도구는 Adobe Commerce 및 Magento Open Source에 대한 명령줄 유틸리티입니다. 이를 통해 수행할 수 있는 작업:

* 최신 품질 패치에 대한 일반 정보를 확인합니다.
* 설치에 품질 패치를 적용합니다.
* 필요한 경우 적용된 패치 되돌리기

이러한 패치는 Adobe 개발자 및 Magento Open Source 커뮤니티에서 개발하여 안정성과 성능을 향상시킵니다. 향후 업그레이드를 복잡하게 만들 수 있으므로 많은 수의 패치를 적용하는 것은 권장되지 않습니다.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## 품질 패치 도구를 사용하는 이유

다음을 원하는 경우 Adobe Commerce 또는 Magento Open Source용 품질 패치 도구를 사용할 수 있습니다.

안정성 및 성능 향상: 품질 패치는 문제를 해결하고, 보안을 강화하고, 설치를 최적화합니다.
최신 상태 유지: 패치를 적용하면 시스템이 최신 상태로 유지되고 보호됩니다.
변경 사항 되돌리기: 패치로 인해 예기치 않은 문제가 발생하면 도구를 사용하여 되돌릴 수 있습니다. 향후 업그레이드를 복잡하게 하지 않도록 제한된 수의 패치를 적용하는 것이 가장 적합합니다.  

## 품질 패치 도구 사용에 대한 제한 사항 또는 우려 사항

품질 패치 도구 는 이점을 제공하지만 몇 가지 고려 사항이 있습니다.

* 호환성: 패치가 특정 버전의 Adobe Commerce 또는 Magento Open Source과 호환되는지 확인하십시오.
* 테스트: 프로덕션 환경에 적용하기 전에 항상 스테이징 환경에서 패치를 테스트하십시오. 예상치 못한 문제가 발생할 수 있습니다.
* 패치 종속성: 일부 패치는 다른 패치에 따라 달라질 수 있습니다. 전제 조건을 숙지하십시오.
* 사용자 지정: 사용자 지정 코드를 변경한 경우 패치가 충돌할 수 있습니다. 변경 사항을 주의 깊게 검토하십시오.
* 백업: 데이터 손실을 방지하기 위해 패치를 적용하기 전에 설치를 백업합니다.

[품질 패치 도구]는 제한된 수의 패치를 적용하는 데 유용하지만 많은 양의 패치를 처리하는 데는 권장되지 않습니다. 패치를 너무 많이 적용하면 향후 업그레이드 및 유지 관리가 복잡해질 수 있습니다. 적용할 패치가 많은 경우 다른 방법을 고려하거나 Magento 전문가와 상의하십시오. 

## 요약

Quality Patches Tool을 사용하면 전자 상거래 플랫폼에서 패치를 적용하여 안정성과 보안을 향상시킬 수 있습니다. 이러한 패치는 문제를 해결하고 성능을 향상시키며 시스템을 최적화합니다. 설치를 최신 상태로 유지하면 취약점을 방지할 수 있습니다.

패치를 적용하기 전에 스테이징 환경에서 테스트하는 것이 중요합니다. 특정 버전의 Adobe Commerce 또는 Magento Open Source과의 호환성을 확인합니다. 일부 패치에는 종속성이 있을 수 있으므로 사전 요구 사항을 주의 깊게 검토하십시오.

 데이터 손실을 방지하기 위해 패치를 적용하기 전에 설치를 백업합니다. 사용자 지정 코드를 변경한 경우 패치가 충돌할 수 있습니다. 모범 사례를 따르고 각 패치의 영향을 모니터링합니다.

## 관련 문서 및 비디오

* [품질 패치 도구 검색](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=ko)
* [릴리스 정보](https://experienceleague.adobe.com/ko/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* 패치에 대한 [Github](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [품질 패치 도구 사용](https://experienceleague.adobe.com/ko/docs/commerce-operations/tools/quality-patches-tool/usage)
* [QPT의 기술 비디오](https://experienceleague.adobe.com/ko/docs/commerce-learn/tutorials/tools/quality-patch-tool)
