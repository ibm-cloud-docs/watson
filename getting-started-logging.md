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

# Controlling request logging for {{site.data.keyword.watson}} services

By default, all {{site.data.keyword.ibmwatson}} services log requests and their results. Logging is done only to improve the services for future users. The logged data is not shared or made public.
{: shortdesc}

If you are concerned with protecting the privacy of users' personal information or otherwise do not want your requests to be used by {{site.data.keyword.IBM_notm}}, you can choose to opt out.

To prevent {{site.data.keyword.IBM_notm}} usage of your data for general service improvements, set the header parameter X-Watson-Learning-Opt-Out to `true` or `1` for the request. (Any value other than false or 0 disables request logging for that call.) You must set the header on each request that you do not want {{site.data.keyword.IBM_notm}} to use for general service improvements.

> **Important:** `X-WDC-PL-OPT-OUT` deprecated: The earlier name, X-WDC-PL-OPT-OUT, is deprecated, although it continues to work for now. The header accepts a value of 1 to opt out of request logging. Any other value enables logging.
