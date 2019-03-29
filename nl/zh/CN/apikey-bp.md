---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM authentication,service keys,service roles,best practices

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

# IAM 服务 API 密钥
{: #api-key-bp}

{{site.data.keyword.ibmwatson}} 服务 API 密钥用于获取不记名令牌，而不记名令牌用于认证对 {{site.data.keyword.ibmwatson_notm}} 服务的调用。
{: shortdesc}

不同于 API 密钥的标准 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 实施，{{site.data.keyword.ibmwatson_notm}} IAM 密钥是逐个服务进行设置的。您可以向每个 API 密钥而不是向每个服务分配许可权。此实施不同于基于用户的 API 密钥的标准 {{site.data.keyword.cloud_notm}} 方法。对于 {{site.data.keyword.ibmwatson_notm}}，将针对每个服务向用户分配角色。有关常规 {{site.data.keyword.cloud_notm}} IAM 实施的更多信息，请参阅 [API 密钥最佳实践](/docs/services/iam?topic=iam-iamoverview#iamoverview)。

一个 Watson API 密钥只能绑定到单个服务实例。
{: tip}

每次创建 {{site.data.keyword.ibmwatson_notm}} 服务实例时，将自动为`管理者`角色生成一组凭证。您可以在服务实例的**仪表板**页面上使用这些凭证。您可以使用它们来调用服务 API 的*所有*方法。通过使用不同角色创建 API 密钥，您可以限制在使用服务时可执行的操作。

|服务访问角色 |操作描述|
|:-----------------|:-----------------|
| `读取者` | 读取者可以执行服务中的只读操作，例如查看特定于服务的资源。|
| `写入者` |写入者的许可权超过读取者角色，包括创建和编辑特定于服务的资源。|
| `管理者` |管理者的许可权超过写入者角色，可完成服务所定义的特权操作。此外，他们可以创建和编辑特定于服务的资源。|
{: caption="表 1. 用户服务访问角色示例" caption-side="top"}

有关创建额外的 {{site.data.keyword.ibmwatson_notm}} 服务凭证的更多信息，请参阅[更新 IAM 服务凭证](/docs/services/watson?topic=watson-iam#update-existing-svcs)。

## API 密钥最佳实践
{: #api-bp}

API 密钥作为服务的认证方法，必须妥善保存。以下最佳实践帮助您确保 API 密钥的安全，并减少公开暴露凭证而损害您的应用程序的可能性。

-   *通过适用于您所使用功能的角色来使用 API 密钥。*

    创建最终用户应用程序时，请考虑通过`读取者`角色将 API 密钥用于对 `GET` API 方法的所有调用。`读取者`角色不能对服务实例执行任何破坏性或附加功能。

-   *不要直接在代码中嵌入 API 密钥。*

    嵌入代码中的 API 密钥可能会公开给您的用户。不要将 API 密钥嵌入应用程序代码中，而是考虑将其存储在环境变量中或者源代码控制系统外部的文件中。

-   *不要将 API 密钥存储在应用程序的源代码控制系统内的文件中。*

    如果将 API 密钥存储在文件中，请将这些文件放置在应用程序的源代码之外。如果您使用 GitHub 之类的公共源代码管理系统，此做法尤为重要。

-   *定期重新生成 API 密钥。*

    您可以随时创建新凭证。
    1.  从资源列表中，选择服务实例。
    1.  单击页面左侧导航菜单上的**服务凭证**。

     将应用程序迁移到新凭证时，针对想要删除的凭证，不要忘记使用废纸箱图标删除旧的凭证。
     {: tip}
