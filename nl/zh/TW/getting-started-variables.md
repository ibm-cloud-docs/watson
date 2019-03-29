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

# VCAP\_SERVICES 環境變數
{: #vcapServices}

`VCAP_SERVICES` 環境變數包含的資訊，可以用來在 {{site.data.keyword.cloud}} 中與服務實例互動。當您將服務實例連結至應用程式時，會設定此環境變數中的欄位。
{: shortdesc}

您的應用程式需要 URL 及服務認證，才能向 {{site.data.keyword.watson}} 服務進行鑑別。執行中的應用程式通常會在服務連結到它之後讀取這些環境變數，以擷取所需的名稱/值配對。您可以將資源群組或 Cloud Foundry 服務連結至您的應用程式，但資源群組服務需要服務別名。

## IAM 服務的服務別名
{: #vcapIAMAlias}

透過給予服務一個別名，您可以使用 `VCAP_SERVICES` 環境變數及資源群組服務。例如，將下列指令與 {{site.data.keyword.cloud_notm}} 指令行介面一起使用，以建立名為 `my-personality-insights-service` 的服務之 `personality-insights-service` 別名。

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

然後您可以將您的應用程式連結至服務別名。執行中的應用程式通常會在服務實例連結到它之後讀取這些環境變數，以擷取所需的名稱/值配對。

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## 範例
{: #gs-variables-vcapIAMAliasExample}

這個範例顯示連結至應用程式之 {{site.data.keyword.personalityinsightsshort}} 服務的 `VCAP_SERVICES` 環境變數。服務實例使用 Cloud Foundry 鑑別。

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

下表說明 `VCAP_SERVICES` 環境變數的內容。

|項目|說明|
|----------|--------------------------------------------------------------------------------------------|
|password |用來連接至服務的 HTTP 基本鑑別認證的密碼|
|url      |服務的連線 URL|
|username |用來連接至服務的 HTTP 基本鑑別認證的使用者名稱|
|label    |與 {{site.data.keyword.cloud_notm}} 中的服務相關聯的名稱|
|name     |服務實例的名稱|
|plan     |適用於 {{site.data.keyword.cloud_notm}} 中的服務的方案|
|tags     |服務的其他相關資訊|

## 檢視環境變數
{: #gs-variables-vcapView}

您可以從 {{site.data.keyword.cloud_notm}} 指令行介面 (CLI) 或從 Web 介面中擷取變數的相關資訊。

- 使用 {{site.data.keyword.cloud_notm}} CLI：建立服務之後，呼叫 Cloud Foundry `env` 指令，並將其連結至您的應用程式：

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- 從 {{site.data.keyword.cloud_notm}} 介面：可以從應用程式的摘要頁面取得環境變數的相關資訊。
