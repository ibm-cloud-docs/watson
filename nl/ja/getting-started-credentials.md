---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# Cloud Foundry サービス資格情報による認証
{: #creating-credentials}

{{site.data.keyword.ibmwatson}} サービスの Cloud Foundry サービス資格情報により、{{site.data.keyword.cloud}} のサービスに対する認証が行われます。サービス資格情報では HTTP 基本認証が使用され、特定の {{site.data.keyword.watson}} サービスに対する認証が行われます。
{: shortdesc}

リソース・グループ内に作成される、もしくは Cloud Foundry からリソース・グループに移行される {{site.data.keyword.watson}} サービスは、IAM 認証を使用します。デフォルトでは、新しい {{site.data.keyword.watson}} サービスはすべて、IAM 認証を使用します。リソース・グループと IAM 認証について詳しくは、[IAM トークンによる認証](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually) を参照してください。
{: note}

## Cloud Foundry サービス資格情報の取得
{: #getting-credentials-manually}

Cloud Foundry サービス資格情報を使用して API メソッドにアクセスするためには、まず資格情報を収集する必要があります。サービス資格情報には {{site.data.keyword.cloud_notm}} Web インターフェースからアクセスできます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}){: new_window} にログインします。
1.  [リソース・リスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/dashboard){: new_window} に移動します。
1.  サービス・インスタンスを選択します。
1.  **「資格情報の表示」**をクリックすると、資格情報の**ユーザー名**、**パスワード**、**URL** が表示されます。

## Cloud Foundry サービス資格情報の更新
{: #existing-svcs}

サービス・ダッシュボードから、既存のサービス・インスタンスのサービス資格情報を更新できます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}){: new_window} にログインします。
1.  [リソース・リスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/dashboard){: new_window} に移動します。
1.  ページの左にあるナビゲーション・メニューから**「サービス資格情報」**を選択します。
1.  メニューとアイコンを使用して、既存の資格情報を削除したり、新しい資格情報を追加したりします。

変更があった場合は、アプリケーション内の資格情報を必ず更新してください。

## {{site.data.keyword.Bluemix_dedicated_notm}} の資格情報

[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated) インスタンスのサービス・タイルは、組織内の 1 つのサービスにつきタイルを 1 つだけ使用できるように実装されています。

以下の手順を実行して、独自の {{site.data.keyword.Bluemix_dedicated_notm}} ワークスペースにあるサービスを参照してください。

1.  使用するサービスの参照タイルの資格情報セットを作成します。
1.  それらの資格情報を使用するユーザー提供のカスタム・サービスを作成します。

    1.  Web インターフェースを使用して、使用するサービスのタイルを選択し、そのサービスの資格情報セットを作成します。資格情報は JSON フォーマットで作成され、サービス・エンドポイント URL、ユーザー名、パスワードが含まれます。
    1.  `ibmcloud cf cups` (ユーザー提供サービスの作成) コマンドを使用して、これらの資格情報にバインドするカスタム・サービスを作成します。以下に例を示します。

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      このような資格情報であるとすると、`Personality-Insights-Std` というユーザー提供のカスタム・サービス・インスタンスを次のようにして作成します。

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      ユーザー名とパスワードは、必ず二重引用符で囲んだ 1 つのストリングとして指定し、それぞれをコロンで区切ってください。 また、ユーザー名とパスワードの両方のストリングを二重引用符で囲み、円記号でエスケープしてください。
      {: tip}

### {{site.data.keyword.Bluemix_dedicated_notm}} でのアプリケーションの使用

GitHub の一部の {{site.data.keyword.watson}} のサンプル・アプリとスターター・キットには、**「{{site.data.keyword.cloud_notm}} へのデプロイ」**ボタンが含まれています。 このボタンはパブリック {{site.data.keyword.Bluemix_dedicated_notm}} 環境を参照するため、{{site.data.keyword.cloud_notm}} インストール済み環境では機能しません。

{{site.data.keyword.Bluemix_dedicated_notm}} 環境でサンプル・アプリケーションとスターター・キットを使用するには、以下のステップに従ってください。

1.  アプリケーションに対する命令の中で、作成する `manifest.yml` ファイルが、`ibmcloud cf cups` コマンドで作成したサービスの名前を使用するようになっていることを確認してください。
1.  同じ命令で、`ibmcloud cf create-service` コマンドを実行する代わりに、`ibmcloud cf cups` コマンドを使用します。ただし、ユーザー提供のカスタム・サービス・インスタンスを作成した場合は、サンプル・アプリケーションやスターター・キットで使用するためにそのインスタンスを再度作成する必要はありません。
