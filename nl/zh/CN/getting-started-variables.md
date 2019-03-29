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

# VCAP\_SERVICES 环境变量
{: #vcapServices}

`VCAP_SERVICES` 环境变量包含可用于与 {{site.data.keyword.cloud}} 中的服务实例进行交互的信息。在将服务实例绑定到应用程序时，将设置此环境变量中的字段。
{: shortdesc}

应用程序需要 URL 和服务凭证以向 {{site.data.keyword.watson}} 服务进行认证。正在运行的应用程序通常在向其绑定服务后读取这些环境变量，以抽取必需的名称/值对。您可以将资源组或 Cloud Foundry 服务绑定到应用程序，但是资源组服务需要服务别名。

## IAM 服务的服务别名
{: #vcapIAMAlias}

您可以通过向服务提供别名，将 `VCAP_SERVICES` 环境变量用于资源组服务。例如，在 {{site.data.keyword.cloud_notm}} 命令行界面中使用以下命令，为 `my-personality-insights-service` 服务创建别名 `personality-insights-service`。

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

然后，您可以将应用程序绑定到服务别名。正在运行的应用程序通常在向其绑定服务实例后读取这些环境变量，以抽取必需的名称/值对。

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## 示例
{: #gs-variables-vcapIAMAliasExample}

此示例显示绑定到应用程序的 {{site.data.keyword.personalityinsightsshort}} 服务的 `VCAP_SERVICES` 环境变量。该服务实例使用 Cloud Foundry 认证。

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

下表描述 `VCAP_SERVICES` 环境变量的内容。

|项       |描述                                                                                       |
|----------|--------------------------------------------------------------------------------------------|
|password |来自用于连接到服务的 HTTP 基本认证凭证的密码                                               |
|url      |服务的连接 URL                                                                             |
|username |来自用于连接到服务的 HTTP 基本认证凭证的用户名                                             |
|label    |与 {{site.data.keyword.cloud_notm}} 中的服务相关联的名称                                                            |
|name     |服务实例的名称                                                                             |
|plan     |适用于 {{site.data.keyword.cloud_notm}} 中的服务的套餐                                                              |
|tags     |有关服务的其他信息                                                                         |

## 查看环境变量
{: #gs-variables-vcapView}

您可以从 {{site.data.keyword.cloud_notm}} 命令行界面 (CLI) 或者 Web 界面检索有关该变量的信息。

- 通过 {{site.data.keyword.cloud_notm}} CLI：在创建服务后调用 Cloud Foundry `env` 命令，并将其绑定到应用程序：

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- 从 {{site.data.keyword.cloud_notm}} 界面：应用程序的摘要页面提供了有关环境变量的信息。
