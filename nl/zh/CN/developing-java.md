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

# 使用 Java 开发 {{site.data.keyword.watson}} 应用程序

为演示如何使用其 REST API，每个 {{site.data.keyword.ibmwatson}} 服务在 {{site.data.keyword.watson}} GitHub 项目中提供基于 Web 的样本 Java 应用程序。
{: shortdesc}

## 开始之前

- 在 {{site.data.keyword.cloud_notm}} 上创建帐户，或使用现有帐户。[免费注册 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}。
- 安装 [Cloud Foundry ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/cli#downloads){: new_window} 命令行客户机。如果已安装，确保版本为最新。
- 下载并安装 [Apache Maven ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://maven.apache.org/download.cgi){: new_window}。
- 安装 [IBM WebSphere Liberty 概要文件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developer.ibm.com/wasdev/downloads/){: new_window}。您可以仅下载 WebSphere Liberty 概要文件的运行时版本，或者下载作为 Eclipse IDE 插件的版本。另请安装并配置 *Java JDK* 或 *Eclipse IDE*。

## 获取源代码
访问 [{{site.data.keyword.watson}} GitHub ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud){: new_window} 项目以查看适用于 {{site.data.keyword.watson}} 的样本 Java 应用程序。然后，使用 GitHub 在本地克隆存储库。

## 本地安装
如果要修改应用程序或将其用作构建自己的应用程序的基础，请在本地进行安装。然后，可以将修改的应用程序版本部署到 {{site.data.keyword.cloud_notm}}。

### 设置 {{site.data.keyword.watson}} 服务

1.  在命令行中，转至克隆的应用程序的本地项目目录。
1.  登录到 {{site.data.keyword.cloud_notm}}：

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  查找服务和套餐的名称。如果样本应用程序的 `manifest.yml` 文件包含 `declared-services` 部分，请记录 `label` 和 `plan`。否则，使用 `cf marketplace` 命令来获取服务和套餐的名称：

    ```bash
    cf marketplace
    ```
    {: pre}

1.  在 {{site.data.keyword.cloud_notm}} 中创建服务的实例：`cf create-service {service-name} {service-plan} {service-instance-name}`。对于 service-name 和 service-plan，使用来自清单文件或 `cf marketplace` 命令的信息。

    例如，发出以下命令以针对 {{site.data.keyword.personalityinsightsshort}} 服务创建服务实例：

    ```bash
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### 配置应用程序环境

1.  创建 `cf create-service-key {service_instance} {service_key}` 格式的服务密钥。例如：


    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  使用命令 `cf service-key {service_instance} {service_key}` 从服务密钥检索凭证。例如：


    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    此命令的输出是 JSON 对象，如以下示例中所示：

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }

    ```
    {: codeblock}
1.  将服务密钥中的值复制到应用程序工作目录中应用程序的源文件：

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### 构建和运行应用程序

这些指示信息使用 WebSphere Liberty 概要文件的运行时版本。如果使用 Eclipse 插件，那么需要修改步骤。
{: tip}

1.  通过从应用程序的工作目录运行以下 Maven 命令来构建应用程序：

    ```java
    mvn clean package
    ```
    {: pre}

1.  发出 WebSphere Liberty 概要文件 `server create` 命令以针对应用程序创建服务器。指定为命令自变量的服务器的名称是任意值，但它在本地 WebSphere Liberty 概要文件环境中必须唯一。

    ```bash
    server create server-name
    ```
    {: pre}

1.  将应用程序的 `.war` 文件复制到目录 `wlp-installation/usr/servers/server-name/dropins`，其中 *wlp-installation* 是 WebSphere Liberty 概要文件的本地安装的基本目录，*server-name* 是先前步骤中针对服务器指定的名称。
1.  启动 WebSphere Liberty 概要文件中的服务器。指定应用程序的服务器名称作为命令的自变量。

    ```bash
    server start server-name
    ```
    {: pre}

1.  将浏览器指向 http://localhost:9080/webApp 以试用应用程序。在 `app.js` 文件中指定端口。

    在文件 `wlp-installation/usr/servers/server-name/server.xml` 中指定端口。要更改端口，请更改 `server.xml` 中的值，然后停止并重新启动服务器。

### 故障诊断

如果服务器未能正常启动或者无法响应，请检查目录 `wlp-installation/usr/servers/server-name/logs` 中的日志文件以确定原因。

## 部署到 {{site.data.keyword.cloud_notm}}

您可以使用 Cloud Foundry 将本地版本的应用程序部署到 {{site.data.keyword.cloud_notm}}。

1.  在项目根目录中，打开 manifest.yml 文件：
    1. 在 `manifest.yml` 文件的 `applications` 部分中，将 name 值更改为针对您的应用程序版本的唯一名称。
    1. 在 `services` 部分中，指定针对应用程序创建的 {{site.data.keyword.watson}} 服务实例的名称。如果记不住服务名称，请使用 `cf services` 命令列出服务。

        以下示例显示修改后的 `manifest.yml` 文件：

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

1.  登录到 {{site.data.keyword.cloud_notm}}：

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  将应用程序推送至 {{site.data.keyword.cloud_notm}}，然后指定来自 `manifest.yml` 文件的应用程序名称。例如，

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    将浏览器指向在命令输出中指定的 URL。
