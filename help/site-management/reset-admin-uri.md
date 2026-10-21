---
title: CLI를 사용하여 관리자 URI 재설정
description: Adobe Commerce Cloud CLI에서 관리 URI를 재설정하는 방법에 대해 알아봅니다. 이 방법은 관리자 URL 변경으로 인해 액세스 문제가 발생하는 경우 유용합니다.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 144
last-substantial-update: 2024-10-14T00:00:00.000Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
TQID: 'https://experienceleague.adobe.com/OgyaTHVhQSRFApJPYwkzodSyAJcBjwk8kIrw4VBXltQ'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: c4af0798-d497-5e6b-8380-19812c26d00a
    internal-label: Console
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%
---
# cli를 사용하여 관리자 URI 재설정

Adobe Commerce Cloud cli 명령을 사용하여 관리 URI를 재설정하는 방법에 대해 알아봅니다. 이 기능은 관리자로부터 관리자 URL을 변경했지만 오류가 발생하여 더 이상 관리자에 액세스할 수 없는 경우에 유용합니다.

>[!VIDEO](https://video.tv.adobe.com/v/3439699?captions=kor&learn=on)

## 자습서에서 사용되는 일부 명령

사용자 지정 관리자 경로 URL을 0으로 예상하도록 구성을 변경합니다.

`$ php bin/magento config:set admin/url/use_custom 0`

사용자 지정 관리 경로 URL에 대한 구성을 0으로 변경합니다.

`$ php bin/magento config:set admin/url/use_custom_path 0`
