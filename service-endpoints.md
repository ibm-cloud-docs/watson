---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-22"

keywords: service endpoint,private network endpoint,network endpoint

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:preview: .preview}
{:beta: .beta}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Public and private network endpoints
{: #public-private-endpoints}

{{site.data.keyword.cloud}} supports both public and private network endpoints for certain plans. Connections to private network endpoints do not require public internet access.
{:shortdesc}

Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

## Enabling your account
{: #requirements-endpoints}

Private network endpoints are supported for paid plans. Check the plan information for your service to learn about the plans that support private network endpoints.
{: important}

Your account must be configured before you can use private endpoints. To use private network endpoints, the following account features must be enabled for your account.

- Virtual routing and forwarding (VRF).
- Service endpoints. Enabling service endpoints means that all users in the account can connect to private network endpoints.

To enable VRF, you create a support case. To enable service endpoints, you use the {{site.data.keyword.Bluemix_notm}} CLI. For more information about how to enable your account, see [Enabling VRF and service endpoints](/docs/account?topic=account-vrf-service-endpoint).

## Setting a private endpoint

After your account is enabled for VRF and service endpoints, you can add a private network endpoint to a service instance.

A service instance can have a private network endpoint, a public network endpoint, or both.

- Public: A service endpoint on the {{site.data.keyword.cloud_notm}} public network.
- Private: A service endpoint that is accessible only on the {{site.data.keyword.cloud_notm}} private network with no access from the public internet.
- Both public and private: Service endpoints that allow access over both networks.

### Adding a private network endpoint

You add a private endpoint to a paid service instance from the service details page if you have a Manager or Writer service access role.

1.  Go to your [Resource list](https://{DomainName}/resources){: external}.
1.  Click the name of a service instance that is on a paid plan. Lite plans do not support private network endpoints.
1.  In the service details page, click the **Manage** tab.
1.  Click **Add private network endpoint**.

## Viewing your endpoint URL

The service endpoint URLs are different for private and public network endpoints. You can view the URL for an endpoint from the service details page.

1.  Go to your [Resource list](https://{DomainName}/resources){: external}.
1.  Click the name of a service instance that has a private network endpoint.
1.  In the service details page, click the **Manage** tab, and then click **Private Network Endpoint**.

## What to do next
- [Configure your account](/docs/account?topic=account-vrf-service-endpoint) for VRF and Service endpoints.
- Modify your applications to use the new service endpoint URL.
- Read more about [service endpoints](/docs/account?topic=account-service-endpoints-overview).
