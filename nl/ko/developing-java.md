---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-09"

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

# Java로 {{site.data.keyword.watson}} 애플리케이션 개발

REST API의 사용을 보여주기 위해 각 {{site.data.keyword.ibmwatson}} 서비스는 {{site.data.keyword.watson}} GitHub 프로젝트에서 Java로 작성된 샘플 웹 기반 애플리케이션을 제공합니다.
{: shortdesc}

## 시작하기 전에

- {{site.data.keyword.cloud_notm}}의 계정을 작성하거나 기존 계정을 사용하십시오. [무료로 등록하십시오. ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window} 
- [Cloud Foundry ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli#downloads){: new_window} 명령행 클라이언트를 설치하십시오. 이미 설치되어 있는 경우 해당 버전이 최신 상태인지 확인하십시오.
- [Apache Maven ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://maven.apache.org/download.cgi){: new_window}을 다운로드하여 설치하십시오.
- [IBM WebSphere Liberty 프로파일 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developer.ibm.com/wasdev/downloads/){: new_window}을 설치하십시오. 런타임 버전의 WebSphere Liberty 프로파일만 다운로드하거나 버전을 Eclipse IDE용 플러그인으로 다운로드할 수 있습니다. 또한 *Java JDK* 또는 *Eclipse IDE*를 설치 및 구성하십시오.

## 소스 코드 가져오기
{{site.data.keyword.watson}}에 사용 가능한 샘플 Java 앱을 보려면 [{{site.data.keyword.watson}} GitHub ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud){: new_window} 프로젝트를 방문하십시오. 그런 다음 GitHub를 사용하여 로컬로 저장소를 복제하십시오.

## 로컬로 설치
앱을 수정하거나 자체 앱을 빌드하기 위한 기초로 사용하려면 로컬로 앱을 설치하십시오. 그런 다음 수정된 버전의 앱을 {{site.data.keyword.cloud_notm}}에 배치할 수 있습니다.

### {{site.data.keyword.watson}} 서비스 설정

1.  명령행에서 복제한 앱의 로컬 프로젝트 디렉토리로 이동하십시오.
1.  {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  서비스 및 플랜의 이름을 찾으십시오. 샘플 앱의 `manifest.yml` 파일에 `declared-services` 섹션이 포함되어 있는 경우 `label` 및 `plan`을 기록해 두십시오. 그렇지 않으면 `cf marketplace` 명령을 사용하여 서비스 및 플랜의 이름을 가져오십시오.

    ```bash
    cf marketplace
    ```
    {: pre}

1.  `cf create-service {service-name} {service-plan} {service-instance-name}`을 사용하여 {{site.data.keyword.cloud_notm}}에 서비스 인스턴스를 작성하십시오. service-name 및 service-plan에는 Manifest 파일 또는 `cf marketplace` 명령에서 얻은 정보를 사용하십시오.

    예를 들어, {{site.data.keyword.personalityinsightsshort}} 서비스에 대한 서비스 인스턴스를 작성하려면 다음 명령을 실행하십시오.

    ```bash
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### 앱 환경 구성

1.  `cf create-service-key {service_instance} {service_key}` 형식의 서비스 키를 작성하십시오. 예를 들어, 다음과 같습니다.

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  `cf service-key {service_instance} {service_key}` 명령을 사용하여 서비스 키에서 신임 정보를 검색하십시오. 예를 들어, 다음과 같습니다.

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    다음 예에서와 같이 이 명령의 출력은 JSON 오브젝트입니다.

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }

    ```
    {: codeblock}
1.  서비스 키의 값을 앱의 작업 디렉토리에 있는 앱의 소스 파일로 복사하십시오.

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### 앱 빌드 및 실행

이 지시사항에서는 런타임 버전의 WebSphere Liberty 프로파일을 사용합니다. Eclipse 플러그인을 사용하는 경우 단계를 수정해야 합니다.
{: tip}

1.  앱의 작업 디렉토리에서 다음 Maven 명령을 실행하여 앱을 빌드하십시오.

    ```java
    mvn clean package
    ```
    {: pre}

1.  WebSphere Liberty 프로파일 `server create` 명령을 실행하여 애플리케이션에 대한 서버를 작성하십시오. 명령에 대한 인수로 지정된 서버의 이름에는 임의의 이름을 사용할 수 있지만 로컬 WebSphere Liberty 프로파일 환경에서 고유해야 합니다.

    ```bash
    server create server-name
    ```
    {: pre}

1.  앱의 `.war` 파일을 `wlp-installation/usr/servers/server-name/dropins` 디렉토리로 복사하십시오. 여기서 *wlp-installation*은 WebSphere Liberty 프로파일의 로컬 설치에 대한 기본 디렉토리이며 *server-name*은 이전 단계에서 서버에 대해 지정한 이름입니다.
1.  WebSphere Liberty 프로파일의 서버를 시작하십시오. 앱에 대한 서버 이름을 명령에 대한 인수로 지정하십시오.

    ```bash
    server start server-name
    ```
    {: pre}

1.  앱을 사용해 보려면 브라우저에 http://localhost:9080/webApp 을 지정하십시오. 포트는 `app.js` 파일에 지정되어 있습니다.

    포트는 `wlp-installation/usr/servers/server-name/server.xml` 파일에 지정되어 있습니다. 포트를 변경하려면 `server.xml`의 값을 변경하고 서버를 중지한 후 다시 시작하십시오.

### 문제점 해결

서버가 올바르게 시작되지 않거나 응답하지 않는 경우 `wlp-installation/usr/servers/server-name/logs` 디렉토리에 있는 로그 파일을 검사하여 원인을 판별하십시오.

## {{site.data.keyword.cloud_notm}}에 배치

Cloud Foundry를 사용하여 로컬 버전의 앱을 {{site.data.keyword.cloud_notm}}에 배치할 수 있습니다.

1.  프로젝트 루트 디렉토리에서 manifest.yml 파일을 여십시오.
    1. `manifest.yml` 파일의 `applications` 섹션에서 이름 값을 사용자 앱 버전에 대한 고유 이름으로 변경하십시오.
    1. `services` 섹션에서 앱에 대해 작성한 {{site.data.keyword.watson}} 서비스 인스턴스의 이름을 지정하십시오. 서비스 이름을 기억하지 못하는 경우 `cf services` 명령을 사용하여 서비스를 나열하십시오.

        다음 예에서는 수정된 `manifest.yml` 파일을 보여줍니다.

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: tiered
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  {{site.data.keyword.cloud_notm}}에 로그인하십시오.

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  앱을 {{site.data.keyword.cloud_notm}}로 푸시하고 `manifest.yml` 파일에 있는 사용자 앱 이름을 지정하십시오. 예를 들어, 다음과 같습니다.

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    브라우저가 명령 출력에 지정된 URL을 가리키도록 하십시오.
