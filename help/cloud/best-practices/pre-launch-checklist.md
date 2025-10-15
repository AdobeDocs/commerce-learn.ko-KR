---
title: Adobe Commerce Cloud 사전 실행 검사 목록
description: Adobe Commerce Cloud 사전 실행 검사 목록에 대해 알아봅니다.
feature: Cloud
topic: Commerce, Architecture, Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 191cfb29de7b4fff5ca73dcd1603b51d852aebd1
workflow-type: tm+mt
source-wordcount: '1605'
ht-degree: 0%

---

# Commerce Cloud 실행 전 체크리스트

다음은 Adobe Commerce [사이트 시작 설명서](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}의 개요입니다.

이 체크리스트는 Adobe Commerce Cloud 사이트의 성공적인 시작을 계획하고 실행하는 데 도움이 되는 것을 목적으로 합니다. Adobe Commerce Cloud용 시스템 통합자와 협력하여 모든 구성 작업 및 체크리스트 항목이 완료되고 확인되도록 하십시오. 체크리스트 항목에 대해 문제가 발생하거나 질문이 있는 경우 지정된 고객 기술 자문 또는 고객 성공 엔지니어에게 문의하십시오. 계정에 할당된 CTA/CSE가 없는 경우 지원 티켓을 만들 수 있습니다.

계정에 할당된 CTA/CSE가 있는 경우 새 Adobe Commerce Cloud 사이트를 시작하기 최소 4주 전에 해당 계정과 계정 관리자에게 문의하여 **의도**&#x200B;를 알리십시오.

- 일부 검사는 [!BADGE Blocker] (으)로 강조 표시됩니다.{type=caution tooltip="잠재적 차단제"}
- 구현 접근 방식에 맞게 개발자 또는 시스템 통합 파트너와 공동 작업해야 합니다.

>[!IMPORTANT]
> 이 체크리스트를 사용하고 완료하지 못한 경우 프로덕션 시작 일정 및 진행 중인 사이트 안정성에 대한 모든 악영향 및 관련 위험에 대해 [책임](https://experienceleague.adobe.com/ko/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}을(를) 승인합니다.

## 1. 라이브 전

1. 테스트 및 라이브에 대한 설명서 검토 [사이트 시작 설명서](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >필요한 모든 작업 항목을 통합하여 파트너 또는 시스템 통합자와 함께 포괄적인 _&quot;go-live 준비 계획&quot;_&#x200B;이(가) 완전히 준비되었는지 확인하십시오. 출시 전 체크리스트에서는 Adobe의 모범 사례를 강조하지만 _&#x200B;**자체 Go-Live 준비 계획의 필요성을 대체하지 않습니다**&#x200B;_.

2. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}[사용 안내서](https://experienceleague.adobe.com/ko/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. 최종 사용자/판매자가 백엔드 작업을 포함하여 UAT(사용자 승인 테스트)를 수행했습니다.
4. 시스템 통합자 팀은 스테이징 및 프로덕션에 대해 엔드 투 엔드 UAT를 수행했습니다. [Experience League 설명서](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}를 참조하세요.
5. 스테이징 및 프로덕션 환경에서 코드 배포 및 테스트를 확인합니다([자세한 내용](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. 생산 클러스터의 크기가 계약된 일일 기준선으로 영구적으로 증가했습니다. 자세한 내용은 할당된 CTA/CSE에 문의하거나 지원 티켓을 제출하십시오.

## 2. 현재 구성

1. Adobe Commerce 및 관련 패키지/서비스를 [최신 버전으로 업그레이드](https://experienceleague.adobe.com/ko/docs/commerce-operations/release/notes/overview){target="_blank"}
2. SI/파트너와 현재 구성 및 서비스를 검토하고 [모범 사례를 따르세요](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. MySQL/공유 파일 [디스크 사용량](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"} 검토

## 3. Fastly 구성

1. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}[전체 페이지 캐시](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} 또는 [GraphQL 캐싱](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). [빠른 설정 가이드](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}를 읽어 보세요.
2. 해당되는 경우 PWA/Headless 웹 사이트에서 GraphQL 쿼리에 대해 GET 방법을 사용합니다.

   >[!NOTE]
   > HTTP GET 작업과 함께 제출된 쿼리만 캐시할 수 있습니다(해당하는 경우). [POST 쿼리를 캐시할 수 없습니다](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Fastly 이미지 최적화가 활성화되어 있는지 확인합니다([Fastly 이미지 최적화 참조](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"}).
4. 올바른 실드 위치가 구성되었는지 확인합니다([캐시, 백 엔드 및 원본 실드를 구성](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. 웹 응용 프로그램 방화벽(**WAF**)이 작동 중입니다. ([차단된 요청 문제 해결](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}(있는 경우) 및 제한 사항 을 참조하십시오.)
6. 캐시 성능을 향상시키려면 관리 패널에서 Fastly [&quot;무시된 URL 매개 변수&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} 목록을 업데이트하십시오.

   >[!NOTE]
   > _관리 > 저장소 > 구성 > 시스템 > 전체 페이지 캐시 > Fastly 구성 > 고급 구성 > 무시된 URL 매개 변수(전역)_&#x200B;의 Fastly 구성에서는 캐시된 페이지를 검색할 때 Fastly가 무시해야 하는 쉼표로 구분된 매개 변수 목록을 찾을 수 있습니다. 이 목록을 수정한 후 VCL을 다시 업로드해야 합니다.

## 4. DNS 및 SSL

1. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}_(추가되거나 변경된 도메인에 대한 지원 티켓을 미리 제출)_
2. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}자세한 내용은 [이 문서](https://experienceleague.adobe.com/ko/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"}를 참조하세요.
3. Go-Live에 대해 DNS [TTL(Time to Live)](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} 값을 가능한 최소값으로 업데이트합니다.
4. Sendgrid SPF 및 DKIM 활성화

   >[!NOTE]
   > 각 도메인에 대한 SendGrid CNAME 레코드를 DNS 구성에 추가합니다. 보낸 사람 도메인 등을 변경하는 방법을 알아보려면 [SendGrid 전자 메일 서비스](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}를 읽어 보세요.

## 5. 데이터베이스 구성

Adobe Commerce Cloud은 MariaDB Galera 클러스터를 스테이징 및 프로덕션 환경의 데이터베이스로 사용합니다. Galera 클러스터는 성능과 확장성을 향상시키는 데 중요한 역할을 합니다. Galera 클러스터 복제의 최적 관행과 제한에 대한 통찰력을 얻으려면 다음 문서를 참조하십시오.

- [MySQL 구성 모범 사례](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Adobe Commerce에서 관리되는 경고: [MariaDB 경고](https://experienceleague.adobe.com/ko/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- [데이터베이스 구성](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}에 대한 모범 사례
- [Galera 클러스터 복제 및 흐름 제어에 대한 심층 분석.](https://experienceleague.adobe.com/ko/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. 높은 데이터베이스 로드 중에 성능을 향상시키려면 [MYSQL 슬레이브 연결](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"}을 사용하는 것이 좋습니다.
2. 모든 데이터베이스 테이블의 행 형식이 COMPACT[&#128279;](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} 대신 DYNAMIC으로 설정되어 있는지 확인하십시오(온-프레미스 대 클라우드 마이그레이션에 특히 해당됨).
3. 모든 테이블에 대해 데이터베이스 저장소 엔진을 [MyISAM에서 InnoDB](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"}(으)로 변경하십시오.
4. 크기가 1GB를 초과하는 데이터베이스 테이블을 미리 검토하고 최적화합니다.
5. 데이터베이스 스키마 정보가 최신 상태입니다. ([이 안내서](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"} 참조).

## 6. 배포

1. 프로덕션 환경에 배포하는 동안 유지 관리 시간을 줄이기 위해 SCD(정적 콘텐츠 배포) 이상적인 상태를 검토하십시오. [정적 콘텐츠 배포(SCD) 전략](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} 및 [저장소 구성 관리](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"} 안내서를 검토하십시오.
2. HTML, JavaScript 및 CSS에 대한 축소 설정을 검토합니다. (PWA/Headless 웹 사이트에는 적용되지 않습니다.)
3. 다음 클라우드 변수의 활용도가 의도한 목적에 맞는지 확인합니다. ([SCD_MATRIX](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} 및 [SKIP_SCD](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## 7. 테스트 및 문제 해결

1. 보내는 트랜잭션 이메일을 테스트합니다. [Adobe Commerce Cloud - SendGrid 메일 기능](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}에 대해 자세히 알아보세요.
2. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}
3. [!BADGE 차단]{type=caution tooltip="잠재적 차단제"}

   >[!NOTE]
   > [부하 및 부하 분산 테스트는 응용 프로그램 내에서 병목 현상을 식별하고 성능 문제를 발견하는 목적](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"}을 수행합니다. 클러스터 규모에 대한 기대치를 관리하고 비즈니스 요구 사항을 효과적으로 충족하기 위해 필요한 규모 조정을 결정하는 중요한 역할을 합니다.

   >[!IMPORTANT]
   > **_경고:_** 부하 테스트를 준비할 때 **_실시간 트랜잭션 전자 메일을 더미 주소로도 보내지 마십시오_**. 테스트 중에 이메일을 보내면 프로젝트가 실행 전에 SendGrid에 대해 구성된 기본 전송 한도(12k)에 도달할 수 있습니다.
   > 
   > - 이메일 통신을 비활성화하는 방법:
   >   _스토어 > 구성 > 고급 > 시스템 > 전자 메일 전송 설정_(으)로 이동합니다.

4. [공유 권한 보안 모델](https://business.adobe.com/kr/products/magento/secure-ecommerce.html){target="_blank"}의 일부로 프로덕션 인스턴스에서 보안 침투 테스트를 수행합니다. PCI(Payment Card Industry) 규정 준수를 위해 맞춤화된 사이트에는 침투 테스트가 필요합니다.

## 8. 기타 구성

1. 인덱싱을 _&quot;일정에 따라 업데이트_&quot;로 전환합니다. 단, &quot;저장&quot;에 남아 있는 **_customer_grid_**&#x200B;은(는) 예외입니다([인덱싱 모드](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"} 참조).
2. 서드파티 검색 엔진 또는 확장을 사용 중입니까?
3. 인덱서/크롤러가 웹 사이트를 검색할 수 있도록 [SEO(검색 엔진 최적화) 구성이 올바르게 설정되었는지 확인](https://experienceleague.adobe.com/ko/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"}합니다(해당하는 경우).
4. 리디렉션 및 경로 추가([경로 구성](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"} 참조)

   >[!NOTE]
   >스테이징 및 프로덕션에 배포하기 전에 통합 환경에서 route.yaml 파일에 리디렉션 및 경로를 추가하고 이 환경에서 구성을 확인합니다.

       &quot;http://{all}/&quot;:
       유형: 업스트림
       업스트림: &quot;mymagento:http&quot;
       
       &quot;http://{all}/&quot;:
       유형: 업스트림
       업스트림: &quot;mymagento:http&quot;
   
5. 개발 중에 활성화된 경우 XDebug가 비활성화되는지 확인합니다([Xdebug 구성](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"} 참조).
6. op-cache 및 기타 구성이 php.ini 파일에서 정확하게 업데이트되었는지 확인합니다([이 샘플을 참조](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. [**Adobe Commerce 상태 페이지**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}에 가입하세요.
8. 지정된 성능 지표([자세히 보기](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"})를 모니터링하려면 New Relic &quot;[Adobe Commerce용 관리 경고](https://experienceleague.adobe.com/ko/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}&quot; 알림 채널을 구독하세요.

## 9. 보안

1. Adobe Commerce 보안 검사 설정

   >[!NOTE]
   > [Adobe Commerce 보안 검사는 사이트에서 오래된 소프트웨어 버전, 잘못된 구성 및 잠재적인 맬웨어를 검색하는 데 유용한 도구입니다](https://experienceleague.adobe.com/ko/docs/commerce-admin/systems/security/security-scan){target="_blank"}. 등록하고, 자주 실행되도록 예약하며, 이메일이 올바른 기술 보안 담당자에게 전송되는지 확인하십시오.
   > 
   > UAT 중에 이 작업을 완료하십시오. 주기적 스캔 옵션을 사용하는 경우 수요가 적은 시간에 스캔을 예약해야 합니다. Adobe Commerce 계정의 [보안 검사](https://account.magento.com/scanner/index/dashboard/){target="_blank"} 페이지를 참조하세요. 보안 검사에 액세스하려면 Adobe Commerce 계정에 로그인해야 합니다.

2. Adobe Commerce 관리자에 대한 기본 설정을 변경합니다.
3. 관리자 암호를 변경합니다([관리자 보안 구성](https://experienceleague.adobe.com/ko/docs/commerce-admin/systems/security/security-admin){target="_blank"} 참조).
4. 관리자 URL을 변경합니다([사용자 지정 관리자 URL 사용](https://experienceleague.adobe.com/ko/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"} 참조).
5. 더 이상 프로젝트에 없는 사용자를 제거합니다([사용자 만들기 및 관리](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"} 참조).
6. 관리자용 암호가 구성되었습니다([관리자 암호 요구 사항](https://experienceleague.adobe.com/ko/docs/commerce-admin/systems/security/security-admin){target="_blank"} 참조).
7. 이중 인증을 구성합니다([이중 인증](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"} 참조).

## 10. 라이브 진행

전환 시간이 되면 다음 단계를 수행하십시오(자세한 내용은 [DNS 구성](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}을 참조하십시오).

1. DNS 서비스에 액세스하고 각 도메인 및 호스트 이름에 대한 A 및 CNAME 레코드를 업데이트합니다.
   1. **prod.magentocloud.map.fastly.net**&#x200B;을(를) 가리키는 _&lt;&lt;www.yourdomain.com>>_&#x200B;에 대한 CNAME 레코드 추가
   2. _&lt;&lt;yourdomain.com>>_&#x200B;에 대해 A 레코드 4개를 설정하고 다음을 가리킵니다.\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Adobe Commerce 기본 URL을 _&lt;&lt;www.yourdomain.com>>_(으)로 변경
3. TTL 시간이 경과될 때까지 기다린 다음 웹 브라우저를 다시 시작합니다.
4. 웹 사이트를 테스트합니다.

### Go-Live를 차단하는 데 문제가 있는 경우:

전환 중에 시작하지 못하는 문제가 발생하는 경우, 적시에 적절한 지원을 받을 수 있는 가장 빠른 방법은 헬프 데스크를 사용하여 &quot;내 스토어를 시작할 수 없습니다&quot;라는 이유로 티켓을 열고 핫라인 지원 번호로 전화를 거는 것입니다([Adobe Commerce P1(우선 순위 1) 핫라인 번호 목록](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"} 참조).

- 미국 무료 전화: (+1) 877 282 7436(Adobe Commerce P1 핫라인으로 직접 연결)
- 미국 무료 전화: (+1) 800 685 3620(첫 번째 메뉴에서 7번을 누르면 Adobe Commerce P1 핫라인)
- 미국 로컬: (+1) 408 537 8777

## 11. Go-Live 후

사이트가 활성 상태가 되면 할당된 CTA(고객 기술 자문), CSE(고객 성공 엔지니어) 및 AM(계정 관리자)에게 이메일을 보냅니다. 그러나 프로젝트에 할당된 계정 관리자가 없는 경우 사이트가 활성 상태가 되면 하이 SLA 모니터링을 활성화하도록 요청하는 지원 티켓을 생성할 수 있습니다. CTA/CSE는 Fastly를 활성화하고 캐싱을 사용하여 사이트를 시작하는 것이 확인되면 즉시 다음 작업을 수행합니다.

- 클러스터에 라이브로 태그를 지정하고 지원 티켓을 만들어 높은 SLA(SLA) 모니터링을 활성화합니다.
- 가동 시간 모니터링을 위해 New Relic Synthics를 활성화합니다.

>[!MORELIKETHIS]
> 
> - [Launch 준비 개요 - 구현 플레이북](https://experienceleague.adobe.com/ko/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [시작 검사 목록 - 사용 안내서](https://experienceleague.adobe.com/ko/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [사전 실행 검사 목록 - 사이트 관리자/Commerce 관리자 가이드](https://experienceleague.adobe.com/ko/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [공유 권한 보안 모델](https://experienceleague.adobe.com/ko/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
