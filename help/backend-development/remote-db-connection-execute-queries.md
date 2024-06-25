---
title: 데이터베이스에 대한 쿼리 연결 및 실행
description: Adobe Commerce 클라우드 프로젝트에 연결하는 몇 가지 방법을 알아봅니다. 오프사이트에서 사용할 DB를 다운로드하는 방법에 대해 알아봅니다. PII를 마스킹하고 제거하는 몇 가지 방법을 알아봅니다.
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-06-25T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: 435364592c0b609b3c379bb58df80e2691c82d40
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 0%

---

# Adobe Commerce 데이터베이스에 대한 쿼리 연결 및 실행

Adobe Commerce on cloud 프로젝트에 연결하고, 오프사이트 사용을 위해 데이터베이스 덤프를 만들고, 이를 마스킹 또는 제거하여 PII(개인 식별 정보)를 처리하는 방법에 대해 알아봅니다. 로컬 DB 덤프, MySQL Workbench 또는 TablesPlus와 같은 애플리케이션과의 원격 DB 연결 및 Magento Cloud CLI 도구를 통한 직접 연결 등 다양한 방법을 사용하여 Adobe Commerce 데이터에 액세스하는 방법에 대해 알아봅니다.

## 비디오 콘텐츠

* MysqlWorkbench 또는 TablesPlus와 같은 도구를 사용하여 원격 Adobe Commerce Cloud 프로젝트에 빠르게 연결하는 방법을 알아봅니다.
* 명령줄을 통해 SQL을 실행하기 위해 Adobe Commerce 프로젝트에 빠르게 연결하는 방법에 대해 알아봅니다

>[!VIDEO](https://video.tv.adobe.com/v/3430507?learn=on)

Adobe Commerce on cloud 프로젝트에 연결하고, 오프사이트에서 사용할 데이터베이스를 덤프하고, PII를 마스킹하고 제거하는 방법을 알아봅니다.

다음 방법 중 하나를 사용하여 클라우드 프로젝트에서 Adobe Commerce 데이터에 액세스할 수 있습니다.

* 로컬 DB 덤프 사용
* Mysql Workbench 또는 Tables Plus와 같은 애플리케이션을 사용하는 원격 클라우드 환경에 대한 DB 연결
* magento-cloud CLI 도구를 사용하여 클라우드 환경에 직접 연결하고 원격 서버에서 명령을 실행합니다.

기본 방법은 데이터베이스 덤프를 수행하고 스크러빙하여 고객 정보를 제거하는 것입니다. 데이터가 필요하지 않은 경우 고객 데이터를 완전히 제거합니다.

## Adobe Commerce Cloud CLI 도구 사용

데이터베이스 덤프를 생성하려면 다음을 수행해야 합니다. [ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) 설치됨. 로컬 랩톱에서 디렉터리로 이동하여 다음 명령을 실행합니다. 다음을 교체해야 합니다. `your-project-id` 프로젝트 ID를 사용하여 (과 유사) `asasdasd45q`. 또한 다음을 교체해야 합니다. `your-environment-name` 환경 이름 포함, 예: `master` 또는 `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

프로젝트 ID나 환경을 잘 모를 경우 명령에서 이를 생략할 수 있습니다.

`magento-cloud db:dump`

CLI에서 올바른 프로젝트 및 환경을 지정하라는 메시지가 표시됩니다. 다음 예제에서는 해당 대화 상자를 표시합니다. 이 예에서는 계정에 할당된 여러 프로젝트를 보여 주지만 사용 가능한 프로젝트는 하나만 있을 수 있습니다.

디렉터리로 변경

```bash
cd ~/Downloads/db-tutorial 
```

이제 명령을 실행하여 데이터베이스 덤프를 생성합니다.

```bash
magento-cloud db:dump
```

프로젝트 또는 환경을 지정하지 않았으므로 Adobe Commerce CLI에 몇 가지 질문이 있습니다. 대화 상자의 예는 다음과 같습니다

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Adobe Commerce ECE 도구 사용

Adobe Commerce CLI 도구가 없는 경우 `ssh` 을(를) 프로젝트에 입력하고 `ece` 명령 `vendor/bin/ece-tools db-dump`: 샘플 응답:

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

사용 `SFTP` 또는 `rsync` 를 클릭하여 데이터베이스 덤프를 로컬 환경으로 가져옵니다.

다음 예제에서는 를 사용합니다 `rsync` 파일을 로 가져오려면 `~/Downloads/db-tutorial` 폴더를 삭제합니다.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

터미널 창은 몇 가지 정보를 출력합니다. 여기 몇 가지 출력 예가 있습니다

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

파일의 내용을 보고 파일이 성공적으로 다운로드되었는지 확인합니다.

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

데이터가 있는 경우 고객 데이터를 제거하거나 마스킹하여 정리해야 합니다. 다음 샘플 스크립트는 시작하는 데 도움이 됩니다.

이 예제는 고객 데이터를 무작위 문자열로 전환하지만 모든 항목을 유지합니다. 이 예에는 고객 PII를 타사 테이블 및 핵심 테이블에서 찾을 수 있음을 보여 주는 몇 가지 추가 테이블이 포함되어 있습니다. 모든 테이블의 데이터를 주의 깊게 검사하고 고객 데이터를 마스킹 또는 제거합니다.

일반적으로 설계자 또는 리드 개발자는 데이터베이스 덤프의 마스킹 및 살균을 담당하는 유일한 사람입니다. 전용 소독제를 비치하면 원시 데이터 노출이 줄어들어 규정 준수 규칙 및 규정 위반 기회가 줄어든다.

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

또는 정보를 마스킹하는 대신 레코드를 삭제할 수 있으므로 새 DB가 더 작아집니다. PII가 마스킹되거나 제거되면 데이터는 로컬 환경에서 사용하기 위해 팀원에게 안전하게 제공될 수 있습니다.

## Adobe Commerce Cloud 프로젝트에 대한 원격 DB 연결

이 방법을 사용하면 실수로 실제 데이터를 편집하고 삭제할 수 있습니다. 이 접근법은 주의하여 사용해야 한다. 데이터베이스 백업을 사용하고 오프라인으로 데이터를 검토하는 것이 선호됩니다. Adobe Commerce Cloud에서 직접 데이터에 액세스해야 하는 경우가 있지만 위험성이 수반됩니다. &quot;확실합니까?&quot;가 없습니다. 질문으로, 의도치 않게 데이터를 변경하거나 제거할 수 있습니다.

아주 중요해요! 원격 DB 연결을 수행하는 것은 편리하고 실제 라이브 데이터를 사용하는 것이 좋지만 위험이 수반됩니다. 개인적으로 그리고 Adobe Commerce의 수석 기술 설계자로서는 권장하지 않습니다. 원격 DB에 있는 것을 잊어버리고 실수로 데이터를 삭제하거나 수정하기가 너무 쉽습니다. 읽기 전용 복제본에 연결하는 옵션이 있지만, 이 옵션은 SQL 작업의 무겁기에 따라 사이트에 영향을 줍니다. 그러나 가능하므로 이를 수행하기 위한 단계입니다.

SSH 터널 설정:

```bash
magento-cloud tunnel:open
```

프로젝트를 선택하고 환경을 선택하면 mysql 그래픽 인터페이스 설정에 사용되는 명령의 출력이 나타납니다.

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

다음을 사용하여 MySQL 그래픽 인터페이스를 사용하여 연결 설정 `SSH tunnel opened to database at` 명령 옵션을 사용합니다.

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

이제 올바른 정보를 얻었으므로 이러한 값을 Cloud Console에 계속 삽입합니다.

클라우드 콘솔의 클라우드 자격 증명에서 SSH 호스트 이름과 사용자 이름을 찾을 수 있습니다.

![로고 - Adobe Commerce Cloud 콘솔](./assets/cloud-ui-screenshot.png "Adobe Commerce Cloud 콘솔")

다음은 한 가지 예입니다. `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
SSH 호스트 이름은 @ 기호 뒤에 있는 모든 것입니다. `ssh.us-4.magento.cloud` 이 예제에서
SSH 사용자 이름은 @ 기호 앞에 있는 모든 것입니다.  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## 데이터베이스에 연결할 값 찾기

MariaDB 데이터베이스에 직접 액세스하려면 SSH를 사용하여 원격 클라우드 환경에 로그인하고 데이터베이스에 연결해야 합니다.

1. SSH를 사용하여 원격 환경에 로그인합니다.

   ```bash
   magento-cloud ssh
   ```

1. 에서 MySQL 로그인 자격 증명 검색 `database` 및 `type` 의 속성 [$MAGENTO_클라우드_관계](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) 변수를 채우는 방법에 따라 페이지를 순서대로 표시합니다.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   또는

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   응답에서 MySQL 정보를 찾습니다. For example:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

그런 다음 MySQL GUI에서 구성 값을 사용합니다. 다음 예제에서는 MySQL Workbench를 사용하지만, MySQL 연결을 지원하는 앱도 유사한 필드를 갖게 됩니다.

![로고 - Mysql Workbench를 사용한 Mysql GUI 예](./assets/mysql-workbench-after-connecting.png " Mysql Workbench를 사용한 Mysql GUI 예")

![로고 - TablesPlus를 사용한 Mysql GUI 예](./assets/tablesPlus-db-connection.png " TablesPlus를 사용한 Mysql GUI 예")

모든 것이 설정되면 MySQL GUI를 사용하여 원격 Adobe Commerce Cloud 프로젝트에서 쿼리를 실행할 수 있습니다.

## SQL을 실행하기 위해 클라우드 프로젝트 데이터베이스에 직접 연결

다음 메서드는 `magento-cloud` cli를 사용하여 mysql 데이터베이스에 직접 연결하고 SQL을 실행하여 데이터베이스 쿼리 속도를 높일 수 있습니다. 이 데이터베이스를 복사해야 하는 경우 [데이터베이스 덤프 만들기](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

예를 들어 `core_config_data` 단어가 포함된 표 `secure` 열의 일부로 `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## 추가 리소스

[ADOBE COMMERCE CLOUD CLI](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[MySQL 서비스 설정](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[원격 MySQL 데이터베이스 연결 설정](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[클라우드 인프라의 Adobe Commerce에서 데이터베이스 덤프 만들기](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
