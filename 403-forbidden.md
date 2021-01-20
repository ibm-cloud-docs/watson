---

copyright:
   years: 2020, 2021
lastupdated: "2021-01-20"

keywords: watson 401 error, watson messages, watson error messages, watson response codes, watson status codes

subcollection: watson

content-type: troubleshoot

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:support: data-reuse='support'}

# Why can't I complete an API request to a Watson service?
{: #forbidden-error}
{: troubleshoot}

You send a request to a Watson API but receive an error message that you don't have permission.
{: shortdesc}

When you send a request through a Watson API, you receive a 403 HTTP status code.
{: tsSymptoms}

 For example,

```json
{
  "trace":"b0615b07-3a0f-4eb0-8df4-263d64be6b03",
  "error":"Forbidden",
  "more_info":"https://cloud.ibm.com/docs/watson?topic=watson-forbidden-error",
  "code":403
}
```
{: screen}

A 403 HTTP status code indicates that you are not allowed to make the request. 403 is similar to [401](/docs/watson?topic=watson-authorization-error), but changing the authentication does not affect the result.
{: tsCauses}

The access is usually controlled in {{site.data.keyword.watson}} services by service roles. Validate the role that is associated with your credentials. The service access role that is associated with your API key determines which endpoints and methods you can access.
{: tsResolve}

For example, with a Reader role, you might not be able to edit, create, or delete anything, and might not be able to view some details of a resource.

- To manage roles through the {{site.data.keyword.cloud_notm}} console, go to **Manage** > **Access (IAM)**, and then select **Users**, **Access groups**, or **Service IDs** to get started.
- For more information about access to {{site.data.keyword.conversationshort}} resources, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).
- For more information about roles required for other {{site.data.keyword.watson}} services, see [IAM roles and actions](/docs/account?topic=account-iam-service-roles-actions) and search for your service.
- For more information about IAM roles in {{site.data.keyword.cloud_notm}}, see [Access management](/docs/account?topic=account-cloudaccess).
