---
title: 분할 결제 POC — 튜토리얼 빠른 참조
description: 분할 결제 POC 파일 맵에 대해 알아봅니다. Luma 체크아웃을 위해 각 AI 프롬프트, 제안된 섹션 순서 및 작성자 메모와 일치하는 Experience League 페이지
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1444'
ht-degree: 0%

---

# 분할 결제 POC: 튜토리얼 빠른 참조

이 페이지에서는 분할 결제 개념 증명 자습서 시리즈를 구성하는 방법을 요약합니다. 번호가 매겨진 각 소스 프롬프트 파일을 게시된 Experience League 페이지에 매핑하고, 독자를 위한 권장 섹션 순서를 나열하며, 작성자 및 유지 관리자를 위한 메모를 수집합니다.


## 파일별 참조

### [분할 결제 POC 만들기: App Builder 및 AI 도구](./overview.md)

자습서에 대한 **목적:** 소개 및 방향.

**필요한 이유:** 개발자에게 &quot;방법&quot; 앞에 &quot;이유&quot;를 제공합니다. 사이트가 깔끔한 Luma 설치와 다를 경우 고객이 빌드할 내용, 대상이 누구이며 비판적으로 무엇을 사용자 정의해야 하는지 설명합니다. 또한 시리즈의 나머지 부분에 대한 목차 역할을 합니다.

**튜토리얼 사용:** 섹션을 엽니다. 기술 단계 전에 컨텍스트를 설정합니다.


### [분할 결제 POC: 아키텍처 및 디자인 결정](./architecture-and-decisions.md)


**목적:** PoC의 모든 아키텍처 결정에 대한 자세한 설명.

**필요한 이유:** 이 PoC를 사용하여 작업할 때 혼동을 일으키는 가장 일반적인 점은 PHP와 App Builder에 있는 이유 *이유*&#x200B;를 이해하는 것입니다. 이러한 컨텍스트가 없으면 개발자는 이동할 수 없는 항목(예: 스토어 크레딧 애플리케이션)을 이동하려고 시도하지만 실패합니다. 이 문서는 명확한 추론을 통해 이러한 실패를 사전에 예방합니다.

**주요 주제 설명:**

* &quot;PHP에서 유지되어야 하는 것&quot; 규칙과 이유
* 임계값 이중 적용 패턴
* 스토어 신용 응용 프로그램을 비동기 처리할 수 없는 이유
* 플러그인이 처리하는 5가지 Commerce Edge 사례
* 확장 속성 데이터 흐름 체크아웃 → 견적 → 주문 → I/O 이벤트 → App Builder

**튜토리얼 사용:** &quot;아키텍처&quot; 또는 &quot;작동 방법&quot; 섹션. 숙련된 Commerce 개발자는 건너뛸 수 있지만 App Builder 초보자에게는 필수적입니다.


### [분할 결제 POC: 사전 요구 사항 및 환경 설정](./prerequisites-and-setup.md)


**목적:** 코드를 작성하거나 프롬프트를 실행하기 전에 사전 검사 목록을 완료하십시오.

**필요한 이유:** 자습서에서 설치 단계의 실패율이 가장 높습니다. 이 문서는 Commerce 관리자가 올바로 구성되었는지(정확한 COD 제목, 스토어 크레딧 활성화, 테스트 고객의 잔액, 통합 생성됨), App Builder 프로젝트에 I/O 이벤트 및 Commerce 이벤트 공급자가 연결되었는지, 로컬 환경의 버전이 올바른지 확인합니다.

**강조 항목:**

* 게재 제목의 캐시는 정확히 `Cash`이어야 합니다(중요 — JavaScript은 문자열 일치).
* OAuth 자격 증명이 작동하려면 Commerce 통합이 *활성화*(단순 저장이 아님)되어야 합니다.
* `entity_id`과(와) `increment_id`은(는) 혼동을 방지하기 위해 여기에 설명되어 있습니다.

**자습서 사용:** &quot;필수 구성 요소&quot; 또는 &quot;시작&quot; 섹션. 읽기만 하는 것이 아니라 대화식으로 완성해야 합니다.


### [분할 결제 POC: 환경 변수 참조](./env-reference.md)


**목적:** 세 구성 요소 모두에 대한 모든 환경 변수를 한 곳에 배치합니다.

**필요한 이유:** 변수 이름이 다른 세 개의 다른 파일에 동일한 4개의 OAuth 자격 증명이 표시됩니다(`COMMERCE_*` 대 `COMMERCE_INTEGRATION_*`). 단일 참조가 있으면 하나의 자격 증명 집합에 오타가 있는 &quot;이 기능이 작동하지 않는 이유&quot; 디버깅을 수행할 수 없습니다.

**포함된 구성 요소:**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env`(IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**자습서 사용:** 참조 사이드바 또는 &quot;구성&quot; 섹션. 빌드 프롬프트의 동반자로도 사용됩니다.


### [분할 결제 POC: Commerce 모듈 AI 프롬프트](./commerce-module-prompt.md)


**목적:** 전체 `Client_SplitPayment` PHP 모듈을 생성하기 위한 자체 포함된 AI 프롬프트를 완료합니다.

**필요한 이유:** PHP 측의 기본 AI 빌드 아티팩트입니다. PHP 경험이 없는 개발자가 Cursor에(Claude 사용)로 전달하고 작업 모듈을 가져올 수 있도록 작성되었습니다. 모든 파일이 지정되고, 모든 동작 계약이 정의되며, 중요한 구현 메모가 포함되며, 이후에 모듈을 활성화하는 명령이 제공됩니다.

**적용 범위:**

* 전체 파일 구조(30개 이상의 파일)
* 데이터베이스 스키마, 확장 속성, REST 끝점, I/O 이벤트 구성
* 완전한 동작 사양(서명 뿐만 아니라)이 있는 모든 PHP 클래스
* 녹아웃JS 체크아웃 구성 요소 사양
* 각각 존재하는 이유에 대한 설명이 포함된 5개의 에지 사례 플러그인
* 비 Luma 테마에 대한 사용자 지정 설명선

**자습서 사용:** &quot;Commerce 모듈 빌드&quot; 섹션. 프롬프트 자체가 아티팩트입니다. 개발자는 아티팩트를 AI 도구에 복사하고 실행합니다.


### [분할 결제 POC: App Builder orchestrator AI 프롬프트](./orchestrator-prompt.md)


**목적:** `split-payment-orchestrator` App Builder 응용 프로그램을 생성하기 위한 자체 포함된 AI 프롬프트를 완료합니다.

**필요한 이유:** 4가지 App Builder 작업(`payment-orchestrator`, `payment-accept`, `payment-decline`, `demo-dashboard`)은 &quot;PHP에서 이동된 항목&quot; 스토리의 핵심입니다. 이 프롬프트는 전체 동작 사양, 올바른 `app.config.yaml` 구조 및 이벤트 등록 구성을 사용하여 모든 동작을 생성합니다.

**적용 범위:**

* `app.config.yaml`(네 가지 작업 모두 포함) 및 I/O 이벤트 등록
* `commerce-client.js` OAuth 1.0a 공유 클라이언트
* 완전한 논리 사양이 있는 네 가지 작업
* `store-credit.js`의 사용되지 않는 스텁(*사용되지 않는 이유*&#x200B;에 대한 설명 포함, 교육학적으로 중요)
* HTML 렌더링, 순서 필터링 및 보안이 포함된 데모 대시보드
* 모든 변수가 있는 `.env.example`

**자습서 사용:** &quot;App Builder 애플리케이션 빌드&quot; 섹션. Commerce 모듈 프롬프트에 대한 설명입니다.


### [분할 결제 POC: Experience Cloud UI 확장 AI 프롬프트](./experience-cloud-ui-prompt.md)


**목적:** 선택적 Experience Cloud 관리 UI SDK 확장을 생성하도록 AI 메시지를 표시합니다.

**필요한 이유:** Orchestrator 프롬프트에서 데모 대시보드는 의도적으로 거칠어졌습니다. 이는 프로덕션이 아니라 개념 증명입니다. 이 섹션에서는 개발자에게 다음 단계인 관리 UI SDK을 사용하여 분할 결제 대시보드를 Commerce 관리 셸에 포함하는 방법을 보여줍니다. 원래 프롬프트에서 완전히 누락되었습니다.

**적용 범위:**

* 관리자 UI SDK 확장에 대한 `ext.config.yaml`
* 주문 대시보드 및 주문 세부 사항에 대한 React 구성 요소
* 목록에 IMS 인증을 사용하고 수락/거부에 OAuth를 사용하는 백엔드 작업
* 시뮬레이션 스크립트(테스트에도 사용)

**튜토리얼 사용:** 선택적 &quot;계속 진행&quot; 또는 &quot;프로덕션 경로&quot; 섹션. 자습서가 PoC에만 중점을 두는 경우 건너뛸 수 있습니다.


### [분할 결제 POC: 테스트 및 확인 가이드](./testing-and-verification.md)


**목적:** 올바른 확인 순서에 따라 모든 구성 요소를 다루는 단계별 테스트 안내서입니다.

**필요한 이유:** 구성 요소를 빌드하는 것이 작업의 절반입니다. 개발자는 가장 낮은 수준(데이터베이스 열, REST 끝점)에서 시작하여 전체 종단 간 흐름으로 빌드되는 명확한 확인 경로가 필요합니다. 원래 프롬프트에 설정 체크리스트가 있지만 명시적인 테스트 흐름은 없었습니다.

**적용 범위:**

* Commerce 모듈 설치 확인
* 관리자 구성 확인
* REST 끝점 연기 테스트(curl 명령)
* 체크아웃 UI 안내(유효성 검사 사례 포함)
* 임계값 보호 테스트
* 완전 주문 배치 테스트
* 수락 및 거절을 위한 시뮬레이션 스크립트 사용
* 데모 대시보드 사용
* App Builder 작업 로그 검사
* 특정 수정 사항이 있는 10가지 일반적인 장애 모드

**자습서 사용:** &quot;테스트&quot; 또는 &quot;확인&quot; 섹션. 문제 해결 참조 자료로도 유용합니다.


### [분할 결제 POC: 개념 증명 후 다음 단계](./next-steps.md)


**목적:** PoC를 프로덕션 준비 패턴으로 발전시키기 위한 로드맵.

**필요한 이유:** PoC 자습서가 개발자에게 &quot;지금 무엇&quot;을 남길 위험이 있습니다. 느낌. 이 문서는 데모 대시보드를 Experience Cloud 확장으로 교체, 실제 ERP 연결, API Mesh 추가, App Builder 워크플로우 확장 및 프로덕션 배포 체크리스트 등 데모에서 프로덕션으로 이어지는 자연스런 진행 상황을 매핑합니다.

**키 콘텐츠:**

* 아키텍처 진화 다이어그램(PoC → 2단계 → 3단계)
* ERP 통합 패턴(변경 사항, 동일하게 유지)
* API Mesh 브로커 패턴
* 프로덕션 배포 검사 목록
* 키 참조 링크

**자습서 사용:** 섹션을 닫는 &quot;다음 단계&quot; 또한 실제 프로젝트의 범위를 지정할 때 대화 시작점으로도 유용합니다.


## 추천 튜토리얼 섹션

이러한 파일을 기반으로 하는 독자의 일반적인 구조는 다음과 같습니다.

| 튜토리얼 섹션 | Experience League 페이지 |
| --- | --- |
| 소개 및 학습 목표 | [분할 결제 POC 만들기: App Builder 및 AI 도구](./overview.md) |
| 엔드 투 엔드 데모(비디오) | [분할 결제 POC 만들기: App Builder 전체 데모](./full-demo.md) |
| 아키텍처: 어디에 사는가 | [분할 결제 POC: 아키텍처 및 디자인 결정](./architecture-and-decisions.md) |
| 사전 요구 사항 및 설정 | [분할 결제 POC: 사전 요구 사항 및 환경 설정](./prerequisites-and-setup.md) |
| 환경 변수 | [분할 결제 POC: 환경 변수 참조](./env-reference.md) |
| 1단계: Commerce 모듈 구축 | [분할 결제 POC: Commerce 모듈 AI 프롬프트](./commerce-module-prompt.md) |
| 2단계: App Builder Orchestrator 구축 | [분할 결제 POC: App Builder orchestrator AI 프롬프트](./orchestrator-prompt.md) |
| 3단계: 엔드 투 엔드 플로우 테스트 | [분할 결제 POC: 테스트 및 확인 가이드](./testing-and-verification.md) |
| 4단계(선택 사항): 관리자 UI 확장 | [분할 결제 POC: Experience Cloud UI 확장 AI 프롬프트](./experience-cloud-ui-prompt.md) |
| 다음 단계 및 프로덕션 경로 | [분할 결제 POC: 개념 증명 후 다음 단계](./next-steps.md) |


## 튜토리얼 작성자를 위한 중요 참고 사항

**Luma 테마 가정에서:** 모든 빌드 프롬프트는 깨끗한 Luma 설치를 위한 코드를 생성합니다. 튜토리얼의 맨 위에는 이 내용이 명확하게 설명되어 있어야 합니다. 사용자 지정 체크 아웃을 사용하는 개발자는 `LayoutProcessorPlugin` 삽입 경로 및 RequireJS 구성을 조정해야 합니다. 시리즈 소개 및 빌드 프롬프트에 이에 대한 사용자 지정 콜아웃이 포함됩니다.

**PHP 모듈에서:** 자습서에서는 PHP 코드를 복사-붙여넣기 다운로드로 제공하지 않습니다. AI 프롬프트 *생성*&#x200B;합니다. 이것은 의도적인 것입니다. AI를 사용하여 보일러판을 복사하여 붙여넣는 것이 아니라 Commerce 확장을 만드는 패턴을 설명합니다. 그러나 깨끗한 환경에서 메시지가 표시될 때 생성된 코드는 `app/code/Client/SplitPayment/`의 작업 코드와 정확히 일치해야 합니다.

**$100 임계값:**&#x200B;이 값은 하드 코딩된 PoC 값입니다. 자습서에서는 실제 저장소가 컴파일 시간 상수가 아닌 Commerce 관리 구성을 통해 이 작업을 구성하려고 한다는 점을 참고하십시오.

**스토어 크레딧 종속성:** 빌드된 분할 결제 흐름에는 `Magento_CustomerBalance`(기본 스토어 크레딧 모듈)이 필요합니다.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
