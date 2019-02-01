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

# Watson 服务的服务凭证

{{site.data.keyword.ibmwatson}} 服务的服务凭证向 {{site.data.keyword.cloud}} 中的服务提供认证。一组凭证只能用于为其创建这些凭证的服务。
{: shortdesc}

根据使用的编程模型不同，可通过不同方式使用服务凭证。有关详细信息，请参阅 [{{site.data.keyword.watson}} 服务的编程模型](/docs/services/watson/getting-started-develop.html)。

服务凭证是特定 {{site.data.keyword.watson}} 服务的 HTTP 基本认证凭证。

## 手动获取凭证
{: #getting-credentials-manually}

您可以从 {{site.data.keyword.cloud_notm}} Web 界面查找服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}。
1.  转至现有服务。（[带我前往该处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/developer/watson/existing-services){: new_window}）：
    1.  选择左上角的菜单，然后选择 **{{site.data.keyword.watson}}** 以转至 {{site.data.keyword.watson}} 控制台。
    1.  从 **Watson 服务**中选择**现有服务**以查看服务和项目列表。
1.  查看凭证：
    - 如果服务属于某个项目，请单击包含该服务的项目的名称。从项目详细信息页面的**凭证**部分查看凭证。
    - 如果服务不属于项目，请单击要查看的服务名称，然后选择**服务凭证**。

## 更新服务凭证
{: #existing-svcs}

您可以从服务仪表板更新现有服务实例的服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}。
1.  转至现有服务。（[带我前往该处 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/developer/watson/existing-services){: new_window}）：
    1.  选择左上角的菜单，然后选择 **{{site.data.keyword.watson}}** 以转至 {{site.data.keyword.watson}} 控制台。
    1.  从 **Watson 服务**中选择**现有服务**以查看服务列表。
1.  选择要查看或更新的服务，然后选择**服务凭证**。
1.  删除或添加凭证：
    - 要删除凭证，请选择凭证，然后选择**删除凭证**。
    - 要添加凭证，请单击**新建凭证**，然后单击“添加新凭证”窗口中的**添加**。查看并复制新凭证。

确保在应用程序中使用新凭证：

- 如果应用程序以编程方式获取其凭证，请停止并重新启动应用程序。
- 如果凭证嵌入在应用程序代码中，请更新代码并重新启动应用程序。

有关如何在停机时间最短的情况下使用新凭证更新应用程序的详细信息，请参阅[更新应用程序](/docs/manageapps/updapps.html)。

## {{site.data.keyword.Bluemix_dedicated_notm}} 中的凭证

在 [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) 实例中，实施服务磁贴，以便组织中的每个服务只允许有一个磁贴。

执行以下步骤以在自己的 {{site.data.keyword.Bluemix_dedicated_notm}} 工作空间引用服务：

1.  针对要使用的服务的引用磁贴创建一组凭证。
1.  创建使用这些凭证的用户提供的定制服务：

    1.  使用 {{site.data.keyword.cloud_notm}} 界面来选择要使用的服务的磁贴，然后针对服务创建一组凭证。这些凭证采用 JSON 格式，并且包含单独的 URL 字段（用于与服务通信）以及用户名和密码字段（与此服务一起使用）。
    1.  使用 `cf cups`（创建用户提供的服务）命令以创建绑定到这些凭证的定制服务。例如，假定以下凭证：

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      创建名为 `Personality-Insights-Std` 的用户提供的定制服务实例：

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      确保将用户名和密码指定为用双引号括起且用冒号分隔的单个字符串。另外，用双引号将用户名和密码字符串括起来，并使用反斜杠进行转义。
      {: tip}

### 将应用程序与 {{site.data.keyword.Bluemix_dedicated_notm}} 一起使用

GitHub 上的某些 {{site.data.keyword.watson}} 样本应用程序和入门模板工具包包含一个**部署到 {{site.data.keyword.cloud_notm}}** 按钮。这些按钮在 {{site.data.keyword.Bluemix_dedicated_notm}} 安装中无效，因为它们引用公共 {{site.data.keyword.cloud_notm}} 环境。

要在 {{site.data.keyword.Bluemix_dedicated_notm}} 环境中使用样本应用程序和入门模板工具包：

1.  在应用程序指令中，确保创建的 `manifest.yml` 文件使用通过 `cf cups` 命令创建的服务的名称。
1.  在这些相同指令中，代替执行 `cf create-service` 命令，而是使用 `cf cups` 命令。但是，如果创建了用户提供的定制服务实例，那么无需再次创建即可将其与样本应用程序或入门模板工具包一起使用。
