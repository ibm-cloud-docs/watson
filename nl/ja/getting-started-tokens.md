---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Watson tokens,Cloud Foundry

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

# Watson トークン
{: #gs-tokens-watson-tokens}

{{site.data.keyword.watson}} トークンを使用して、呼び出しごとに Cloud Foundry サービス資格情報を組み込むことなく {{site.data.keyword.ibmwatson}} サービスに対して認証された要求を行うアプリケーションを作成します。
{{site.data.keyword.watson}} トークンを取得するには、サービスの Cloud Foundry サービス資格情報を使用する必要があります。詳しくは、[Cloud Foundry サービス資格情報による認証](/docs/services/watson?topic=watson-creating-credentials) を参照してください。取得するのは特定のサービス・インスタンスのトークンであり、そのトークンはそのインスタンスでのみ機能します。
{: shortdesc}

デフォルトで、{{site.data.keyword.cloud_notm}} は、すべての新規サービス・インスタンスに対して Identity and Access Management (IAM) 認証を使用します。ここで説明されているトークンは、Cloud Foundry サービス資格情報を処理します。IAM 認証について詳しくは、[IAM トークンによる認証](/docs/services/watson?topic=watson-iam#iam)を参照してください。
{: important}

トークンを取得し、それをクライアント・アプリケーションに返す認証プロキシーを {{site.data.keyword.cloud}} 内に作成することができます。クライアント・アプリケーションは、そのトークンを使用してサービスを直接呼び出せます。 このプロキシーがない場合は、クライアント・アプリケーションからのサービス資格情報を公開しないようにするために、すべての要求を中間のサーバー・サイド・アプリケーションを介すチャネルで送信する必要がありますが、このプロキシーがあればその必要はありません。

## トークンの取得
{: #gs-tokens-obtain}

サービスのトークンを取得するには、`許可` API の `token` メソッドに HTTP `GET` 要求を送信します。 ホストは、使用しているロケーションやサービスにより異なります。サービスの API のエンドポイントからホストを識別できます。

トークンを取得する対象のサービスを指定するには、メソッドの `url` 照会パラメーターを使用してサービスの基本 URL を渡します。 例えば、以下の curl コマンドは `url` パラメーターを使用して {{site.data.keyword.texttospeechshort}} サービスのトークンを取り出します。

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

要求でサービスの HTTP 基本認証資格情報を渡す方法は、トークンを使用しない呼び出しの場合と同じです。 例えば、以下の curl コマンドは、{{site.data.keyword.texttospeechshort}} サービスのトークンを取得します。

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

`--user` オプションでは、サービスの*ユーザー名*と*パスワード*の連結値を渡します。その資格情報は、{{site.data.keyword.cloud_notm}} から取得することも、サービス・インスタンスにバインドされたアプリケーションの `VCAP_SERVICES` 環境変数から取得することもできます。{{site.data.keyword.cloud_notm}} のログイン ID とパスワードではなく、サービス資格情報を使用してください。

このメソッドは、暗号化ペイロードの 1 キロバイト Base64 エンコードとしてトークンを返します。

トークンの存続時間 (TTL) は 1 時間です。この時間を超えると、そのトークンを使用してサービスとの接続を確立することはできなくなります。トークンですでに確立されていた既存の接続は、タイムアウトの影響を受けません。有効期限が切れたトークンまたは無効なトークンを渡そうとすると、{{site.data.keyword.cloud_notm}} から HTTP `401 Unauthorized` の状況コードが生成されます。 この戻りコードへの応答としてトークンをリフレッシュするようにアプリケーション・コードを準備しておく必要があります。

## トークンの使用
{: #gs-tokens-watson-use}

サービスの HTTP インターフェースにトークンを渡すには、`X-Watson-Authorization-Token` 要求ヘッダー、`watson-token` 照会パラメーター、Cookie のいずれかを使用します。 HTTP 要求ごとにトークンを渡す必要があります。

最初の 2 つの手法を使用した curl の例を以下に示します。

1.  {{site.data.keyword.texttospeechshort}} サービスのトークンを取得し、`token` という名前のファイルに書き込みます。

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  {{site.data.keyword.texttospeechshort}} API の `synthesize` メソッドへの呼び出しで `token` ファイルの内容を渡します。 最初のコマンドは、`X-Watson-Authorization-Token` ヘッダーを使用してトークンを渡します。 2 番目のコマンドは、`watson-token` 照会パラメーターを使用して渡します。 2 つのコマンドは等価です。 どちらのコマンドも、呼び出しの出力を `hello_world.wav` という名前のファイルに書き込みます。

    -   *UNIX または Linux のシェルから*、`token` ファイルの内容を以下のコマンドにリダイレクトします。

        ```bash
        curl -X POST \
      --header "X-Watson-Authorization-Token: $(< ./token)" \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token=$(< ./token)"
        ```
        {: pre}

    -   *Windows コマンド・プロンプトから*、`token` ファイルの内容を以下のコマンドに組み込みます。

        ```bash
        curl -X POST
        --header "X-Watson-Authorization-Token: {token_string}"
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token={token_string}"
        ```
        {: pre}
