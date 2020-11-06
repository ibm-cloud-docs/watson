---

copyright:
  years: 2015, 2020
lastupdated: "2020-11-06"

keywords: logging,service improvements,opt out

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# Controlling request logging for {{site.data.keyword.watson}} services
{: #gs-logging-overview}

By default, all {{site.data.keyword.ibmwatson}} services log requests and their results. Logging is done only to improve the services for future users. The logged data is not shared or made public.
{: shortdesc}

If you are concerned with protecting the privacy of users' personal information or otherwise do not want your requests to be used by {{site.data.keyword.IBM_notm}}, you can choose not to have {{site.data.keyword.IBM_notm}} learn from my data (opt out). Choose to opt out at either the account level or at the API request level.

- To prevent {{site.data.keyword.IBM_notm}} usage of your data for general service improvements for all services for which you are the owner, select `Do not learn from my data` on the [{{site.data.keyword.watson}} Privacy Settings](https://{DomainName}/watson/settings/){: external} page. To access the setting, you must be the account owner.
- To prevent {{site.data.keyword.IBM_notm}} usage of your data for an API request, set the **X-Watson-Learning-Opt-Out** header parameter to `true`.

A choice to opt out from logging data at either level overrides a choice to opt in.

You cannot opt in at the request level if you opt out at the account level. However, you can opt out at the request level if the account is set to opt in. If your intent is to opt out for a subset of your services, select `Learn from my data` at the account level. Then, set the request header to `true` to prevent usage of your data on those requests.
{: tip}

For more information about specific use, see the [API reference](https://{DomainName}/developer/watson/documentation){: external} for the {{site.data.keyword.watson}} service.
