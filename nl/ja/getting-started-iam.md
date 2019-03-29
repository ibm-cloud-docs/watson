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

# IAM トークンによる認証
{: #iam}

{{site.data.keyword.cloud}} Identity and Access Management (IAM) トークンを使用して、呼び出しごとにサービス資格情報を組み込むことなく {{site.data.keyword.ibmwatson}} サービスへの認証された要求を作成します。IAM 認証はアクセス・トークンを使用して認証を行います。認証は、API キーを持つ要求を送信することで獲得します。
{: shortdesc}

リソース・グループ内に作成される、もしくは Cloud Foundry からリソース・グループに移行される {{site.data.keyword.watson}} サービスは、IAM 認証を使用します。デフォルトでは、新しい {{site.data.keyword.watson}} サービスはすべて、IAM 認証を使用します。
{: note}

## IAM サービス資格情報の取得
{: #iam-getting-credentials-manually}

IAM サービス資格情報を使用して API メソッドにアクセスするには、まずその資格情報を収集する必要があります。サービス資格情報には {{site.data.keyword.cloud_notm}} Web インターフェースからアクセスできます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}){: new_window} にログインします。
1.  [リソース・リスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/dashboard){: new_window} に移動します。
1.  サービス・インスタンスを選択します。
1.  **「資格情報の表示」**をクリックすると、資格情報の **API キー**と **URL** が表示されます。
    1.  資格情報のトークンを表示するには、ページの左側にあるナビゲーション・メニューで**「サービス資格情報」**を選択します。
    1.  **「資格情報の表示」**を選択して、資格情報のすべての API キーとトークンの情報を表示します。

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

## IAM サービス資格情報の更新
{: #update-existing-svcs}

サービス・ダッシュボードから、既存のサービス・インスタンスのサービス資格情報を更新できます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}){: new_window} にログインします。
1.  [リソース・リスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/dashboard){: new_window} に移動します。
1.  ページの左にあるナビゲーション・メニューから**「サービス資格情報」**を選択します。
1.  メニューとアイコンを使用して、既存の資格情報を削除したり、新しい資格情報を追加したりします。

変更があった場合は、アプリケーション内の資格情報を必ず更新してください。

## Watson サービス API キーを使用した IAM トークンの取得
{: #iamtoken}

サービス・インスタンス用に生成された API キーを使用して、{{site.data.keyword.ibmwatson_notm}} サービス API にアクセスできます。その API キーを使用して、IAM アクセス・トークンを生成します。他の {{site.data.keyword.cloud_notm}} サービスを使って作業する必要のあるアプリケーションを開発する場合も、このプロセスを使用します。

API キーのセキュアな使用について詳しくは、[IAM サービス API キー](/docs/services/watson?topic=watson-api-key-bp)を参照してください。
{: tip}

以下の curl コマンドは、`POST identity/token` メソッドを使用し、API キーを渡すことで、IAM アクセス・トークンを生成します。`Content-Type` ヘッダーは、データが URL エンコードされていることを示します。`data-urlencode` パラメーターは URL エンコード・データをメソッドに渡します。

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

アクセス・トークンを生成するには認証を使用します。トークンをリフレッシュする場合は、同じ認証を使用してください。

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

以下の例は、予期される応答を示しています。

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

## トークンを使用した認証
{: #use_token}

IAM アクセス・トークンは `POST identity/token` メソッドを使用して生成します。こうすることで、トークンを使用して、認証された API 呼び出しを作成できます。1 つの API キーは 1 つの特定のサービス・インスタンスに関連付けられます。その API キーで生成されたトークンは、そのサービス・インスタンスへの呼び出しでのみ使用できます。

{{site.data.keyword.watson}} サービスに対する標準的な curl 呼び出しの形式は以下のとおりです (`{token}` は生成した IAM アクセス・トークン)。

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

あるいは、トークンに接頭部 `Authorization: Bearer ` を付けて (例えば、`Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`)、それをファイルに保存することもできます。その場合、以下のコマンドを使用します。

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## トークンのリフレッシュ
{: #refresh_token}

`POST identity/token` メソッドにより生成された IAM リフレッシュ・トークンを使用して、アクセス・トークンの存続時間を延ばすことができます。以下のコマンドは、既存のアクセス・トークンをリフレッシュします。コマンド内の `{refresh-token}` は、生成した IAM リフレッシュ・トークンです。

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

トークンをリフレッシュするときは、元のトークンを作成したときに使用したのと同じ認証を使用してください。
{: important}
