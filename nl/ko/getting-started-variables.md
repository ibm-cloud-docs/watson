---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-02"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# VCAP\_SERVICES 환경 변수
{: #vcapServices}

`VCAP_SERVICES` 환경 변수에는 {{site.data.keyword.Bluemix}}의 서비스 인스턴스와 상호작용하는 데 사용할 수 있는 정보가 포함됩니다. 이 환경 변수의 필드는 서비스를 애플리케이션에 바인드할 때 설정됩니다.
{: shortdesc}

{{site.data.keyword.watson}} 서비스로 인증하려면 애플리케이션에 URL 및 HTTP 기본 인증 서비스 신임 정보가 필요합니다.

실행 중인 애플리케이션은 일반적으로 서비스가 바인드된 후 이러한 환경 변수를 읽어서 필수 이름-값 쌍을 추출합니다. 이 변수에 대한 정보는 [앱 배치](/docs/manageapps/depapps.html#app_env)를 참조하고 "환경 변수"를 검색하십시오.

## 예
이 예에서는 애플리케이션에 바인드된 {{site.data.keyword.personalityinsightsshort}} 서비스에 대한 `VCAP_SERVICES` 환경 변수를 보여줍니다.

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

| 항목     | 설명                                                                                       |
|----------|--------------------------------------------------------------------------------------------|
| password | 서비스에 연결하는 데 사용되는 HTTP 기본 인증 신임 정보의 비밀번호                          |
| url      | 서비스에 대한 연결 URL                                                                     |
| username | 서비스에 연결하는 데 사용되는 HTTP 기본 인증 신임 정보의 사용자 이름                     |
| label    | {{site.data.keyword.cloud_notm}}의 서비스와 연관된 이름                            |
| name     | 서비스 인스턴스의 이름                                                                     |
| plan     | {{site.data.keyword.cloud_notm}}의 서비스에 사용 가능한 플랜                       |
| tags     | 서비스에 대한 추가 정보                                                                    |

## 환경 변수 보기
Cloud Foundry 도구에서 또는 {{site.data.keyword.cloud_notm}} 인터페이스 내에서 변수에 대한 정보를 검색할 수 있습니다.

- Cloud Foundry 명령행 도구 사용: 서비스를 작성하여 애플리케이션에 바인드한 후 `cf env` 명령을 사용하십시오.

    ```bash
    cf env {application-name}
    ```
    {: pre}

- {{site.data.keyword.cloud_notm}} 인터페이스에서: 애플리케이션 요약 페이지에서 환경 변수에 대한 정보를 사용할 수 있습니다.
