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

# VCAP\_SERVICES 环境变量
{: #vcapServices}

`VCAP_SERVICES` 环境变量包含可用于与 {{site.data.keyword.Bluemix}} 中的服务实例进行交互的信息。在将服务绑定到应用程序时，设置此环境变量中的字段。
{: shortdesc}

应用程序需要 URL 和 HTTP 基本认证服务凭证以向 {{site.data.keyword.watson}} 服务进行认证。

正在运行的应用程序通常在向其绑定服务后读取这些环境变量，以抽取必需的名称/值对。有关变量的信息，请参阅[部署应用程序](/docs/manageapps/depapps.html#app_env)并搜索“环境变量”。

## 示例
此示例显示绑定到应用程序的 {{site.data.keyword.personalityinsightsshort}} 服务的 `VCAP_SERVICES` 环境变量：

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

| 项       | 描述                                                                                       |
|----------|--------------------------------------------------------------------------------------------|
| password | 来自用于连接到服务的 HTTP 基本认证凭证的密码                                               |
| url      | 服务的连接 URL                                                                             |
| username | 来自用于连接到服务的 HTTP 基本认证凭证的用户名                                             |
| label    | 与 {{site.data.keyword.cloud_notm}} 中的服务相关联的名称                                                            |
| name     | 服务实例的名称                                                                             |
| plan     | 适用于 {{site.data.keyword.cloud_notm}} 中的服务的套餐                                                              |
| tags     | 有关服务的其他信息                                                                         |

## 查看环境变量
您可以从 Cloud Foundry 工具或者在 {{site.data.keyword.cloud_notm}} 界面中检索有关变量的信息。

- 使用 Cloud Foundry 命令行工具：在创建服务并将其绑定到应用程序后，使用 `cf env` 命令：

    ```bash
    cf env {application-name}
    ```
    {: pre}

- 从 {{site.data.keyword.cloud_notm}} 界面：应用程序的摘要页面提供了有关环境变量的信息。
