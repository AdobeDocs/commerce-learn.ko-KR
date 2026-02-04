---
title: 에디션 배너
description: 시각적 요소를 재사용하여 특정 에디션에 적용되는 기능 또는 페이지 확인
source-git-commit: 180bfd303df77d95c88fa518253ddff6a67c76d4
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# 에디션 배너

## EE 전용 기능 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce 기능" src="../assets/adobe-logo.svg" width="20" height="20" /> Adobe Commerce의 전용 기능(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">자세히 알아보기</a>)</td></tr>
</table>

## B2B 전용 기능 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce 기능" src="../assets/b2b.svg" width="20" height="20" /> Adobe Commerce용 <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html">B2B</a>에서만 사용할 수 있는 전용 기능</td></tr>
</table>

## 400개 문제 {#avoid-400-error}

>[!CAUTION]
>
>API 호출을 수행할 때 일종의 searchCriteria가 사용되는지 확인하십시오. 페이지 매김을 사용하는 것도 고려해 볼 수 있습니다. Adobe Commerce의 결과가 너무 크면 Adobe Developer App Builder 용량이 충족되어 파일이 예기치 않게 종료될 수 있습니다. 그 결과 400 오류로 인해 응답 형식이 잘못되었습니다.\
> 예를 들어 Adobe Commerce에서 모든 현재 제품을 요청해야 한다고 가정해 보겠습니다. 결과 URL은 `{{base_url}}rest/V1/products?searchCriteria=all`과(와) 비슷합니다. 반환된 카탈로그의 크기에 따라 App Builder에서 사용하기에는 json이 너무 클 수 있습니다. 대신 페이지 매김을 사용하고 `Response is not valid 'message/http'.`을(를) 피하기 위해 몇 가지 요청을 하십시오.

## PaaS 및 Commere Cloud 전용 {#only-for-on-prem-commerce-cloud}


**이 정보는**&#x200B;에 적용됩니다.

| On-Prem | Adobe Commerce Cloud | Adobe Commerce as a Cloud Service | 아오베 Commerce Optimizer |
| --- | --- | --- | --- |
| 예 | 예 | 아니요 | 아니요 |
