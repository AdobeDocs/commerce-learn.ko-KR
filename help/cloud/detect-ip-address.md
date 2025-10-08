---
title: IP 주소 감지
description: 보안을 강화하고 서버 통신을 간소화하기 위해 Adobe Commerce Cloud 환경의 IP 주소를 감지하는 방법을 알아봅니다
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: b015b9c64be631b43ad63d180c003dda8fdd198a
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# 다양한 환경에 대한 IP 주소 감지

Adobe Commerce Cloud 프로젝트에서 다양한 환경의 IP 주소를 감지하는 방법에 대해 알아봅니다. Adobe Commerce CLI, sed, xargs, dig, grep 및 sort -u를 포함한 일련의 명령을 사용하여 개발, 스테이징 및 프로덕션 환경에 대한 IP 주소를 식별할 수 있습니다.

## 이 비디오는 누구의 것입니까?

* 개발자: Adobe Commerce Cloud 프로젝트에 대한 IP 주소를 수집하는 방법을 이해하려고 합니다.
* 서드파티 또는 백엔드 시스템에 대한 액세스를 제한해야 하는 DevOps 및 보안 팀

## 비디오 콘텐츠 {#video-content}

* Adobe Commerce Cloud에서 모든 환경의 IP 주소를 찾는 방법에 대해 알아봅니다.

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## IP 주소를 가져오는 명령

자리 표시자 정보 대신 프로젝트 ID와 환경 이름을 사용해야 합니다.  Adobe Commerce 클라우드 클러스터에 있는 노드 수와 일치하도록 `{1..3}`을(를) 변경해야 할 수도 있지만 3이 가장 일반적입니다.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Adobe Commerce Cloud CLI

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

magento-cloud CLI 도구는 개발자와 시스템 관리자가 Adobe Commerce Cloud 프로젝트 및 환경을 효율적으로 관리할 수 있도록 설계되었습니다. 클라우드 콘솔의 기능을 확장하여 사용자가 일상적인 작업을 수행하고 로컬에서 자동화를 실행할 수 있도록 합니다. 주요 기능에는 통합 환경 관리, 환경 체크 아웃 및 병합, 변수 나열 및 SSH를 사용하여 원격 환경에 연결 등이 있습니다. 이 도구는 로컬 워크스테이션에서 직접 명령을 실행할 수 있도록 하여 워크플로우를 간소화함으로써 전반적인 개발 및 배포 프로세스를 향상시킵니다.

예제 코드의 이 초기 섹션 `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1`에서 환경에 대한 URL을 요청하고 있습니다. 반환된 값은 이 `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`과(와) 비슷합니다. 가끔씩 이 `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`과(와) 비슷하게 보입니다.  이 첫 번째 명령은 다소 간단하며, 이제 다음 명령으로 이동할 차례입니다.

자세한 내용은 [Cloud CLI 개요](https://experienceleague.adobe.com/ko/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}를 참조하십시오.

## 검색 및 바꾸기에 `sed` 사용

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

`xargs`과(와) 함께 `dig`을(를) 사용하면 복잡해지지만 필요합니다. 목표는 정리된 URL을 가져와서 저장하는 것입니다.  URL이 변수로 저장되면 `dig` 명령에 삽입됩니다.

DNS 정보를 수집하기 위해 `dig` 명령을 만들었습니다. 반환되는 데이터의 양을 줄이기 위해 인수 `+short`을(를) 사용합니다. `dig`과(와) 결합된 `+short`을(를) 사용하면 IP 주소와 때로 문자열이 반환됩니다.

명령의 해당 부분에서 `xargs`이(가) 해당 URL `abcikdxbg789.ent.magento.cloud`을(를) 저장하고 다음 명령 `dig`에 삽입하고 있습니다. 반복과 결합된 URL을 저장하는 기술로 `dig` 명령을 더 쉽게 사용할 수 있습니다. 샘플 코드가 목표를 달성하기 위한 한 가지 방법임을 기억하고, 요구 사항과 기대에 맞게 자유롭게 항목을 수정할 수 있습니다.

이제 URL이 준비되었습니다. 다음으로 클러스터의 각 서버를 확인하는 방법을 살펴보겠습니다. Adobe Commerce Cloud의 경우 클러스터에 있는 각 서버에는 숫자가 있습니다. 서버 식별자는 방금 정리되어 사용할 준비가 된 URL의 접두사입니다. `{1..3}`을(를) 사용하면 서버를 빠르고 쉽게 확인할 수 있습니다. `{1..3}` 명령을 세 번 실행함을 알리는 `dig`을(를) 사용합니다. 다음은 실행을 실시간으로 지켜볼 경우 발생하는 상황을 나타낸 것입니다.

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

* [지역 IP 주소](https://experienceleague.adobe.com/ko/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
