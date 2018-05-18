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

# Cloud Foundry コマンド・ライン・インターフェース

`cf` コマンド・ライン・インターフェース (CLI) ツールを使用すれば、ローカル・システムで {{site.data.keyword.cloud}} アプリケーションの作業を実行できます。このツールには、充実した開発エクスペリエンスを実現するための豊富なインターフェースが用意されています。
{: shortdesc}

アプリケーション開発用の Cloud Foundry ツールの詳細については、[Cloud Foundry と {{site.data.keyword.cloud_notm}} の連携方法](/docs/overview/cf.html)と [Cloud Foundry の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://docs.cloudfoundry.org/){: new_window}を参照してください。

## cf コマンド・ライン・インターフェースのインストール

このツールの最新バージョンは、[Cloud Foundry リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/cloudfoundry/cli#downloads){: new_window} の**「Downloads」**セクションにあります。次の 2 つのタイプのパッケージがあります。

- **インストーラー**。これは、ご使用のプラットフォームのパッケージ管理ツールまたはインストール済みのソフトウェア・トラッキング・システムと統合され、正式な場所にツールをインストールします。
- **バイナリー**。これは、任意の場所にインストールできるアーカイブ・ファイルを提供します。

ツールのインストール後に、必ず `PATH` 環境変数にコマンドを含めてください。{: tip}

## サービス・インスタンスのコマンド

{{site.data.keyword.cloud_notm}} の既存の {{site.data.keyword.watson}} サービス・インスタンスの管理に有用な `cf` コマンドを以下にまとめます。

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services): 現行スペース内のサービス・インスタンスのリストを表示します。

  ```bash
  cf services
  ```
  {: pre}

  サービスの名前が分かっている場合は、`cf service` コマンドを使用してそのサービスに関する情報を確認できます。以下に例を示します。

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): {{site.data.keyword.cloud_notm}} で実行するアプリケーションにバインドできるサービスのインスタンスを作成します。

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - *{service-name}* 引数と *{service-plan}* 引数には、`cf marketplace` コマンドの出力を使用します。
    - 作成する {service-instance-name} を指定します。多くのアプリケーションでは、この名前は `manifest.yml` や他の構成ファイルで指定されています。

- **cf delete-service**: サービス・インスタンスを削除します。

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    不要になったサービスを削除することで、{{site.data.keyword.cloud_notm}} の不要なリソースを解放できます。

## アプリケーションをデプロイおよび実行するためのコマンド

{{site.data.keyword.cloud_notm}} でアプリケーションをデプロイしたり実行したりするための一般的な `cf` コマンドを以下にまとめます。

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): {{site.data.keyword.watson}} API の場所を指定します。

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login): {{site.data.keyword.ibmid}} とパスワードを使用して {{site.data.keyword.cloud_notm}} にログインします。

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): 接続できる {{site.data.keyword.cloud_notm}} カタログ内のサービスのリストを表示します。

  ```bash
  cf marketplace
  ```
  {: pre}

  各サービスの名前と、そのサービスを利用できるプランのリストが出力されます。{{site.data.keyword.cloud_notm}} で実行するアプリケーションのサービス・インスタンスを作成する時に、この情報が必要になります。

  サービスの名前が分かっている場合は、`-s` オプションを使用できます。以下に例を示します。

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): {{site.data.keyword.cloud_notm}} に対してアプリケーションのデプロイまたは更新を行います。アプリケーションにアクセスするための URL が出力されます。

  ```bash
  cf push {application-name}
  ```
  {: pre}

  このコマンドは多くの場合 `manifest.yml` や他の構成ファイルを使用して、プッシュする対象のファイル (例えば、Java の場合は `.war` ファイル、Node.js の場合は `.zip` ファイル) を検出します。ただし、`-p` オプションを使用して、アプリケーション・ファイルのパス名を指定することも可能です。

## 既存のアプリケーション用のコマンド

{{site.data.keyword.cloud_notm}} の既存のアプリケーションの管理に有用な `cf` コマンドを以下にまとめます。

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps): 現在のスペースにデプロイされているアプリケーションのリストを表示します。

  ```bash
  cf apps
  ```
  {: pre}

  アプリケーション名を含めれば、そのアプリケーションの正常性と状況に関する詳細情報を表示できます。

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: {{site.data.keyword.cloud_notm}} でアプリケーションを再ロードして再始動します。

  ```bash
  cf restage {application-name}
  ```
  {: pre}

このコマンドは、{{site.data.keyword.cloud_notm}} に存在するアプリケーションを再始動します。最新バージョンのアプリケーションを {{site.data.keyword.cloud_notm}} にアップロードしてから再始動する場合は、`cf push` コマンドを使用します。

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete): アプリケーションを削除します。

  ```bash
  cf delete application-name
  ```
  {: pre}

  アプリを削除すると、{{site.data.keyword.cloud_notm}} のメモリーと割り当て量が解放されて、他のアプリケーションのために使用できるようになります。

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service): サービスをアプリケーションにバインドします。

  サービスのインスタンスをそのインスタンスを使用するアプリケーションにバインドするには、通常は `cf push` コマンドを使用します。`cf bind-service` コマンドを使用すれば、既存のサービス・インスタンスを既存のアプリケーションにバインドできます。

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  この後、`cf restage` または `cf push` を実行してアプリケーションを再始動します。

- **cf unbind-service**: アプリケーションとサービスの間の関連付けを解除します。

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  場合によっては、`cf restage` コマンドまたは `cf push` コマンドを実行して変更を反映する必要があります。

  このコマンドは、`VCAP_SERVICES` 環境変数から認証資格情報を削除します。サービス・インスタンスまたはアプリケーションを削除する場合は、`cf delete-service` または `cf delete` を使用します。{: tip}
