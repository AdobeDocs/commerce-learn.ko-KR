---
title: 분할 결제 POC - 개념 증명 후 다음 단계
description: 분할 결제 POC를 프로덕션으로 이동하는 방법을 알아봅니다. Experience Cloud UI, ERP 후크, API Mesh, PHP 범위, App Builder 워크플로 및 배포 확인 목록.
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# 분할 결제 POC: 개념 증명 후 다음 단계

이 자습서에서 빌드한 데모 대시보드 및 시뮬레이션 스크립트는 의도적으로 거칠게 되었습니다. 그들은 패턴을 증명하기 위해 존재한다. 이 페이지에서는 개념 증명에서 프로덕션 스타일 App Builder 개발까지의 실제 경로에 대해 설명합니다.


## 1단계 — 데모 대시보드를 Experience Cloud UI 확장으로 바꾸기

`demo-dashboard` 웹 작업은 Node.js 함수 내의 문자열에서 HTML을 제공합니다. 작동은 하지만 생산 패턴은 아닙니다.

적절한 대체 요소는 `commerce-checkout-starter-kit`의 `commerce-backend-ui-1` 확장(Adobe Admin UI SDK을 통해 Commerce Admin Shell에 포함된 React 애플리케이션)입니다. 생성 프롬프트는 [분할 결제 POC: Experience Cloud UI 확장 AI 프롬프트](./experience-cloud-ui-prompt.md)를 참조하십시오.

**변경 내용:**
* 운영자는 Commerce 관리 셸(공유 암호 대신 IMS 인증)을 통해 로그인합니다
* 주문 목록은 IMS 토큰 컨텍스트를 사용합니다. 데모 암호는 필요하지 않습니다.
* 수락/거부 작업의 범위가 운영자의 IMS ID로 지정됩니다.
* UI는 Commerce 관리자가 이미 알고 있는 URL의 Commerce 관리자에 임베드됩니다

**동일하게 유지되는 항목:**
* `payment-accept` 및 `payment-decline` App Builder 작업이 변경되지 않았습니다.
* Commerce REST 엔드포인트는 변경되지 않음
* PHP 모듈이 변경되지 않았습니다.


## 2단계 — 실제 ERP 연결

이 PoC의 수락/거절 흐름은 사용자가 버튼을 클릭하여 구동됩니다. 프로덕션 환경에서 ERP, CRM 또는 결제 프로세서에 의해 구동됩니다.

**통합 패턴:**
1. ERP 시스템에서 현금을 캡처하고 `{ orderId: <entity_id> }`(으)로 `POST /payment-accept`(App Builder 웹 작업 URL)을 호출합니다.
2. App Builder이 호출의 유효성을 검사합니다(전달자 토큰 또는 API 키 — `payment-accept`에 인증 미들웨어 추가).
3. App Builder에서 Commerce REST `cash-received`을(를) 호출함
4. Commerce 송장 및 주문 출하

PHP 변경이 필요하지 않습니다. REST면이 이미 있습니다 App Builder 작업에는 대시보드 단추 대신 보안 호출자가 필요합니다.

**ERP 호출자에 대한 인증 옵션:**
* Adobe I/O Runtime은 IMS 토큰에 대해 `require-adobe-auth: true`을(를) 지원합니다(ERP가 IMS 토큰을 가져올 수 있는 경우).
* Adobe이 아닌 시스템의 경우: `payment-accept` 작업에 간단한 API 키 검사를 추가합니다(작업의 환경에 저장된 암호에 대해 헤더 확인).


## 3단계 — API 메쉬를 브로커 계층으로 추가

현재 App Builder은 OAuth 1.0a 자격 증명으로 Commerce REST를 직접 호출합니다. 대규모 배포의 경우 Adobe API Mesh는 App Builder과 Commerce 간에 관리되는 게이트웨이 계층을 제공합니다.

**이점:**
* 중앙 집중식 자격 증명 관리 — API Mesh가 Commerce 자격 증명을 보유하며, App Builder이 API Mesh를 호출합니다.
* 변환 요청 — App Builder 작업을 변경하지 않고 App Builder 페이로드를 Commerce REST 셰이프에 매핑합니다.
* 속도 제한 및 캐싱 — 대용량 이벤트 트래픽으로부터 Commerce 보호

**패턴:**
* `createCommerceClient(params, logger)` 호출을 API Mesh 끝점에 대한 호출로 바꾸기
* API Mesh가 Commerce을 향한 OAuth 서명을 처리합니다.


## 4단계 — PHP Footprint 감소

현재 PHP 모듈은 진행 중이어야 하는 5가지 사항을 처리합니다([분할 결제 POC: 아키텍처 및 디자인 결정](./architecture-and-decisions.md) 참조). Adobe Commerce의 API 표면이 발달함에 따라 이러한 API 중 일부는 이동할 수 있게 될 수 있습니다.

**향후 이동 가능:**
* 스토어 크레딧 REST API가 발전하고 있습니다. 향후 버전은 신용 사후 주문 또는 비활성 장바구니에 적용을 지원할 수 있습니다.
* 더 많은 Commerce 작업들이 비동기-안전해짐에 따라, 임계 가드는 프리-오더 API 메쉬 레졸버로 이동될 수 있다

**이동할 수 없음(아키텍처 제한):**
* Commerce이 API 우선 확장 지점을 통해 깔끔한 후크를 노출하지 않는 한 `placeOrder()` 이전의 따옴표 조작에는 항상 처리 중인 코드가 필요합니다.
* REST 끝점(`/V1/split-payment/*`)은 이 기능에 한정되며, Commerce 내부 서비스를 호출하므로 Commerce에 있습니다


## 5단계 — App Builder에 워크플로우 추가

현재 PoC는 주문 배치를 수신한 다음 수락/거부를 활성화하는 한 가지 작업을 수행합니다. 자연어 확장:

**수락하기 전 부정 채점:**
`payment-orchestrator`에서 보류 중인 현금을 기록한 후 오케스트레이션 결과가 최종으로 간주되기 전에 부정 채점 API를 호출합니다. 점수가 임계값을 초과하는 경우 연산자 작업을 기다리지 않고 자동 거부를 수행합니다.

**알림 전자 메일:**
`payment-accept`이(가) 성공하면 현금 결제가 확인되었음을 고객에게 알리는 이메일(Adobe Campaign, SendGrid 또는 HTTPS API를 통해)을 트리거합니다.

**충성도 포인트 상:**
현금이 확인되면 충성도 API를 호출하여 포인트를 부여합니다. 이것은 순수 App Builder — PHP가 필요하지 않습니다.

**시간 제한 처리:**
`split_cash_status = 'pending'`이(가) X일보다 오래된 주문을 검색하고 자동으로 거부하는 예약된 App Builder 작업(`app.config.yaml`에서 `cron` 사용)을 추가합니다.


## 6단계 — 프로덕션에 배포

`Stage` 작업 영역에 대해 PoC가 구성되었습니다. 프로덕션으로 이동:

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**프로덕션 준비 확인 목록:**
* [ ] `DEMO_UI_SECRET` 집합(또는 데모 대시보드가 Experience Cloud UI로 대체됨)
* [ 프로덕션의 ] `LOG_LEVEL=warn` 또는 `error`(`debug` 아님)
* [ ] `PAYMENT_THRESHOLD`이(가) Commerce 프로덕션 구성과 일치함
* [ `.env`의 ] Commerce 통합 자격 증명은 전용 프로덕션 통합을 위한 것입니다(스테이징이 아님).
* [ App Builder 프로덕션 이그레스 IP(Commerce Cloud)로 업데이트된 ] Fastly IP 허용 목록에 추가하다
* [ 프로덕션 작업 영역에서 ] I/O 이벤트 등록이 확인되었습니다.


## 아키텍처 진화 다이어그램

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## 주요 참조

* [Adobe App Builder 설명서](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Commerce용 Adobe I/O Events](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Commerce 체크아웃 스타터 키트](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [Adobe 관리 UI SDK](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Adobe API 메쉬](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime(OpenWhsk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
