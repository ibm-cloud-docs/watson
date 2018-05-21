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

# Cloud Foundry 명령행 인터페이스

`cf` 명령행 인터페이스(CLI) 도구를 사용하여 로컬 시스템에서 {{site.data.keyword.cloud}} 애플리케이션에 대해 작업할 수 있습니다. 이 도구는 전체 개발 환경에 대한 다양한 인터페이스를 제공합니다.
{: shortdesc}

애플리케이션 개발을 위한 Cloud Foundry 도구 관련 작업에 대한 자세한 정보는 [Cloud Foundry가 {{site.data.keyword.cloud_notm}}에 대해 작업하는 방법](/docs/overview/cf.html) 및 [Cloud Foundry 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://docs.cloudfoundry.org/){: new_window}를 참조하십시오.

## cf 명령행 인터페이스 설치

[Cloud Foundry GitHub 저장소 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/cloudfoundry/cli#downloads){: new_window}의 **다운로드** 섹션에서 최신 버전의 도구를 찾을 수 있습니다. 두 가지 유형의 패키지가 표시됩니다.

- **설치 프로그램**은 플랫폼의 패키지 관리 도구 또는 설치된 소프트웨어 추적 시스템과 통합되어 "공식" 위치에 도구를 설치합니다.
- **바이너리**는 임의의 위치에 설치할 수 있는 아카이브 파일을 제공합니다.

도구를 설치한 후 명령을 `PATH` 환경 변수에 포함시켜야 합니다.
{: tip}

## 서비스 인스턴스에 대한 명령

{{site.data.keyword.cloud_notm}}에서 기존 {{site.data.keyword.watson}} 서비스 인스턴스를 관리하는 데 유용한 `cf` 명령:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services): 현재 영역의 모든 서비스 인스턴스를 나열합니다.

  ```bash
  cf services
  ```
  {: pre}

  서비스의 이름을 알고 있는 경우 서비스에 대한 정보를 보려면 `cf service` 명령을 사용하십시오. 예를 들어, 다음과 같습니다.

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): {{site.data.keyword.cloud_notm}}에서 실행하는 애플리케이션에 바인드할 수 있는 서비스의 인스턴스를 작성합니다.

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - *{service-name}* 및 *{service-plan}* 인수에는 `cf marketplace` 명령의 출력을 사용하십시오.
    - 작성할 {service-instance-name}을 지정하십시오. 많은 애플리케이션에서 이 이름은 `manifest.yml` 또는 애플리케이션에 대한 다른 구성 파일에 지정되어 있습니다.

- **cf delete-service**: 서비스 인스턴스를 삭제합니다.

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    더 이상 필요하지 않은 서비스를 삭제하여 필요하지 않은 {{site.data.keyword.cloud_notm}}의 리소스를 해제합니다.

## 애플리케이션을 배치하고 실행하기 위한 명령

{{site.data.keyword.cloud_notm}}에서 애플리케이션을 배치하고 실행하기 위한 일반 `cf` 명령:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): {{site.data.keyword.watson}} API의 위치를 지정합니다.

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login): {{site.data.keyword.ibmid}} 및 비밀번호를 사용하여 {{site.data.keyword.cloud_notm}}에 로그인합니다.

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): {{site.data.keyword.cloud_notm}} 카탈로그의 연결할 수 있는 서비스를 나열합니다.

  ```bash
  cf marketplace
  ```
  {: pre}

  출력에는 각 서비스의 이름과 서비스가 사용 가능한 플랜의 목록이 포함됩니다. {{site.data.keyword.cloud_notm}}에서 실행 중인 애플리케이션에 대한 서비스의 인스턴스를 작성할 때 이 정보가 필요합니다.

  서비스의 이름을 알고 있는 경우 `-s` 옵션을 사용하십시오. 예를 들어, 다음과 같습니다.

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): 애플리케이션을 {{site.data.keyword.cloud_notm}}에 배치하거나 업데이트합니다. 출력에는 애플리케이션에 액세스하기 위한 URL이 포함됩니다.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  이 명령은 일반적으로 `manifest.yml` 또는 다른 구성 파일을 사용하여 푸시될 파일(예: Java의 경우`.war` 파일 또는 Node.js의 경우 `.zip` 파일)을 찾습니다. 그러나 `-p` 옵션을 사용하여 애플리케이션 파일의 경로 이름을 지정할 수 있습니다.

## 기존 애플리케이션에 대한 명령

{{site.data.keyword.cloud_notm}}에서 기존 애플리케이션을 관리하는 데 유용한 `cf` 명령:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps): 현재 영역에 배치된 애플리케이션을 나열합니다.

  ```bash
  cf apps
  ```
  {: pre}

  애플리케이션의 상태에 대한 자세한 정보를 보려면 애플리케이션 이름을 포함시키십시오.

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: {{site.data.keyword.cloud_notm}}에서 애플리케이션을 다시 로드하고 다시 시작합니다.

  ```bash
  cf restage {application-name}
  ```
  {: pre}

이 명령은 {{site.data.keyword.cloud_notm}}에 존재하는 대로 애플리케이션을 다시 시작합니다. 다시 시작하기 전에 최신 버전의 애플리케이션을 {{site.data.keyword.cloud_notm}}에 업로드하려면 대신 `cf push` 명령을 사용하십시오.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete): 애플리케이션을 삭제합니다.

  ```bash
  cf delete application-name
  ```
  {: pre}

  앱을 삭제하여 다른 애플리케이션에 사용할 수 있도록 {{site.data.keyword.cloud_notm}} 메모리 및 할당량을 해제합니다.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service): 서비스를 애플리케이션에 바인드합니다.

  일반적으로 `cf push` 명령을 사용하여 서비스 인스턴스를 사용하는 애플리케이션에 해당 서비스 인스턴스를 바인드합니다. `cf bind-service` 명령을 사용하여 기존 서비스 인스턴스를 기존 애플리케이션에 바인드할 수 있습니다.

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  그런 다음 `cf restage` 또는 `cf push`를 실행하여 애플리케이션을 다시 시작하십시오.

- **cf unbind-service**: 애플리케이션과 서비스 간의 연관을 제거합니다.

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  변경사항을 반영하기 위해 `cf restage` 또는 `cf push` 명령을 실행해야 할 수도 있습니다.

  이 명령은 `VCAP_SERVICES` 환경 변수에서 인증 신임 정보를 제거합니다. 서비스 인스턴스 또는 애플리케이션을 제거하려면 `cf delete-service` 또는 `cf delete`를 사용하십시오.
  {: tip}
