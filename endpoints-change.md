---

copyright:
   years: 2020, 2021
lastupdated: "2021-08-25"

keywords: watsonplatform,migrate watson endpoints,update watson endpoints,update watson url

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:beta: .beta}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:curl: .ph data-hd-programlang='curl'}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:go: .ph data-hd-programlang='go'}
{:java: .ph data-hd-programlang='java'}
{:javascript: .ph data-hd-programlang='javascript'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:unity: .ph data-hd-programlang='unity'}

# Updating endpoint URLs from `watsonplatform.net`
{: #endpoint-change}

{{site.data.keyword.watson}} API endpoint URLs at `watsonplatform.net` are changing and will not work after they are retired. Update your calls to use the newer endpoint URLs.
{: shortdesc}

The `watsonplatform.net` endpoint URLs were retired on 26 May 2021. Update your API calls to use new URLs.
{: deprecated}

As each {{site.data.keyword.watson}} service announced in December 2019, the pattern for the new endpoint URLs is `api.{location}.{offering}.watson.cloud.ibm.com`. For example, with {{site.data.keyword.conversationshort}} services that are hosted in Washington DC, the `message` endpoint changes from `https://gateway-wdc.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/message` to `https://api.us-east.assistant.watson.cloud.ibm.com/instances/{instance_id}/v1/workspaces/{workspace_id}/message`. The domain, location, and offering identifier are different in the new endpoint.

When you call a {{site.data.keyword.watson}} API, use the new base URL that corresponds to your {{site.data.keyword.watson}} offering and to the location where your service instance is hosted.

## Finding and updating the endpoint URL
{: #endpoint-find-update}

You can find the URL for your service instance with the service credentials. You can also update the endpoint URL and API key in the same place.

1.  Click the name of the service in the [Resource list](https://{DomainName}/resources?groups=resource-instance){: external}.
1.  If you see a URL with the domain `watsonplatform.net` in the credentials, update the credentials:
    1.  Click the **Service credentials** tab, and then click **New credential**.
    1.  Expand the new credentials and look for the `url`. The pattern is `api.{location}.{offering}.watson.cloud.ibm.com`.
    1.  Look for the `apikey` in the new credentials.
1.  Delete the older credentials by clicking the **Delete** icon ![Delete icon](../icons/delete.svg "Delete") for the set.
1.  Update your API calls to use the `watson.cloud.ibm.com` URL and API key values.

## Updating your API calls
{: #endpoint-update-code}

After 26 May 2021, your applications and API calls that use `watsonplatform.net` cannot connect.
{: important}

Update your client applications to use the updated endpoint URL with the pattern `api.{location}.{offering}.watson.cloud.ibm.com` and the API key from the same set of credentials. Although the existing API key works with the new endpoint URL until you delete the older credentials, some {{site.data.keyword.watson}} tools might continue to use the old URL if it exists. By deleting the older credential set, you reduce the chance that the call does not succeed.

For more information about setting the URL with the {{site.data.keyword.watson}} SDKs, see the **Endpoint URLs** section of the [API reference](/docs?tab=api-docs&category=ai) for your service.

## Information about other services
{: #endpoint-links}

The following links lead to more information about how to update endpoint URLs for specific offerings.

- [{{site.data.keyword.conversationshort}} integrations](/docs/assistant?topic=assistant-release-notes#6April2021) (including web chat, Slack, Facebook Messenger, Jupyter Notebooks)
- [{{site.data.keyword.IBM_notm}} Voice Gateway](https://ibm.box.com/s/w6t0pckql87hv84dlxuk9gpiuq960w1g){: external}
