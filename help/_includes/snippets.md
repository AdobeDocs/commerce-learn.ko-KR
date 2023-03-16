---
title: 편집 배너
description: 특정 버전에 적용되는 기능 또는 페이지를 메모하기 위해 시각적 요소를 재사용합니다.
source-git-commit: 8c7c64ddff456815b0a1649f497e917da8b8fca0
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 0%

---

# 편집 배너

## EE 전용 기능 {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Adobe Commerce 기능" src="../assets/adobe-logo.svg" width="20" height="20" /> Adobe Commerce의 전용 기능(<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">추가 정보</a>)</td></tr>
</table>

## B2B 전용 기능 {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Adobe Commerce 기능" src="../assets/b2b.svg" width="20" height="20" /> 전용 기능은 <a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html#product-editions">Adobe Commerce용 B2B</a></td></tr>
</table>

## 400개 문제 {#avoid-400-error}

>[!CAUTION]
>
>API 호출을 수행할 때 일부 종류의 searchCriteria가 사용되는지 확인하십시오. 페이지 매김 사용을 고려해 볼 수도 있습니다. Adobe Commerce의 결과가 너무 큰 경우 Adobe Developer App Builder 용량이 충족될 수 있고 이로 인해 파일이 예기치 않게 종료됩니다. 결과는 400 오류로 인해 잘못된 형식의 응답 결과입니다.\
> 예를 들어 Adobe Commerce에서 모든 현재 제품을 요청해야 한다고 가정합니다. 결과 URL은 다음과 같습니다 `{{base_url}}rest/V1/products?searchCriteria=all`. 반환된 카탈로그의 크기에 따라 App Builder가 소비하기에 JSON이 너무 클 수 있습니다. 대신 페이지 매기기를 사용하고 몇 가지 요청을 수행하여 을 방지하십시오 `Response is not valid 'message/http'.`
