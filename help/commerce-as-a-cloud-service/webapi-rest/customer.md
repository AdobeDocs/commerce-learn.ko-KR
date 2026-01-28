---
title: 새로운 고객 REST API 살펴보기
description: Adobe Commerce Cloud Service에서 새로운 고객 REST API를 사용하는 방법에 대해 알아봅니다. 설계자 및 개발자에게 이상적입니다.
feature: REST, Customers, Saas
topic: Development, Integrations
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 225
last-substantial-update: 2026-01-27T00:00:00Z
jira: KT-20160
source-git-commit: cb3fecf5f8b23425311dc31ed526b3b9bfe07b45
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 0%

---


# 고객 REST API

Adobe Commerce as a Cloud Service에서 새로운 고객 REST API를 사용하는 방법을 알아봅니다. 이 자습서는 API 솔루션을 효과적으로 통합 및 최적화하려는 설계자 및 개발자에게 적합합니다.

## 이 비디오는 누구의 것입니까?

* Adobe Commerce과의 통합 구축을 담당하는 백엔드 개발자
* Headless 상거래 구현을 위한 고객 관리 워크플로를 디자인하는 기술 설계자

## 비디오 콘텐츠

* 서버 간 자격 증명을 사용하여 Adobe IMS로 인증하여 API 요청에 대한 액세스 토큰을 얻습니다
* Commerce as a Cloud Service에 올바른 REST API 끝점 형식 사용
* 적절한 JSON 페이로드와 함께 POST 및 PUT 요청을 사용하여 프로그래밍 방식으로 고객 계정을 만들고 업데이트합니다

>[!VIDEO](https://video.tv.adobe.com/v/3479361/?learn=on&enablevpops)

## 코드 샘플

시작하기 전에 [Experience Cloud](https://experience.adobe.com) 및 [Adobe Developer Console](https://developer.adobe.com/console)에서 필요한 모든 값을 수집합니다. 이러한 값을 준비하면 원활한 설정 프로세스가 보장됩니다.

>[!NOTE]
>
>올바른 조직에서 작업하고 있는지 확인하십시오. 조직 선택 사항은 Experience Cloud 및 Developer Console 모두에서 볼 수 있는 인스턴스 및 환경에 영향을 줍니다.

### 인스턴스 세부 사항 - experience.adobe.com

인스턴스 세부 사항에는 인스턴스 ID, GraphQL 끝점, 자격 증명과 같은 것이 포함됩니다.

### 개발자 세부 사항 - https://developer.adobe.com/console/

Developer Console에서 클라이언트 ID, 클라이언트 암호 및 액세스 토큰을 포함한 API 자격 증명을 관리할 수 있습니다. 서버 간 인증 또는 네이티브 앱 인증과 같은 새 자격 증명 유형을 만들 수도 있습니다.

## 사전 요구 사항

| 항목 | 값 | 이 값은 어디에 있습니까 |
|--- |--- |--- |
| 인스턴스 ID | `<instance_id>` | experience.adobe.com |
| REST 끝점 | `<rest_endpoint>` | experience.adobe.com |
| 클라이언트 ID | `<client_id>` | developer.adobe.com/console |
| 클라이언트 암호 | `<client_secret>` | developer.adobe.com/console |


## 1단계: 액세스 토큰(서버 간 인증) 가져오기

>[!IMPORTANT]
>
> 이 샘플에 표시된 변수가 잘못되었습니다. 프로젝트 자격 증명에서 &lt;client_id> 및 &lt;client_secret>을 사용합니다.

```bash
curl -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles'
```

**샘플 응답:**

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "token_type": "bearer",
  "expires_in": 86399
}
```

## 2단계: 고객 만들기

>[!IMPORTANT]
>
> 이 샘플에 제공된 URL이 잘못되었습니다. REST 기본 URL을 사용합니다. URL로 &#39;&lt;rest_endpoint>&#39;를 교환합니다. 이 `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`과(와) 비슷합니다.
>
> 이 끝점에는 URL의 일부로 /rest/ 가 없습니다. 이를 포함하면 오류가 발생합니다.

**끝점:** `POST /V1/customers`

```bash
curl -X POST \
  "<rest_endpoint>/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }'
```

**응답:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:15",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "store_id": 1,
  "website_id": 1,
  "addresses": [],
  "disable_auto_group_change": 0
}
```

## 3단계: 고객 업데이트

>[!IMPORTANT]
>
> 이 샘플에 제공된 URL이 잘못되었습니다. REST 기본 URL을 사용합니다. URL로 &#39;&lt;rest_endpoint>&#39;를 교환합니다. 이 `https://na1-sandbox.api.commerce.adobe.com/AbCYab34cdEfGHiJ27123`과(와) 비슷합니다.

다음 예제의 `5` 숫자는 POST `"id": 5,`을(를) 사용하여 이전에 만든 고객의 ID입니다. `5`을(를) 요청에서 반환된 ID로 변경하십시오.

**끝점:** `PUT /V1/customers/{customerId}`

```bash
curl -X PUT \
  "<rest_endpoint>/V1/customers/5" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <ACCESS_TOKEN>" \
  -d '{
    "customer": {
      "id": 5,
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe-Updated"
    }
  }'
```

**응답:**

```json
{
  "id": 5,
  "group_id": 1,
  "created_at": "2026-01-23 20:40:15",
  "updated_at": "2026-01-23 20:40:30",
  "created_in": "Default Store View",
  "email": "john.doe@example.com",
  "firstname": "John",
  "lastname": "Doe-Updated",
  "store_id": 1,
  "website_id": 1,
  "addresses": []
}
```

## 전체 스크립트(올인원)

>[!IMPORTANT]
>
> 이 샘플에 표시된 변수가 잘못되었습니다. 프로젝트 자격 증명에서 클라이언트 ID 및 클라이언트 암호를 사용합니다. REST 기본 URL을 사용합니다. experience.adobe.com에서 REST 끝점 URL로 &#39;&lt;rest_endpoint>&#39;를 교환합니다. 이 `https://na1-sandbox.api.commerce.adobe.com/AbCDefGHiJ1234567`과(와) 비슷합니다.

```bash
#!/bin/bash

# Configuration be sure to update these with your projects unique values
CLIENT_ID="<client_id>"
CLIENT_SECRET="<client_secret>"
REST_ENDPOINT="<rest_endpoint>"

# Step 1: Get Access Token
echo "Getting access token..."
ACCESS_TOKEN=$(curl -s -X POST 'https://ims-na1.adobelogin.com/ims/token/v3' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}&scope=openid,AdobeID,email,additional_info.projectedProductContext,profile,commerce.aco.ingestion,commerce.accs,org.read,additional_info.roles" | jq -r '.access_token')

echo "Token obtained: ${ACCESS_TOKEN:0:50}..."

# Step 2: Create Customer
echo ""
echo "Creating customer..."
CREATE_RESPONSE=$(curl -s -X POST \
  "${REST_ENDPOINT}/V1/customers" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d '{
    "customer": {
      "email": "john.doe@example.com",
      "firstname": "John",
      "lastname": "Doe",
      "store_id": 1,
      "website_id": 1
    },
    "password": "TempPa55word!"
  }')

echo "Create Response:"
echo "$CREATE_RESPONSE" | jq .

# Extract customer ID
CUSTOMER_ID=$(echo "$CREATE_RESPONSE" | jq -r '.id')
echo "Customer ID: $CUSTOMER_ID"

# Step 3: Update Customer
echo ""
echo "Updating customer..."
curl -s -X PUT \
  "${REST_ENDPOINT}/V1/customers/${CUSTOMER_ID}" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer ${ACCESS_TOKEN}" \
  -d "{
    \"customer\": {
      \"id\": ${CUSTOMER_ID},
      \"email\": \"john.doe@example.com\",
      \"firstname\": \"john\",
      \"lastname\": \"Doe-Updated\"
    }
  }" | jq .
```

## 이 자습서에 대한 중요 참고 사항

1. **URL 경로**: `https://<server>.api.commerce.adobe.com/<tenant-id>/V1/customers` 사용 — **NOT** `https://<host>/rest/<store-view-code>/V1/customers`
1. **인증**: 이 자습서에서는 서버 간(`client_credentials` 부여 유형)을 사용했습니다.
1. **필수 범위**: `commerce.accs`
1. **토큰 만료**: 86400초(24시간)

## 참조

* [Adobe Commerce as a Cloud Service 릴리스 노트](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/release-notes)
* [SaaS REST API 참조](https://developer.adobe.com/commerce/webapi/reference/rest/saas/)
* [사용자 인증 안내서](https://developer.adobe.com/commerce/webapi/rest/authentication/user/)
