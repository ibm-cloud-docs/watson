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

# VCAP\_SERVICES 環境變數
{: #vcapServices}

`VCAP_SERVICES` 環境變數包含的資訊，可以用來在 {{site.data.keyword.Bluemix}} 中與服務實例互動。當您將服務連結至應用程式時，會設定此環境變數中的欄位。
{: shortdesc}

您的應用程式需要 URL 和 HTTP 基本鑑別服務認證，才能向 {{site.data.keyword.watson}} 服務進行鑑別。

執行中的應用程式通常會在服務連結到它之後讀取這些環境變數，以擷取所需的名稱/值配對。如需變數的相關資訊，請參閱[部署應用程式](/docs/manageapps/depapps.html#app_env)，並搜尋「環境變數」。

## 範例
這個範例顯示連結至應用程式的 {{site.data.keyword.personalityinsightsshort}} 服務的 `VCAP_SERVICES` 環境變數：

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

| 項目| 說明|
|----------|--------------------------------------------------------------------------------------------|
| password | 用來連接至服務的 HTTP 基本鑑別認證的密碼|
| url      | 服務的連線 URL|
| username | 用來連接至服務的 HTTP 基本鑑別認證的使用者名稱|
| label    | 與 {{site.data.keyword.cloud_notm}} 中的服務相關聯的名稱|
| name     | 服務實例的名稱|
| plan     | 適用於 {{site.data.keyword.cloud_notm}} 中的服務的方案|
| tags     | 服務的其他相關資訊|

## 檢視環境變數
您可以從 Cloud Foundry 工具或從 {{site.data.keyword.cloud_notm}} 介面中擷取變數的相關資訊。

- 使用 Cloud Foundry 指令行工具：在建立服務並將它連結至應用程式之後，請使用 `cf env` 指令：

    ```bash
    cf env {application-name}
    ```
    {: pre}

- 從 {{site.data.keyword.cloud_notm}} 介面：可以從應用程式的摘要頁面取得環境變數的相關資訊。
