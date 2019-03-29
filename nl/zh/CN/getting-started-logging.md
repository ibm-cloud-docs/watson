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

# 控制 {{site.data.keyword.watson}} 服务的请求日志记录
{: #gs-logging-overview}

缺省情况下，所有 {{site.data.keyword.ibmwatson}} 服务记录请求及其结果。日志记录仅用于改进未来用户的服务。不会共享或公开记录的数据。
{: shortdesc}

如果您关注保护用户个人信息的隐私或者不希望 {{site.data.keyword.IBM_notm}} 使用您的请求，那么可选择退出。

要避免 {{site.data.keyword.IBM_notm}} 使用您的数据进行常规服务改进，请将请求的头参数 X-Watson-Learning-Opt-Out 设置为 `true` 或 `1`。（false 或 0 之外的任何值都将禁用此调用的请求日志记录。）必须在不希望 {{site.data.keyword.IBM_notm}} 用于常规服务改进的每个请求上设置头。

> **重要信息：**不推荐 `X-WDC-PL-OPT-OUT`：不推荐使用先前的名称 X-WDC-PL-OPT-OUT，尽管其现在继续有效。该头接受值 1 以选择退出请求日志记录。任何其他值将启用日志记录。
