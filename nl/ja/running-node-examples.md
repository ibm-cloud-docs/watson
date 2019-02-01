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

# Node.js SDK からのサンプルの実行

{{site.data.keyword.watson}} Node.js Software Development Kit (SDK) には、各サービスのサンプル・コードが含まれています。
{: shortdesc}

## 始める前に

1.  {{site.data.keyword.Bluemix}} アカウントを作成するか、既存のアカウントを使用します。[こちらから無料でお申し込みいただけます ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}。
1.  実行する {{site.data.keyword.watson}} サービスのサービス資格情報を取得します。詳細については、[{{site.data.keyword.watson}} サービスのサービス資格情報](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually)を参照してください。
1.  [Node.js ランタイム ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://nodejs.org/#download){: new_window} ([npm ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.npmjs.com/){: new_window} パッケージ・マネージャーも含む) がインストールされていることを確認します。インストール後に、必ず `PATH` 環境変数にコマンドを含めてください。

## サンプルの更新と実行

1.  実行する {{site.data.keyword.watson}} サービスのサンプル・コードのコピーを保存します。
    1.  watson-developer-cloud [Node SDK ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window} の `/examples` ディレクトリーに移動します。
    1.  サンプルのコードをローカル環境に保存します。ここでは、`personality_insights.v3.js` を使用します。
1.  コードを保存したディレクトリーに移動し、Node.js SDK をインストールします。

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  サービス資格情報 (api_key またはユーザー名とパスワード) を使用するようにサンプル・コードを更新します。

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    場合によっては、他の変更も必要です。例えば、{{site.data.keyword.personalityinsightsshort}} で分析するための十分なテキストをここに追加します。

1.  `node {example_application.vn}.js` コマンドを実行して、コードを実行します。以下に例を示します。

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    応答には、サービスからの情報が組み込まれます。例えば、{{site.data.keyword.personalityinsightsshort}} サービスの場合は、以下の例のような出力になります (一部を切り捨てています)。

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
