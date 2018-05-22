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

# 使用 Node.js 开发 {{site.data.keyword.watson}} 应用程序

为演示如何使用其 REST API，每个 {{site.data.keyword.ibmwatson}} 服务在 {{site.data.keyword.watson}} GitHub 项目中提供基于 Web 的样本 Node.js 应用程序。
{: shortdesc}

## 开始之前

- 在 {{site.data.keyword.cloud_notm}} 上创建帐户，或使用现有帐户。[免费注册 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}。您的帐户必须至少具有 1 个应用程序和 1 个服务的空间。
- [Node.js 运行时 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://nodejs.org/#download){: new_window}，包含 [npm](https://www.npmjs.com/){: new_window} 软件包管理器。确保安装后在 `PATH` 环境变量上包含命令。
- [Cloud Foundry ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/cloudfoundry/cli#downloads){: new_window} 命令行客户机。如果先前已安装，请确保您的版本是最新的。

## 获取源代码

访问 [{{site.data.keyword.watson}} Github ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud){: new_window} 项目以查看适用于 {{site.data.keyword.watson}} 的样本 Node.js 应用程序。然后，使用 GitHub 在本地克隆存储库。

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
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### 配置应用程序环境

1.  将 `.env.example` 文件复制到新的 `.env` 文件。
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

1.  将 `password` 和 `username` 值（不含引号）从 JSON 粘贴到 `.env` 文件中的相关变量。例如：


    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### 安装和启动应用程序

1.  将演示应用程序软件包安装到本地 Node.js 运行时环境：

    ```bash
    npm install
    ```
    {: pre}

1.  启动应用程序：

    ```bash
    npm start
    ```
    {: pre}

1.  将浏览器指向 http://localhost:3000 以试用应用程序。在 `app.js` 文件中指定端口。

## 部署到 {{site.data.keyword.cloud_notm}}

您可以使用 Cloud Foundry 将本地版本的应用程序部署到 {{site.data.keyword.cloud_notm}}。

1.  在项目根目录中，打开 manifest.yml 文件：
    1.  在 `manifest.yml` 文件的 `applications` 部分中，将 name 值更改为针对您的应用程序版本的唯一名称。
    1.  在 `services` 部分中，指定针对应用程序创建的 {{site.data.keyword.watson}} 服务实例的名称。如果记不住服务名称，请使用 `cf services` 命令列出服务。

        以下示例显示修改后的 `manifest.yml` 文件：

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
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

1.  将应用程序推送至 {{site.data.keyword.cloud_notm}}，然后指定来自 `manifest.yml` 文件的应用程序名称。例如：

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    将浏览器指向在命令输出中指定的 URL。
