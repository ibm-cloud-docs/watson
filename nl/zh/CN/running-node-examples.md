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

# 从 Node.js SDK 运行示例

{{site.data.keyword.watson}} Node.js 软件开发包 (SDK) 包含每个服务的示例代码。
{: shortdesc}

## 开始之前

1.  创建 {{site.data.keyword.Bluemix}} 帐户，或者使用现有帐户。[免费注册 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}。
1.  获取要运行的 {{site.data.keyword.watson}} 服务的服务凭证。有关详细信息，请参阅 [{{site.data.keyword.watson}} 服务的服务凭证](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually)。
1.  确保安装了 [Node.js 运行时 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://nodejs.org/#download){: new_window}，包括 [npm ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.npmjs.com/){: new_window} 软件包管理器。确保安装后在 `PATH` 环境变量上包含命令。

## 更新和运行示例

1.  保存要运行的 {{site.data.keyword.watson}} 服务的示例代码的副本。
    1.  转至 watson-developer-cloud [Node SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window} 中的 `/examples` 目录。
    1.  在本地保存示例中的代码。我们将使用 `personality_insights.v3.js`。
1.  切换到保存代码的目录并安装 Node.js SDK：

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  更新示例代码以使用服务凭证（api_key 或用户名和密码）：

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    您可能必须进行其他更改。例如，在此添加足够的文本以供 {{site.data.keyword.personalityinsightsshort}} 进行分析。

1.  发出命令 `node {example_application.vn}.js` 以运行代码。例如，

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    响应包含来自服务的信息。例如，使用 {{site.data.keyword.personalityinsightsshort}} 服务，您将看到类似于以下截断的示例的输出：

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
