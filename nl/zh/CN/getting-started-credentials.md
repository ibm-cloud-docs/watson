---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# 使用 Cloud Foundry 服务凭证进行认证
{: #creating-credentials}

{{site.data.keyword.ibmwatson}} 服务的 Cloud Foundry 服务凭证向 {{site.data.keyword.cloud}} 中的服务提供认证。服务凭证使用 HTTP 基本认证对特定 {{site.data.keyword.watson}} 服务进行认证。
{: shortdesc}

在资源组中创建或者从 Cloud Foundry 迁移到资源组的 {{site.data.keyword.watson}} 服务使用 IAM 认证。缺省情况下，所有新的 {{site.data.keyword.watson}} 服务使用 IAM 认证。有关资源组和 IAM 认证的更多信息，请参阅[使用 IAM 令牌进行认证](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually)。
{: note}

## 获取 Cloud Foundry 服务凭证
{: #getting-credentials-manually}

要通过使用 Cloud Foundry 服务凭证访问 API 方法，您必须先收集凭证。您可以从 {{site.data.keyword.cloud_notm}} Web 界面访问服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
1.  转至[资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/dashboard){: new_window}。
1.  选择服务实例。
1.  单击**显示凭证**，以查看凭证的**用户名**、**密码**以及 **Url**。

## 更新 Cloud Foundry 服务凭证
{: #existing-svcs}

您可以从服务仪表板更新现有服务实例的服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
1.  转至[资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/dashboard){: new_window}。
1.  选择页面左侧导航菜单上的**服务凭证**。
1.  使用菜单和图标以删除现有凭证或者添加新凭证。

针对任何更改，确保在应用程序中更新凭证。

## {{site.data.keyword.Bluemix_dedicated_notm}} 中的凭证

在 [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated) 实例中，实施服务磁贴，以便组织中的每个服务只允许有一个磁贴。

执行以下步骤以在自己的 {{site.data.keyword.Bluemix_dedicated_notm}} 工作空间引用服务：

1.  针对要使用的服务的引用磁贴创建一组凭证。
1.  创建使用这些凭证的用户提供的定制服务：

    1.  使用 Web 界面来选择要使用的服务的磁贴，然后针对服务创建一组凭证。凭证将采用 JSON 格式创建，并且包含服务端点 URL、用户名以及密码。
    1.  使用 `ibmcloud cf cups`（创建用户提供的服务）命令以创建绑定到这些凭证的定制服务。例如，

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      在给定这些凭证的情况下，创建名称为 `Personality-Insights-Std` 的由用户提供的定制服务实例：

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      确保将用户名和密码指定为用双引号括起且用冒号分隔的单个字符串。另外，用双引号将用户名和密码字符串括起来，并使用反斜杠进行转义。
      {: tip}

### 将应用程序与 {{site.data.keyword.Bluemix_dedicated_notm}} 一起使用

GitHub 上的某些 {{site.data.keyword.watson}} 样本应用程序和入门模板工具包包含一个**部署到 {{site.data.keyword.cloud_notm}}** 按钮。这些按钮在 {{site.data.keyword.Bluemix_dedicated_notm}} 安装中无效，因为它们引用公共 {{site.data.keyword.cloud_notm}} 环境。

要在 {{site.data.keyword.Bluemix_dedicated_notm}} 环境中使用样本应用程序和入门模板工具包，请遵循以下步骤：

1.  在应用程序的指示信息中，确保您创建的 `manifest.yml` 文件使用您通过 `ibmcloud cf cups` 命令创建的服务的名称。
1.  在这些相同指示信息中，不要运行 `ibmcloud cf create-service` 命令，而是使用 `ibmcloud cf cups` 命令。但是，如果创建了用户提供的定制服务实例，那么无需再次创建即可将其与样本应用程序或入门模板工具包一起使用。
