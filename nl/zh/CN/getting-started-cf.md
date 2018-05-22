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

# Cloud Foundry 命令行界面

您可以使用 `cf` 命令行界面 (CLI) 工具来处理本地系统上的 {{site.data.keyword.cloud}} 应用程序。该工具提供了丰富的界面，实现全面的开发体验。
{: shortdesc}

有关使用 Cloud Foundry 工具进行应用程序开发的更多信息，请参阅 [Cloud Foundry 如何用于 {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) 和 [Cloud Foundry 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.cloudfoundry.org/){: new_window}。

## 安装 cf 命令行界面

您可以在 [Cloud Foundry GitHub 存储库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/cli#downloads){: new_window} 的 **Downloads** 部分中，找到最新版本的工具。您将看到两种类型的软件包：

- **Installers** 集成平台软件包管理工具或已安装的软件跟踪系统，从而将工具安装到其“官方”位置。
- **Binaries** 提供归档文件，可安装到任何位置。

确保安装工具后在 `PATH` 环境变量上包含命令。
{: tip}

## 用于服务实例的命令

用于管理 {{site.data.keyword.cloud_notm}} 中的现有 {{site.data.keyword.watson}} 服务实例的有用 `cf` 命令：

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services)：列出当前空间中的服务实例。

  ```bash
  cf services
  ```
  {: pre}

  如果您知道服务的名称，可使用 `cf service` 命令以查看有关服务的信息。例如，

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service)：创建可绑定到在 {{site.data.keyword.cloud_notm}} 中运行的应用程序的服务实例：

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - 对于 *{service-name}* 和 *{service-plan}* 自变量，使用 `cf marketplace` 命令的输出。
    - 指定要创建的 {service-instance-name}。对于众多应用程序，在应用程序的 `manifest.yml` 或其他配置文件中指定名称。

- **cf delete-service**：删除服务实例：

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    通过删除不再需要的服务，可释放 {{site.data.keyword.cloud_notm}} 不需要的资源。

## 用于部署和运行应用程序的命令

用于在 {{site.data.keyword.cloud_notm}} 中部署和运行应用程序的常用 `cf` 命令：

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api)：指定 {{site.data.keyword.watson}} API 的位置：

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login)。使用您的 {{site.data.keyword.ibmid}}和密码登录到 {{site.data.keyword.cloud_notm}}：

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace)：列出可连接到的 {{site.data.keyword.cloud_notm}} 目录中的服务：

  ```bash
  cf marketplace
  ```
  {: pre}

  输出包含每个服务的名称以及服务可用的套餐列表。为 {{site.data.keyword.cloud_notm}} 中运行的应用程序创建服务实例时，需要此信息。

  如果您知道服务的名称，请使用 `-s` 选项。例如，

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push)：将应用程序部署或更新到 {{site.data.keyword.cloud_notm}}。输出包含用于访问应用程序的 URL。

  ```bash
  cf push {application-name}
  ```
  {: pre}

  此命令使用 `manifest.yml` 或其他配置文件来查找要推送的文件（例如，用于 Java 的 `.war` 文件或用于 Node.js 的 `.zip` 文件）。但是，您可以使用 `-p` 选项指定应用程序文件的路径名。

## 用于现有应用程序的命令

用于管理 {{site.data.keyword.cloud_notm}} 中现有应用程序的有用 `cf` 命令：

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps)：列出在当前空间中部署的应用程序。

  ```bash
  cf apps
  ```
  {: pre}

  包含应用程序名称以查看有关应用程序的运行状况和状态的详细信息：

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**：在 {{site.data.keyword.cloud_notm}} 中重新装入和重新启动应用程序：

  ```bash
  cf restage {application-name}
  ```
  {: pre}

此命令将重新启动应用程序，就如同位于 {{site.data.keyword.cloud_notm}} 中一样。要在重新启动前将最新版本的应用程序上载到 {{site.data.keyword.cloud_notm}}，请改为使用 `cf push` 命令。

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete)：删除应用程序：

  ```bash
  cf delete application-name
  ```
  {: pre}

  通过删除应用程序，释放 {{site.data.keyword.cloud_notm}} 内存和配额以供其他应用程序使用。

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service)：将服务绑定到应用程序。

  通常，使用 `cf push` 命令将服务的实例绑定到使用其的应用程序。可以使用 `cf bind-service` 命令将现有服务实例绑定到现有应用程序：

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  然后，发出 `cf restage` 或 `cf push` 以重新启动应用程序。

- **cf unbind-service**：除去应用程序和服务之间的关联：

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  您可能需要发出 `cf restage` 或 `cf push` 命令以反映更改。

  此命令将从 `VCAP_SERVICES` 环境变量中除去认证凭证。使用 `cf delete-service` 或 `cf delete` 以除去服务实例或应用程序。
  {: tip}
