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

# VCAP\_SERVICES 環境変数
{: #vcapServices}

`VCAP_SERVICES` 環境変数には、{{site.data.keyword.Bluemix}} のサービス・インスタンスと対話するために使用できる情報が含まれます。この環境変数の各フィールドは、サービスをアプリケーションにバインドする時に設定されます。
{: shortdesc}

アプリケーションには、{{site.data.keyword.watson}} サービスに対して認証を行うための URL と HTTP 基本認証サービス資格情報が必要です。

実行中のアプリケーションは通常、サービスがバインドされた後でこの環境変数を読み取り、必要な名前/値のペアを抽出します。変数の詳細については、[アプリケーションのデプロイ](/docs/manageapps/depapps.html#app_env)を参照し、「環境変数」を検索してください。

## 例
アプリケーションにバインドされた {{site.data.keyword.personalityinsightsshort}} サービスの `VCAP_SERVICES` 環境変数の例を以下に示します。

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

`VCAP_SERVICES` 環境変数の内容を以下の表にまとめます。

| 項目     | 説明                                                                                       |
|----------|--------------------------------------------------------------------------------------------|
| password | サービスへの接続に使用する HTTP 基本認証資格情報のパスワード                               |
| url      | サービスの接続 URL                                                                         |
| username | サービスへの接続に使用する HTTP 基本認証資格情報のユーザー名                               |
| label    | {{site.data.keyword.cloud_notm}} のサービスに関連付けられた名前                          |
| name     | サービス・インスタンスの名前                                                               |
| plan     | {{site.data.keyword.cloud_notm}} のサービスで使用できるプラン                      |
| tags     | サービスに関する追加情報                                                                   |

## 環境変数の表示
Cloud Foundry ツールまたは {{site.data.keyword.cloud_notm}} インターフェースの中から変数に関する情報を取得できます。

- Cloud Foundry コマンド・ライン・ツールを使用する場合: サービスを作成してアプリケーションにバインドしてから、`cf env` コマンドを使用します。

    ```bash
    cf env {application-name}
    ```
    {: pre}

- {{site.data.keyword.cloud_notm}} インターフェースから取得する場合: アプリケーションのサマリー・ページで環境変数に関する情報を確認できます。
