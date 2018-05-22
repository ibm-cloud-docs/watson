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

# 從 Node.js SDK 執行範例

{{site.data.keyword.watson}} Node.js 軟體開發套件 (SDK) 包含每個服務的程式碼範例。
{: shortdesc}

## 開始之前

1.  建立 {{site.data.keyword.Bluemix}} 帳戶，或使用現有帳戶。[免費註冊 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}。
1.  取得您要執行之 {{site.data.keyword.watson}} 服務的服務認證。如需詳細資料，請參閱 [{{site.data.keyword.watson}} 服務的服務認證](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually)。
1.  確定您已安裝 [Node.js 運行環境 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://nodejs.org/#download){: new_window}，包括 [npm ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.npmjs.com/){: new_window} 套件管理程式。請確定在安裝之後，在 `PATH` 環境變數上包含指令。

## 更新並執行範例

1.  儲存您要執行之 {{site.data.keyword.watson}} 服務的程式碼範例副本。
    1.  在 watson-developer-cloud [Node SDK ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window} 前往 `/examples` 目錄。
    1.  在本端環境中儲存範例的程式碼。我們將使用 `personality_insights.v3.js`。
1.  切換至您在其中儲存程式碼的目錄，並安裝 Node.js SDK：

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  更新程式碼範例，以使用您的服務認證（api_key 或使用者名稱及密碼）：

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    您可能需要進行其他變更。例如，在此處，您可以為 {{site.data.keyword.personalityinsightsshort}} 新增足夠的文字以進行分析。

1.  發出 `node {example_application.vn}.js` 指令來執行程式碼。例如，

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    回應包括來自服務的資訊。例如，使用 {{site.data.keyword.personalityinsightsshort}} 服務，您會看到與此截斷範例類似的輸出：

    ```json
    {
      "word_count": 224,
      "word_count_message": "There were 224 words in the input. We need a minimum of 600, preferably 1,200 or more, to compute statistically significant estimates",
      "processed_lang": "en",
      "personality": [
      {
        "trait_id": "big5_openness",
        "name": "Openness",
        "category": "personality",
        "percentile": 0.9015311332932557,
        "children": [. . .]
      },
      . . .
      ],
      "needs": [. . .],
      "values": [. . .],
      "consumption_preferences": [. . .]
    }
    ```
    {: codeblock}
