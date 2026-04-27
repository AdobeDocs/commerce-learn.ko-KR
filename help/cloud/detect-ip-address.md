---
title: Detect IP addresses
description: Learn how to detect IP addresses for Adobe Commerce Cloud environments to enhance security and streamline server communication
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 624
last-substantial-update: 2025-04-07T00:00:00.000Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
TQID: https://experienceleague.adobe.com/evduXZiZpjjhXbDahgpPjmemxRYcMhFJIwfu4GsYI9c
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1127
ht-degree: 0%

---

# Detect IP addresses for different environments

Learn how to detect IP addresses for different environments in an Adobe Commerce Cloud project. By using a series of commands including Adobe Commerce CLI, sed, xargs, dig, grep, and sort -u, users can identify IP addresses for development, staging, and production environments.

## 이 비디오는 누구의 것입니까?

* Developers: looking to understand how to gather the IP addresses for the Adobe Commerce Cloud project.
* DevOps and security teams needing to restrict access to third party or backend systems

## 비디오 콘텐츠 {#video-content}

* Learn how to uncover the IP address for any environment in Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493?learn=on)

## Command to get the IP address

Please note, you need to use your project ID and the environment name instead of the placeholder information.  There may also be a need to change the `{1..3}` to match the number of nodes in your Adobe Commerce Cloud cluster, but 3 is most common.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

The magento-cloud CLI tool is designed to help developers and system administrators manage Adobe Commerce Cloud projects and environments efficiently. It extends the functionality of the Cloud Console, enabling users to perform routine tasks and run automation locally. Key features include managing integration environments, checking out and merging environments, listing variables, and using SSH to connect to remote environments. The tool simplifies workflows by allowing commands to be executed directly from the local workstation, enhancing the overall development and deployment process.

In this initial section of the example code, `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1` it is requesting the URL for the environment. The returned value looks something like this `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`. Every once in a while it looks more like this `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  This first command is rather simple, and now it&#39;s time to move along to the next command.

For more information, please read [Cloud CLI Overview](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Using `sed` for search and replace

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

UNIX®Linux®의 `sed` 명령은 스트림 편집기를 나타냅니다. 입력 스트림(파일 또는 파이프라인의 입력)에 대해 기본적인 텍스트 변환을 수행하는 데 사용됩니다. 일반적인 용도로는 텍스트 검색, 찾기 및 바꾸기, 삽입 및 삭제가 있습니다. `sed` 명령은 텍스트를 한 줄로 처리하고 지정된 작업을 적용하여 텍스트 조작 및 스크립팅을 위한 강력한 도구입니다.

앞에서 언급했듯이 `magento-cloud` CLI에서 반환되는 URL은 일반적으로 두 가지입니다. 중간에 `.com.c.c`이(가) 포함된 변형이 있습니다. 이 변형은 조작해야 하는 변형입니다. 이 구조가 감지되면 URL의 시작 부분부터 `.com.c.c`까지 모든 항목을 제거해야 합니다.  그러면 남아 있는 것은 URL의 마지막 부분입니다. 예제 URL은 `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`과(와) 비슷합니다.  이 패턴이 감지되면 목표는 `.c.` 이후에 모든 항목을 유지하는 것입니다.  제공된 이 예제 코드에서는 `sed 's/.\.c\.(.)/\1/'`을(를) 사용하여 이 부분을 선택하고 원래 반환된 나머지 값은 무시합니다. URL의 나머지 부분은 `abcikdxbg789.ent.magento.cloud/`과(와) 비슷합니다.\
`sed`에 두 개의 명령이 실행되고 있습니다. 그것들은 세미콜론으로 구분됩니다. `sed` 명령 `;s/.$//'`의 두 번째 부분은 후행 슬래시가 있는 경우 제거하고 해당 URL을 `abcikdxbg789.ent.magento.cloud`처럼 정리하는 것입니다.  이 시점에서 URL이 정리되고 다음 명령을 수행할 준비가 되었습니다.

## dig가 있는 Xargs

```bash
xargs -I% dig +short {1..3}."%"
```

UNIX®Linux®의 `xargs` 명령은 표준 입력에서 명령줄을 만들고 실행하는 데 사용됩니다. 이 메서드는 파이프 또는 파일에서 입력을 가져와서 다른 명령의 인수로 변환합니다. 이 메서드는 셸의 제한을 초과하는 많은 수의 인수를 처리할 때 특히 유용합니다. `xargs` 명령을 사용하여 파일을 이동, 복사 또는 삭제하는 등의 작업을 수행할 수 있습니다. 여러 인수를 한 번의 실행으로 명령에 전달하여 배치 처리를 효율적으로 수행할 수 있습니다.

도메인 정보 수집기의 약어로, `dig` 명령은 DNS(Domain Name System) 서버를 쿼리하는 데 사용되는 네트워크 관리 도구입니다. A, AAAA, MX 및 CNAME 레코드와 같은 DNS 레코드에 대한 정보를 검색하는 데 도움이 됩니다. `dig` 명령은 일반적으로 DNS 문제를 해결하고 DNS 구성을 확인하고 도메인 이름과 관련 IP 주소에 대한 자세한 정보를 수집하는 데 사용됩니다. 다양한 옵션과 플래그를 사용하여 특정 세부 사항이나 간결한 요약을 표시하도록 출력을 사용자 지정할 수 있습니다.

`dig`과(와) 함께 `xargs`을(를) 사용하면 복잡해지지만 필요합니다. 목표는 정리된 URL을 가져와서 저장하는 것입니다.  URL이 변수로 저장되면 `dig` 명령에 삽입됩니다.

DNS 정보를 수집하기 위해 `dig` 명령을 만들었습니다. 반환되는 데이터의 양을 줄이기 위해 인수 `+short`을(를) 사용합니다. `+short`과(와) 결합된 `dig`을(를) 사용하면 IP 주소와 때로 문자열이 반환됩니다.

명령의 해당 부분에서 `xargs`이(가) 해당 URL `abcikdxbg789.ent.magento.cloud`을(를) 저장하고 다음 명령 `dig`에 삽입하고 있습니다. 반복과 결합된 URL을 저장하는 기술로 `dig` 명령을 더 쉽게 사용할 수 있습니다. 샘플 코드가 목표를 달성하기 위한 한 가지 방법임을 기억하고, 요구 사항과 기대에 맞게 자유롭게 항목을 수정할 수 있습니다.

이제 URL이 준비되었습니다. 다음으로 클러스터의 각 서버를 확인하는 방법을 살펴보겠습니다. Adobe Commerce Cloud의 경우 클러스터에 있는 각 서버에는 숫자가 있습니다. 서버 식별자는 방금 정리되어 사용할 준비가 된 URL의 접두사입니다. `{1..3}`을(를) 사용하면 서버를 빠르고 쉽게 확인할 수 있습니다. `dig` 명령을 세 번 실행함을 알리는 `{1..3}`을(를) 사용합니다. 다음은 실행을 실시간으로 지켜볼 경우 발생하는 상황을 나타낸 것입니다.

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

더 나은 설명을 위해 `dig`에서 사용하는 이 예제 URL은 다음과 비슷합니다.

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

`{1..3}`이(가) `{1..6}`(으)로 수정되면 다음과 같이 표시됩니다.

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## 명령 `grep`

문자열이 `dig`에서 결과의 일부로 반환되는 경우가 있습니다. 이를 위해 목표는 IP 주소만 사용하며 마침표가 있는 숫자로 구성됩니다. 최종 출력에 숫자만 포함되도록 하려면 명령을 조정할 수 있습니다. 완료되면 최종 구문은 ` grep '^\d'`입니다.  `'^\d'`을(를) 추가하면 `grep` 명령은 숫자만 유지하고 다른 항목은 무시합니다.

## 명령 `sort`

`sort -u`을(를) 사용하면 IP 목록에서 모든 중복을 제거합니다. 중복은 개발 수준을 확인할 때만 발생합니다.

이러한 하위 수준 환경은 다중 테넌트이며 기본 서버를 다른 많은 프로젝트와 공유합니다. 개발 환경은 단일 서버이며 클러스터가 아닙니다. 따라서 dig 명령이 각 반복을 반복할 때 동일한 IP를 여러 번 반환합니다. 따라서 명령 `sort -u`을(를) 사용하면 모든 중복 IP가 제거되고 고유한 IP 주소만 유지됩니다.



## 관련 설명서

* [지역 IP 주소](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
