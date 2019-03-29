---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM tokens,IAM authentication

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

# 使用 IAM 令牌进行认证
{: #iam}

您使用 {{site.data.keyword.cloud}} Identity and Access Management (IAM) 令牌对 {{site.data.keyword.ibmwatson}} 服务发出经认证的请求，而无需在每个调用中嵌入服务凭证。IAM 认证使用访问令牌进行认证，您可以通过使用 API 密钥发送请求来获取访问令牌。
{: shortdesc}

在资源组中创建或者从 Cloud Foundry 迁移到资源组的 {{site.data.keyword.watson}} 服务使用 IAM 认证。缺省情况下，所有新的 {{site.data.keyword.watson}} 服务使用 IAM 认证。
{: note}

## 获取 IAM 服务凭证
{: #iam-getting-credentials-manually}

要通过使用 IAM 服务凭证访问 API 方法，您必须先收集凭证。您可以从 {{site.data.keyword.cloud_notm}} Web 界面访问服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
1.  转至[资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/dashboard){: new_window}。
1.  选择服务实例。
1.  单击**显示凭证**，以查看凭证的 **API 密钥**和 **Url**。
    1.  要查看凭证的令牌，请选择页面左侧导航菜单上的**服务凭证**。
    1.  选择**查看凭证**以查看凭证的完整 API 密钥和令牌信息。

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Auto generated apikey during resource-key operation for...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## 更新 IAM 服务凭证
{: #update-existing-svcs}

您可以从服务仪表板更新现有服务实例的服务凭证。

1.  登录到 [{{site.data.keyword.cloud_notm}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}){: new_window}。
1.  转至[资源列表 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/dashboard){: new_window}。
1.  选择页面左侧导航菜单上的**服务凭证**。
1.  使用菜单和图标以删除现有凭证或者添加新凭证。

针对任何更改，确保在应用程序中更新凭证。

## 使用 Watson 服务 API 密钥获取 IAM 令牌
{: #iamtoken}

使用为服务实例生成的 API 密钥，您可以访问 {{site.data.keyword.ibmwatson_notm}} 服务 API。您使用 API 密钥来生成 IAM 访问令牌。如果您正在开发的应用程序需要使用其他 {{site.data.keyword.cloud_notm}} 服务，那么您也会使用此流程。

有关安全地使用 API 密钥的更多信息，请参阅 [IAM 服务 API 密钥](/docs/services/watson?topic=watson-api-key-bp)。
{: tip}

以下 curl 命令使用 `POST identity/token` 方法通过传递 API 密钥来生成 IAM 访问令牌。`Content-Type` 头指示数据进行了 URL 编码。`data-urlencode` 参数将 URL 编码的数据传递到此方法。

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

使用认证来生成访问令牌。刷新令牌时，请使用相同的认证。

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"

```
{: pre}

以下样本显示期望的响应：

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: pre}

## 使用令牌进行认证
{: #use_token}

您使用 `POST identity/token` 方法来生成 IAM 访问令牌。然后，您可以使用该令牌来发出经认证的 API 调用。API 密钥与特定服务实例相关联。使用 API 密钥生成的令牌仅可以用于对该服务实例的调用。

{{site.data.keyword.watson}} 服务的典型 curl 调用采用以下格式，其中 `{token}` 是您生成的 IAM 访问令牌：

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

或者，您可以为令牌添加前缀 `Authorization: Bearer `（例如 `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`），并将其保存到文件。然后，使用以下命令：

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## 刷新令牌
{: #refresh_token}

您可以使用 `POST identity/token` 方法生成的 IAM 刷新令牌来延长访问令牌的寿命。以下命令刷新现有访问令牌。在此命令中，`{refresh-token}` 是您生成的 IAM 刷新令牌。

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

刷新令牌时，请使用您用于创建原始令牌的相同认证。
{: important}
