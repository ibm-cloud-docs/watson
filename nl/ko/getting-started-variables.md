---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: environment variable,service alias,VCAP services

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

# VCAP\_SERVICES 환경 변수
{: #vcapServices}

`VCAP_SERVICES` 환경 변수에는 {{site.data.keyword.cloud}}의 서비스 인스턴스와 상호작용하는 데 사용할 수 있는 정보가 포함됩니다. 이 환경 변수의 필드는 서비스 인스턴스를 애플리케이션에 바인드할 때 설정됩니다.
{: shortdesc}

애플리케이션은 {{site.data.keyword.watson}} 서비스에 대해 인증하는 데 URL 및 서비스 인증 정보를 필요로 합니다. 실행 중인 애플리케이션은 일반적으로 서비스가 이러한 환경 변수에 바인드된 후 이를 읽어 필수 이름-값 쌍을 추출합니다. 앱에는 리소스 그룹 또는 Cloud Foundry 서비스를 바인드할 수 있지만 리소스 그룹 서비스는 서비스 별명을 필요로 합니다. 

## IAM 서비스의 서비스 별명
{: #vcapIAMAlias}

서비스에 별명을 지정하여 `VCAP_SERVICES` 환경 변수를 리소스 그룹 서비스와 함께 사용할 수 있습니다. 예를 들어, `my-personality-insights-service` 라는 서비스에 대해 `personality-insights-service`라는 별명을 작성하려면 {{site.data.keyword.cloud_notm}} 명령행 인터페이스에서 다음 명령을 사용하십시오. 

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

이렇게 하고 나면 앱을 서비스 별명에 바인드할 수 있습니다. 실행 중인 애플리케이션은 일반적으로 서비스 인스턴스가 이러한 환경 변수에 바인드된 후 이를 읽어 필수 이름-값 쌍을 추출합니다. 

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## 예
{: #gs-variables-vcapIAMAliasExample}

이 예는 애플리케이션에 바인드된 {{site.data.keyword.personalityinsightsshort}} 서비스에 대한 `VCAP_SERVICES` 환경 변수를 보여줍니다. 이 서비스 인스턴스는 Cloud Foundry 인증을 사용합니다. 

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

다음 표에는 `VCAP_SERVICES` 환경 변수의 컨텐츠가 설명되어 있습니다.

|항목     |설명                                                                                |
|----------|--------------------------------------------------------------------------------------------|
|비밀번호 |서비스에 연결하는 데 사용되는 HTTP 기본 인증 인증 정보의 비밀번호 |
|url      |서비스에 대한 연결 URL                                                         |
|username |서비스에 연결하는 데 사용되는 HTTP 기본 인증 인증 정보의 사용자 이름 |
|label    |{{site.data.keyword.cloud_notm}}의 서비스와 연관된 이름                                            |
|이름     |서비스 인스턴스의 이름                                                           |
|plan     |{{site.data.keyword.cloud_notm}}의 서비스에 사용 가능한 플랜                                              |
|tags     |서비스에 대한 추가 정보                                                   |

## 환경 변수 보기
{: #gs-variables-vcapView}

{{site.data.keyword.cloud_notm}} 명령행 인터페이스(CLI) 또는 웹 인터페이스에서 이 변수에 대한 정보를 검색할 수 있습니다. 

- {{site.data.keyword.cloud_notm}} CLI 사용: 서비스를 작성하여 애플리케이션에 바인드한 후 Cloud Foundry `env` 명령을 호출하십시오. 

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- {{site.data.keyword.cloud_notm}} 인터페이스에서: 애플리케이션 요약 페이지에서 환경 변수에 대한 정보를 사용할 수 있습니다.
