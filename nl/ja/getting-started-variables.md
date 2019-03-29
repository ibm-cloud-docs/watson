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

# VCAP\_SERVICES 環境変数
{: #vcapServices}

`VCAP_SERVICES` 環境変数には、{{site.data.keyword.cloud}} のサービス・インスタンスと対話するために使用できる情報が含まれます。 この環境変数の各フィールドは、サービス・インスタンスをアプリケーションにバインドする時に設定されます。
{: shortdesc}

アプリケーションには、{{site.data.keyword.watson}} サービスに対して認証を行うための URL とサービス資格情報が必要です。実行中のアプリケーションは通常、サービスがバインドされた後でこの環境変数を読み取り、必要な名前/値のペアを抽出します。リソース・グループ・サービスまたは Cloud Foundry サービスのいずれかをアプリにバインドできますが、リソース・グループ・サービスにはサービス別名が必要です。

## IAM サービスのサービス別名
{: #vcapIAMAlias}

サービスに別名を付けることで、リソース・グループ・サービスで `VCAP_SERVICES` 環境変数を使用できます。例えば、{{site.data.keyword.cloud_notm}} コマンド・ライン・インターフェースで以下のコマンドを使用して、`my-personality-insights-service` というサービスに `personality-insights-service` という別名を作成します。

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

これで、アプリをサービス別名にバインドできるようになりました。実行中のアプリケーションは通常、サービス・インスタンスがバインドされた後でこの環境変数を読み取り、必要な名前/値のペアを抽出します。

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## 例
{: #gs-variables-vcapIAMAliasExample}

アプリケーションにバインドされた {{site.data.keyword.personalityinsightsshort}} サービスの `VCAP_SERVICES` 環境変数の例を以下に示します。サービス・インスタンスは Cloud Foundry 認証を使用します。

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

| 項目     | 説明                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | サービスへの接続に使用する HTTP 基本認証資格情報のパスワード                               |
| url      | サービスの接続 URL                                                                         |
| username | サービスへの接続に使用する HTTP 基本認証資格情報のユーザー名                               |
| label    | {{site.data.keyword.cloud_notm}} のサービスに関連付けられた名前                          |
| name     | サービス・インスタンスの名前                                                               |
| plan     | {{site.data.keyword.cloud_notm}} のサービスで使用できるプラン                      |
| tags     | サービスに関する追加情報                                                                   |

## 環境変数の表示
{: #gs-variables-vcapView}

変数に関する情報は、{{site.data.keyword.cloud_notm}} コマンド・ライン・インターフェース (CLI) または Web インターフェースから取得できます。

- {{site.data.keyword.cloud_notm}} CLI の場合: サービスを作成し、それをアプリケーションにバインドした後、Cloud Foundry の `env` コマンドを起動します。

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- {{site.data.keyword.cloud_notm}} インターフェースから取得する場合: アプリケーションのサマリー・ページで環境変数に関する情報を確認できます。
