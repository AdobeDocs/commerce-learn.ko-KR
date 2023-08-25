---
title: Adobe Commerce에 기본적으로 제공되는 카탈로그 가져오기 옵션에 대해 알아보기
description: 카탈로그를 Adobe Commerce 스토어로 가져오기 위한 몇 가지 기본 옵션에 대해 알아봅니다.
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
source-git-commit: 273119420a7051b1833d9b796bdce36e17d893c7
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 0%

---

# 카탈로그 가져오기 옵션 알아보기

카탈로그를 Adobe Commerce으로 가져오는 몇 가지 기본 방법이 있습니다. 각 방법에는 반드시 고려해야 할 장단점과 함께 나름의 사용 추론이 있다.

아래 옵션 중 하나를 선택하여 자세히 알아보십시오.

>[!BEGINTABS]

>[!TAB 수동]

## 수동으로 제품 만들기 {#manual-import}

카탈로그가 제한되어 있고 업데이트를 자주 사용하지 않는 경우 수동으로 만드는 것이 가장 좋습니다. 각 제품을 입력하는 데 시간이 필요하고 Commerce 관리자를 사용하는 방법에 대한 일부 제한된 교육도 필요합니다. 수동 카탈로그 관리는 대부분의 상점에 대한 적절한 옵션이 아니지만, 특정 상황에서, 그것은 이치에 맞을 수 있습니다. 이 프로세스에 대한 추가 설명서를 보려면 다음을 방문하십시오. [제품 만들기](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. Do not forget, you can use more than one method to manage your catalog, however once automation is used, manual edits must be limited. Automated updates have the opportunity to overwrite any changes performed manually, and therefore cause confusion. Once the integration with Adobe Commerce to manage the catalog is using automation and APIs, it is advised to restrict management of the catalog from the admin through [user roles and permissions](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.



### 이 접근 방식을 고려해야 하는 경우

- 매우 작은 카탈로그(예: 50개 미만의 제품)
- 업데이트가 자주 수행되지 않습니다.
- 모든 제품 세부 사항, 이미지, 비디오가 있으며 데이터를 CSV로 변환하는 방법에 대해 배우는 데 시간을 할애하고 싶지 않을 것입니다
- 제품을 만들 때 이미지 및 비디오 추가를 포함하려는 경우
- 내 팀 `not` api 및 OAUTH 작동 방식에 익숙함



>[!TAB 관리자 CSV]

## 관리 CSV 가져오기 도구 {#admin-csv}

이 도구를 사용하면 스토어 소유자가 상거래 관리자의 CSV 권한을 사용하여 카탈로그를 가져올 수 있습니다.
[상거래 관리에서 데이터 가져오기](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

장점 : 관리자로부터 CSV를 업로드하는 것은 카탈로그 관리에 대한 직접적인 접근 방식입니다. 이를 통해 중간 크기의 카탈로그에 대한 카탈로그 제품 업데이트를 더 빠르게 수행할 수 있습니다.

단점:

- 느림
- 서버에서 정의된 최대 업로드 파일 크기이며 스토어 소유자가 쉽게 조정할 수 없습니다.
- 작업을 수행하기 위해 관리자 액세스 권한 및 누군가가 필요합니다. 자동화는 제한되어 있습니다.
- 예약 가져오기는 하루에 최대 1회로 제한됩니다.
- 연결된 이미지와 비디오를 별도로 업로드해야 합니다



### 이 접근 방식을 고려해야 하는 경우

- 카탈로그 크기가 보통입니다.
- 업데이트는 하루에 두 번 이상 수행되지 않습니다.
- 최대 파일 업로드 크기를 늘려야 하는 경우 서버 구성에 액세스할 수 있습니다.
- 내 팀 `not` api 및 OAUTH 작동 방식에 익숙함



>[!TAB 벌크 REST API]

## 벌크 REST API {#bulk-rest-api}

Bulk REST API를 사용하면 자동화와 더 빈번한 업데이트를 수행할 수 있습니다. 이 API는 CSV의 관리자 업로드를 사용하는 것보다 빠릅니다.
[벌크 끝점 설명서](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

장점 : CSV 형식이 아닌 큰 데이터 세트를 가져오는 기능입니다.

단점:

- 연결된 이미지와 비디오를 별도로 업로드해야 합니다
- 호스팅 공급자의 대역폭 제한에 의해 제한될 수 있습니다.
- 레이블이 아닌 옵션 속성 ID를 사용해야 합니다



### 이 접근 방식을 고려해야 하는 경우

- 카탈로그가 모든 크기입니다.
- 업데이트는 빈번하며 하루에 1회 이상 허용됩니다.
- 가져올 시간은 중요하지만 중요하지 않음
- 데이터는 CSV 형식으로 구조화되지 않으며 자동화를 사용하여 변환할 수 없습니다



>[!TAB 비동기 REST API]

## 비동기 REST API {#async-rest-api}

비동기 웹 끝점은 웹 API에 대한 메시지를 가로채서 메시지 큐에 씁니다. 시스템이 이러한 API 요청을 수락할 때마다 UUID 식별자를 생성합니다. Adobe Commerce은 대기열에 메시지를 추가할 때 이 UUID를 포함합니다. 그러면 소비자는 대기열에서 메시지를 읽고 하나씩 실행합니다.
[비동기 웹 엔드포인트 설명서](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

장점:

- 데이터를 빠르게 가져오기
- 저장소 범위가 지원되거나 사용자가 지정할 수 있습니다. `all` 기존의 모든 저장소에서 작업을 수행하려면

단점:

- GET 요청이 지원되지 않음
- 레이블 대신 옵션 속성 ID를 사용해야 합니다


### 이 접근 방식을 고려해야 하는 경우

- 수입이 빈번하다
- API를 통해 제출한 후 메시지 큐에서 처리되는 시간으로부터 약간의 지연은 문제가 없습니다.



>[!TAB CSV REST API]

## CSV REST API {#csv-rest-api}

이 API 옵션을 사용하면 다른 모든 기본 옵션과 비교하여 매우 빠른 가져오기를 수행할 수 있습니다.

[데이터 REST CSV API 가져오기](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
장점:

- 들어오는 데이터를 처리하는 가장 빠른 방법
- 하루에 여러 번 수행 가능
- HTTP 요청 크기 제한을 방지하기 위해 큰 요청에 gzip을 사용하여 데이터를 압축할 수 있습니다.

단점:

- 연결된 이미지와 비디오를 별도로 업로드해야 합니다
- 레이블이 아닌 옵션 속성 ID를 사용해야 합니다
- 데이터는 CSV 형식이어야 합니다.

### 이 접근 방식을 고려해야 하는 경우

- 카탈로그가 모든 크기입니다.
- 업데이트는 빈번하며 하루에 1회 이상 허용됩니다.
- 가져올 전체 시간이 중요합니다
- 데이터는 이미 CSV 형식이거나 자동화를 사용하여 쉽게 변환할 수 있습니다



>[!ENDTABS]

## 추가 리소스

- [새 REST CSV를 사용하여 데이터 가져오기](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [데이터 주 설명서 가져오기](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Adobe Commerce 버전 2.4.6 릴리스 노트](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [사용자, 역할 및 권한](../site-management/users-roles-permissions.md){target="_blank"}
