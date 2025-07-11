---
title: Fastly를 사용하여 전체 웹 사이트에 대한 액세스를 거부하십시오.
description: Fastly Edge ACL 및 사용자 지정 VCL을 사용하여 Adobe Commerce 사이트 액세스 제한
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Fastly를 사용하여 전체 웹 사이트에 대한 액세스를 거부하십시오.

Fastly Edge ACL 및 사용자 지정 VCL 코드 조각을 사용하여 Adobe Commerce Cloud 사이트에 대한 액세스를 제한하는 방법에 대해 알아봅니다. 이 단계별 안내서는 특정 IP 주소만 허용하여 개발 사이트가 비공개로 유지되도록 함으로써 출시 전 환경을 보호하는 데 도움이 됩니다.

## 배울 내용

Fastly Edge ACL 및 사용자 지정 VCL을 사용하여 Adobe Commerce 사이트 액세스 제한 | 출시 전 환경 보호

## 이 비디오는 누구의 것입니까?

* DevOps 엔지니어
* Adobe Commerce 개발자
* Site Reliability Engineer

>[!VIDEO](https://video.tv.adobe.com/v/3464779/?learn=on&enablevpops)

## 코드 샘플

다음은 사용된 VCL 예입니다

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## 관련 설명서

* [악성 IP 주소 검색](https://experienceleague.adobe.com/ko/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* 요청을 허용하기 위한 [사용자 지정 VCL](https://experienceleague.adobe.com/ko/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [차단 요청에 대한 사용자 지정 VCL](https://experienceleague.adobe.com/ko/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)