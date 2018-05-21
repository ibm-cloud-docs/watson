---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Watson 서비스에 대한 서비스 신임 정보

{{site.data.keyword.ibmwatson}} 서비스에 대한 서비스 신임 정보는 {{site.data.keyword.cloud}}의 서비스로의 인증을 제공합니다. 신임 정보 세트는 해당 신임 정보가 작성된 서비스에만 사용될 수 있습니다.
{: shortdesc}

사용하는 프로그래밍 모델에 따라 다르게 서비스 신임 정보를 사용합니다. 세부사항은 [{{site.data.keyword.watson}} 서비스에 대한 프로그래밍 모델](/docs/services/watson/getting-started-develop.html)을 참조하십시오.

서비스 신임 정보는 특정 {{site.data.keyword.watson}} 서비스에 대한 HTTP 기본 인증 신임 정보입니다.

## 수동으로 신임 정보 가져오기
{: #getting-credentials-manually}

{{site.data.keyword.cloud_notm}} 웹 인터페이스에서 서비스 신임 정보를 찾을 수 있습니다.

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}에 로그인하십시오.
1.  기존 서비스로 이동하십시오([해당 위치로 이동 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/developer/watson/existing-services){: new_window}).
    1.  왼쪽 상단 메뉴를 선택한 후 **{{site.data.keyword.watson}}**을 선택하여 {{site.data.keyword.watson}} 콘솔로 이동하십시오.
    1.  **Watson 서비스**에서 **기존 서비스**를 선택하면 서비스 및 프로젝트 목록을 볼 수 있습니다.
1.  신임 정보 보기:
    - 서비스가 프로젝트의 일부이면 서비스가 포함된 프로젝트의 이름을 클릭하십시오. 프로젝트 세부사항 페이지의 **신임 정보** 섹션에서 신임 정보를 보십시오.
    - 서비스가 프로젝트의 일부가 아니면 보려는 서비스 이름을 클릭한 후 **서비스 신임 정보**를 선택하십시오.

## 서비스 신임 정보 업데이트
{: #existing-svcs}

서비스 대시보드에서 기존 서비스 인스턴스에 대한 서비스 신임 정보를 업데이트할 수 있습니다.

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}에 로그인하십시오.
1.  기존 서비스로 이동하십시오([해당 위치로 이동 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/developer/watson/existing-services){: new_window}).
    1.  왼쪽 상단 메뉴를 선택한 후 **{{site.data.keyword.watson}}**을 선택하여 {{site.data.keyword.watson}} 콘솔로 이동하십시오.
    1.  **Watson 서비스**에서 **기존 서비스**를 선택하면 서비스 목록을 볼 수 있습니다.
1.  보거나 업데이트하려는 서비스를 선택한 후 **서비스 신임 정보**를 선택하십시오.
1.  신임 정보 삭제 또는 추가:
    - 신임 정보를 삭제하려면 신임 정보를 선택한 후 **신임 정보 삭제**를 선택하십시오.
    - 신임 정보를 추가하려면 **새 신임 정보**를 클릭한 후 "새 신임 정보 추가" 창에서 **추가**를 클릭하십시오. 새 신임 정보를 확인하고 복사하십시오.

애플리케이션에서 새 신임 정보를 사용하는지 확인하십시오.

- 애플리케이션이 프로그래밍 방식으로 해당 신임 정보를 얻는 경우 애플리케이션을 중지한 후 다시 시작하십시오.
- 신임 정보가 애플리케이션 코드에 임베드된 경우 코드를 업데이트하고 애플리케이션을 재실행하십시오.

최소 중단 시간으로 애플리케이션을 새 신임 정보로 업데이트하는 방법에 대한 세부사항은 [앱 업데이트](/docs/manageapps/updapps.html)를 참조하십시오.

## {{site.data.keyword.Bluemix_dedicated_notm}}의 신임 정보

[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) 인스턴스에서는 조직의 서비스당 하나의 타일만 허용되도록 서비스 타일이 구현됩니다.

자체 {{site.data.keyword.Bluemix_dedicated_notm}} 작업공간에서 서비스를 참조하려면 다음 단계에 따르십시오.

1.  사용할 서비스의 참조 타일에 대한 신임 정보 세트를 작성하십시오.
1.  해당 신임 정보를 사용하는 사용자 정의 사용자 제공 서비스를 작성하십시오.

    1.  {{site.data.keyword.cloud_notm}} 인터페이스를 사용하여 사용할 서비스에 대한 타일을 선택하고 서비스에 대한 신임 정보 세트를 작성하십시오. 신임 정보는 JSON 형식으로 작성되며 서비스와 통신할 URL과 해당 서비스에 사용할 사용자 이름 및 비밀번호에 대한 별도의 필드를 포함합니다.
    1.  `cf cups`(사용자 제공 서비스 작성) 명령을 사용하여 해당 신임 정보에 바인드된 사용자 정의 서비스를 작성하십시오. 예를 들어, 다음과 같은 신임 정보가 제공된 경우 

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      `Personality-Insights-Std`라는 사용자 정의 사용자 제공 서비스 인스턴스를 작성하십시오.

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      사용자 이름 및 비밀번호를 큰따옴표 안에 콜론으로 구분된 단일 문자열로 지정해야 합니다. 또한 사용자 이름 및 비밀번호 문자열을 둘 다 큰따옴표로 묶고 큰따옴표를 백슬래시로 이스케이프하십시오.
      {: tip}

### {{site.data.keyword.Bluemix_dedicated_notm}}에서 앱 사용

GitHub의 일부 {{site.data.keyword.watson}} 샘플 앱 및 스타터 킷에는 **{{site.data.keyword.cloud_notm}}에 배치** 단추가 포함되어 있습니다. 이러한 단추는 공용 {{site.data.keyword.cloud_notm}} 환경을 참조하므로 {{site.data.keyword.Bluemix_dedicated_notm}} 설치에서는 작동하지 않을 수 있습니다.

{{site.data.keyword.Bluemix_dedicated_notm}} 환경에서 샘플 애플리케이션 및 스타터 킷을 사용하려면 다음을 수행하십시오.

1.  애플리케이션에 대한 지시사항에서 사용자가 작성하는 `manifest.yml` 파일이 `cf cups` 명령을 사용하여 작성한 서비스의 이름을 사용하는지 확인하십시오.
1.  동일한 지시사항에서 `cf create-service` 명령을 실행하는 대신 `cf cups` 명령을 사용하십시오. 그러나 사용자 정의 사용자 제공 서비스 인스턴스를 작성한 경우 샘플 애플리케이션 또는 스타터 킷에서 사용하기 위해 이를 다시 작성할 필요는 없습니다.
