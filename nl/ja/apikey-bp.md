---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM authentication,service keys,service roles,best practices

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

# IAM サービスの API キー
{: #api-key-bp}

{{site.data.keyword.ibmwatson}} サービスの API キーは、{{site.data.keyword.ibmwatson_notm}} サービスへの呼び出しの認証に次々に使用されるベアラー・トークンを獲得するために使用されます。
{: shortdesc}

API キーの標準的な {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 実装とは異なり、{{site.data.keyword.ibmwatson_notm}} IAM キーはサービスごとに設定されます。許可は、各サービスではなく各 API キーに割り当てることができます。この実装は、ユーザー・ベースの API キーの標準的な {{site.data.keyword.cloud_notm}} 方式とは異なります。{{site.data.keyword.ibmwatson_notm}} では、ユーザーには各サービスの役割が割り当てられます。一般的な {{site.data.keyword.cloud_notm}} IAM 実装について詳しくは、[API キーのベスト・プラクティス](/docs/services/iam?topic=iam-iamoverview#iamoverview)を参照してください。

Watson API キーは、単一のサービス・インスタンスのみにバインドすることができます。
{: tip}

`管理者`役割の一連の資格情報は、{{site.data.keyword.ibmwatson_notm}} サービス・インスタンスが作成されるたびに自動的に生成されます。これらの資格情報は、サービス・インスタンスの**「ダッシュボード」**ページで入手できます。それらを使用してサービスの API の*すべての*メソッドを呼び出せます。異なる役割を持つ API キーを作成することで、サービス使用時に実行できるアクションを限定できます。

| サービスのアクセス役割 | アクションの説明 |
|:-----------------|:-----------------|
| `リーダー` | リーダーは、サービス固有のリソースの表示など、サービス内の読み取り専用アクションを実行できます。|
| `ライター` | ライターは、サービス固有のリソースの作成および編集など、リーダー役割を超える権限を持ちます。|
| `管理者` | 管理者は、サービスによって定義された特権付きアクションを実行する、ライター役割を超えるアクセス権を持ちます。それに加え、サービス固有のリソースを作成および編集できます。 |
{: caption="表 1. ユーザーのサービス・アクセス役割の例" caption-side="top"}

追加の {{site.data.keyword.ibmwatson_notm}} サービス資格情報の作成について詳しくは、[IAM サービス資格情報の更新](/docs/services/watson?topic=watson-iam#update-existing-svcs) を参照してください。

## API キーのベスト・プラクティス
{: #api-bp}

サービスの認証方式に応じて、API キーをセキュアに保っておく必要があります。以下のベスト・プラクティスは、セキュアな API キーを維持し、資格情報が公開されてアプリケーションの情報が漏えいすることのないようにするうえで役立ちます。

-   *使用している機能に適した役割を持つ API キーを使用してください。*

    エンド・ユーザー・アプリケーションを作成する場合、`GET` API メソッドへのすべての呼び出しに対して `リーダー`役割を持つ API キーを使用することを検討してください。`リーダー`役割は、サービス・インスタンスに対して破壊的もしくは追加的な機能は実行できません。

-   *コードに API キーを直接埋め込まないでください。*

    コードに埋め込まれている API キーは、ユーザーに公開されてしまう可能性があります。API キーは、アプリケーション・コードに埋め込まず、環境変数か、ソース・コード制御システム外部のファイルに格納することを検討してください。

-   *API キーをアプリケーションのソース・コード制御システム内部のファイルに格納しないでください。*

    API キーをファイルに格納する場合、そのファイルはアプリケーションのソース・コードの外部に保管してください。このような対策は、GitHub などのパブリックなソース・コード管理システムを使用する場合は特に重要です。

-   *API キーは定期的に再生成してください。*

    新しい資格情報はいつでも作成できます。
    1.  リソース・リストから、サービス・インスタンスを選択します。
    1.  ページの左側にあるナビゲーション・メニューで**「サービス資格情報」**をクリックします。

     アプリケーションを新規資格情報に移行する場合、古い資格情報を必ず削除してください。削除するには、削除する資格情報のごみ箱アイコンを使用します。
     {: tip}
