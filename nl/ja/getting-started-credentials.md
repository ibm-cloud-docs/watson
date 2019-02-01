---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Watson サービスのサービス資格情報

{{site.data.keyword.ibmwatson}} サービスのサービス資格情報によって、{{site.data.keyword.cloud}} のサービスに対する認証を行うことができます。資格情報セットは、その対象のサービスでのみ使用できます。
{: shortdesc}

サービス資格情報の使用法は、プログラミング・モデルによって異なります。詳細については、[{{site.data.keyword.watson}} サービスのプログラミング・モデル](/docs/services/watson/getting-started-develop.html)を参照してください。

サービス資格情報は、特定の {{site.data.keyword.watson}} サービスの HTTP 基本認証資格情報です。

## 資格情報の手動取得
{: #getting-credentials-manually}

{{site.data.keyword.cloud_notm}} Web インターフェースからサービス資格情報を確認できます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window} にログインします。
1.  既存のサービスに移動します。([ここからアクセスしてください ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/developer/watson/existing-services){: new_window})

    1.  左上のメニューを選択し、**「{{site.data.keyword.watson}}」**を選択して、{{site.data.keyword.watson}} コンソールに移動します。
    1.  **「Watson サービス」**から**「既存のサービス」**を選択して、サービスとプロジェクトのリストを表示します。
1.  資格情報を表示します。
    - サービスがプロジェクトに組み込まれている場合は、そのサービスが入っているプロジェクトの名前をクリックします。プロジェクト詳細ページの**「資格情報」**セクションに表示されている資格情報を確認します。
    - サービスがプロジェクトに組み込まれていない場合は、表示するサービス名をクリックし、**「サービス資格情報」**を選択します。

## サービス資格情報の更新
{: #existing-svcs}

サービス・ダッシュボードから、既存のサービス・インスタンスのサービス資格情報を更新できます。

1.  [{{site.data.keyword.cloud_notm}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window} にログインします。
1.  既存のサービスに移動します。([ここからアクセスしてください ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/developer/watson/existing-services){: new_window})

    1.  左上のメニューを選択し、**「{{site.data.keyword.watson}}」**を選択して、{{site.data.keyword.watson}} コンソールに移動します。
    1.  **「Watson サービス」**から**「既存のサービス」**を選択して、サービスのリストを表示します。
1.  表示/更新するサービスを選択し、**「サービス資格情報」**を選択します。
1.  資格情報を削除/追加します。
    - 資格情報を削除するには、資格情報を選択し、**「資格情報の削除」**を選択します。
    - 資格情報を追加するには、**「新規資格情報」**をクリックしてから、「新規資格情報の追加」ウィンドウで**「追加」**をクリックします。新しい資格情報を表示してコピーします。

次のようにして、必ず新しい資格情報をアプリケーションで使用するようにしてください。

- アプリケーションがプログラマチックに資格情報を取得するようになっている場合は、アプリケーションを停止してから再始動します。
- アプリケーション・コードに資格情報が埋め込まれている場合は、コードを更新してアプリケーションを再起動します。

最小限のダウン時間で新しい資格情報でアプリケーションを更新する方法について詳しくは、[アプリの更新](/docs/manageapps/updapps.html)を参照してください。

## {{site.data.keyword.Bluemix_dedicated_notm}} の資格情報

[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) インスタンスのサービス・タイルは、組織内の 1 つのサービスにつきタイルを 1 つだけ使用できるように実装されています。

以下の手順を実行して、独自の {{site.data.keyword.Bluemix_dedicated_notm}} ワークスペースにあるサービスを参照してください。

1.  使用するサービスの参照タイルの資格情報セットを作成します。
1.  それらの資格情報を使用するユーザー提供のカスタム・サービスを作成します。

    1.  {{site.data.keyword.cloud_notm}} インターフェースで、使用するサービスのタイルを選択し、そのサービスの資格情報セットを作成します。JSON 形式で資格情報が作成されます。その中には、サービスと通信するための URL と、そのサービスで使用するユーザー名とパスワードのフィールドが含まれます。
    1.  `cf cups` (ユーザー提供サービスの作成) コマンドを使用して、これらの資格情報にバインドするカスタム・サービスを作成します。例えば、以下の資格情報があるとします。

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      この場合は、次のようにして、`Personality-Insights-Std` という名前のユーザー提供のカスタム・サービス・インスタンスを作成します。

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      ユーザー名とパスワードは、必ず二重引用符で囲んだ 1 つのストリングとして指定し、それぞれをコロンで区切ってください。また、ユーザー名とパスワードの両方のストリングを二重引用符で囲み、円記号でエスケープしてください。
      {: tip}

### {{site.data.keyword.Bluemix_dedicated_notm}} でのアプリケーションの使用

GitHub の一部の {{site.data.keyword.watson}} のサンプル・アプリとスターター・キットには、**「{{site.data.keyword.cloud_notm}} へのデプロイ」**ボタンが含まれています。このボタンは {{site.data.keyword.Bluemix_dedicated_notm}} のパブリック環境を参照しているので、{{site.data.keyword.cloud_notm}} のインストール環境では機能しません。

{{site.data.keyword.Bluemix_dedicated_notm}} 環境でサンプル・アプリケーションとスターター・キットを使用するには、以下のようにします。

1.  アプリケーションに対する命令の中で、作成する `manifest.yml` ファイルで、`cf cups` コマンドで作成したサービスの名前が使用されるようにします。
1.  同じ命令で、`cf create-service` コマンドを実行する代わりに、`cf cups` コマンドを使用します。ただし、ユーザー提供のカスタム・サービス・インスタンスを作成した場合は、サンプル・アプリケーションやスターター・キットで使用するためにそのインスタンスを再度作成する必要はありません。
