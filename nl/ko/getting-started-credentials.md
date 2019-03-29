---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

subcollection: watson

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Cloud Foundry 서비스 인증 정보를 사용한 인증
{: #creating-credentials}

{{site.data.keyword.ibmwatson}} 서비스에 대한 Cloud Foundry 서비스 인증 정보는 {{site.data.keyword.cloud}}의 서비스에 대한 인증을 제공합니다. 서비스 인증 정보는 HTTP 기본 인증을 사용하여 특정 {{site.data.keyword.watson}} 서비스에 인증합니다.
{: shortdesc}

리소스 그룹에서 작성되거나 Cloud Foundry에서 리소스 그룹으로 마이그레이션된 {{site.data.keyword.watson}} 서비스는 IAM 인증을 사용합니다. 기본적으로 모든 새 {{site.data.keyword.watson}} 서비스는 IAM 인증을 사용합니다. 리소스 그룹 및 IAM 인증에 대한 자세한 정보는 [IAM 토큰을 사용한 인증](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually)을 참조하십시오.
{: note}

## Cloud Foundry 서비스 인증 정보 얻기
{: #getting-credentials-manually}

Cloud Foundry 서비스 인증 정보를 사용하여 API 메소드에 액세스하려면 먼저 인증 정보를 수집해야 합니다. 서비스 인증 정보는 {{site.data.keyword.cloud_notm}} 웹 인터페이스에서 액세스할 수 있습니다. 

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}){: new_window}에 로그인하십시오. 
1.  [리소스 목록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/dashboard){: new_window}으로 이동하십시오. 
1.  서비스 인스턴스를 선택하십시오. 
1.  **인증 정보 표시**를 클릭하여 인증 정보의 **사용자 이름**, **비밀번호** 및 **URL**을 보십시오. 

## Cloud Foundry 서비스 인증 정보 업데이트
{: #existing-svcs}

서비스 대시보드에서 기존 서비스 인스턴스에 대한 서비스 인증 정보를 업데이트할 수 있습니다.

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}){: new_window}에 로그인하십시오. 
1.  [리소스 목록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/dashboard){: new_window}으로 이동하십시오. 
1.  페이지 왼쪽의 탐색 메뉴에서 **서비스 인증 정보**를 선택하십시오. 
1.  메뉴 및 아이콘을 사용하여 기존 인증 정보를 삭제하거나 새 인증 정보를 추가하십시오. 

변경 사항이 있는 경우에는 이에 맞춰 애플리케이션의 인증 정보를 반드시 업데이트하십시오. 

## {{site.data.keyword.Bluemix_dedicated_notm}}의 인증 정보

[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated) 인스턴스에서는 조직의 서비스당 하나의 타일만 허용되도록 서비스 타일이 구현됩니다.

자체 {{site.data.keyword.Bluemix_dedicated_notm}} 작업공간에서 서비스를 참조하려면 다음 단계에 따르십시오.

1.  사용할 서비스의 참조 타일에 대한 인증 정보 세트를 작성하십시오.
1.  해당 인증 정보를 사용하는 사용자 정의 사용자 제공 서비스를 작성하십시오.

    1.  웹 인터페이스를 사용하여 사용할 서비스에 대한 타일을 선택하고 서비스에 대한 인증 정보 세트를 작성하십시오. 인증 정보는 JSON 형식으로 작성되며 서비스 엔드포인트 URL, 사용자 이름 및 비밀번호를 포함합니다. 
    1.  `ibmcloud cf cups`(사용자 제공 서비스 작성) 명령을 사용하여 해당 인증 정보에 바인드된 사용자 정의 서비스를 작성하십시오. 예를 들어, 다음과 같습니다.

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      이러한 인증 정보로 `Personality-Insights-Std`라는 사용자 정의 사용자 제공 서비스 인스턴스를 작성하십시오. 

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      사용자 이름 및 비밀번호를 큰따옴표 안에 콜론으로 구분된 단일 문자열로 지정해야 합니다. 또한 사용자 이름 및 비밀번호 문자열을 둘 다 큰따옴표로 묶고 큰따옴표를 백슬래시로 이스케이프하십시오.
      {: tip}

### {{site.data.keyword.Bluemix_dedicated_notm}}에서 앱 사용

GitHub의 일부 {{site.data.keyword.watson}} 샘플 앱 및 스타터 킷에는 **{{site.data.keyword.cloud_notm}}에 배치** 단추가 포함되어 있습니다. 이러한 단추는 공용 {{site.data.keyword.cloud_notm}} 환경을 참조하므로 {{site.data.keyword.Bluemix_dedicated_notm}} 설치에서는 작동하지 않습니다. 

{{site.data.keyword.Bluemix_dedicated_notm}} 환경에서 샘플 애플리케이션 및 스타터 킷을 사용하려면 다음 단계를 따르십시오. 

1.  애플리케이션에 대한 지시사항에서 사용자가 작성하는 `manifest.yml` 파일이 `ibmcloud cf cups` 명령을 사용하여 작성한 서비스의 이름을 사용하는지 확인하십시오. 
1.  동일한 지시사항에서 `ibmcloud cf create-service` 명령을 실행하는 대신 `ibmcloud cf cups` 명령을 사용하십시오. 그러나 사용자 정의 사용자 제공 서비스 인스턴스를 작성한 경우 샘플 애플리케이션 또는 스타터 킷에서 사용하기 위해 이를 다시 작성할 필요는 없습니다.
