---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-03"

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

# {{site.data.keyword.watson}} サービスの要求ロギングの制御

デフォルトでは、すべての {{site.data.keyword.ibmwatson}} サービスが要求と結果をログに記録します。ロギングは、今後のユーザー・サービスの向上の目的でのみ行われます。ログ・データは共有されることも、公開されることもありません。
{: shortdesc}

ユーザーの個人情報のプライバシー保護について懸念がある場合や、自分の要求が {{site.data.keyword.IBM_notm}} によって使用されることを望まない場合は、オプトアウトを選択できます。

自分のデータがサービス全般の向上のために {{site.data.keyword.IBM_notm}} によって使用されないようにするには、要求のヘッダー・パラメーター X-Watson-Learning-Opt-Out を `true` または `1` に設定します。(false または 0 以外の値を指定すると、その呼び出しの要求ロギングが無効になります。) サービス全般の向上のために {{site.data.keyword.IBM_notm}} に使用してほしくない要求ごとに、このヘッダーを設定しなければなりません。

> **重要:** `X-WDC-PL-OPT-OUT` は非推奨になっています。以前の名前であるこの X-WDC-PL-OPT-OUT は現在でも有効ですが、非推奨です。このヘッダーでは、要求ロギングのオプトアウトの値として 1 を使用できます。その他の値を指定すると、ロギングが有効になります。
