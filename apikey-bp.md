---

copyright:
  years: 2015, 2018
lastupdated: "2018-06-22"

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

# IAM service API keys
{: #api-key-bp}

{{site.data.keyword.ibmwatson}} service API keys are used to acquire bearer tokens that are in turn used to authenticate calls to {{site.data.keyword.ibmwatson_notm}} services.
{: shortdesc}

Unlike the standard {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) implementation of API keys, {{site.data.keyword.ibmwatson_notm}} IAM keys are set on a per-service basis. Permissions can be assigned to each API key rather than to each service. This differs from the standard {{site.data.keyword.Bluemix_notm}} method of user-based API key. With {{site.data.keyword.ibmwatson_notm}}, users are assigned a role for each service. For more information about the general {{site.data.keyword.Bluemix_notm}} IAM implementation, see [API key best practices](/docs/services/iam/index.html).

A set of credentials for the `Manager` role is automatically generated each time an {{site.data.keyword.ibmwatson_notm}} service instance is created. These credentials are available to you on the **Dashboard** page of the service instance. You can use them to call *all* methods of the service's API. You can limit the possible actions performed when using the service by creating an API key with a different role.

| Service access role | Description of actions |
|:-----------------|:-----------------|
| `Reader` | Readers can perform read-only actions within a service, such as viewing service-specific resources. |
| `Writer` | Writers have permissions beyond the reader role, including creating and editing service-specific resources. |
| `Manager` | Managers have permissions beyond the writer role to complete privileged actions as defined by the service. In addition, they can create and edit service-specific resources. |
{: caption="Table 1. Example service access user roles" caption-side="top"}

For more information about creating additional {{site.data.keyword.ibmwatson_notm}} service credentials, see [Updating service credentials](/docs/services/watson/getting-started-credentials.html#existing-svcs).

## API key best practices
{: #api-bp}

As the method of authentication to your service, your API keys must be kept secure. The following best practices help you maintain a secure API key and reduce the chance of publicly exposing credentials that compromise your application.

-   *Use an API key with the role that is appropriate for the function that you are using.*

    When creating an end-user application, consider using an API key with the role `Reader` for all calls to `GET` API methods. The `Reader` role can't perform any destructive or additive functions to the service instance.

-   *Do not embed the API key directly in code.*

    API keys that are embedded in code might be exposed to your end users. Instead of embedding the API keys in your application code, consider storing them either in environment variables or in files outside of your source code control system.

-   *Do not store an API key in files inside your application's source code control system.*

    When storing API keys in files, keep the files outside of your application's source tree. This is particularly important if you use a public source code management system such as GitHub.

-   *Regenerate your API keys periodically.*

    You can create new credentials at any time by using the {{site.data.keyword.Bluemix_notm}} dashboard. When you have moved your application over to the new credentials that you have created, don't forget to delete the old credentials from the dashboard by using the trashcan icon for the credentials that you want to delete.
