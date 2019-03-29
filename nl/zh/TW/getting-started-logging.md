---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# 控制 {{site.data.keyword.watson}} 服務的要求記載
{: #gs-logging-overview}

依預設，所有 {{site.data.keyword.ibmwatson}} 服務都會記載要求及其結果。記載只是為了替未來使用者改善服務。所記載的資料不會共用或公開。
{: shortdesc}

如果您對於保護使用者個人資訊的隱私有所顧慮，或是有其他原因不想讓您的要求被 {{site.data.keyword.IBM_notm}} 使用，您可以選擇拒絕。

若要防止 {{site.data.keyword.IBM_notm}} 使用您的資料進行一般服務改進，請將要求的標頭參數 X-Watson-Learning-Opt-Out 設為 `true` 或 `1`。（除了 false 或 0 以外的任何值，都會停用該呼叫的要求記載。）您必須在不想要 {{site.data.keyword.IBM_notm}} 用於一般服務改進的每個要求上都設定標頭。

> **重要事項**：`X-WDC-PL-OPT-OUT` 已淘汰：舊版名稱 X-WDC-PL-OPT-OUT 已淘汰，但它現在仍可繼續運作。該標頭接受值 1，以拒絕要求記載。任何其他值都會啟用記載功能。
