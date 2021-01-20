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

# Why can't I authenticate to my Watson service?
{: #authorization-error}
{: troubleshoot}

<!-- Description of the troubleshooting entry symptom.  -->
You try to authenticate to a Watson service but receive an error message.
{: shortdesc}

When you send your credentials through a Watson API, you receive a 401 HTTP status code.
{: tsSymptoms}

 For example,
```json
{
  "trace":"ec29c5a9-9f99-46a4-9cc0-81a0d4031f84",
  "error":"Unauthorized",
  "more_info":"https://cloud.ibm.com/docs/watson?topic=watson-authorization-error",
  "code":401
}
```
{: screen}

A 401 HTTP status code indicates that your credentials are invalid for the service instance. 401 is similar to [403](/docs/watson?topic=watson-forbidden-error), but refers only to authentication, not permissions.
{: tsCauses}

Common causes include these situations:

- The API key or URL is not correct for the instance.
- The access token is expired.
- The password or API key values include the placeholder brackets (`{`, `}`).
- The instance uses username and password authentication.
- The SDK authentication initialization is the wrong method.

Validate your credentials or try to authenticate with a different command.
{: tsResolve}

- Check your API key and endpoint URL against the service instance by clicking the name of the service instance in the [Resource list](https://{DomainName}/resources?groups=resource-instance){: external} and verifying the credentials.
- If you are using an authorization or service ID to grant access, make sure that you use an endpoint URL that includes the service instance ID. You can find the instance ID by clicking the name of the service instance in the [Resource list](https://{DomainName}/resources?groups=resource-instance){: external} and looking at the credentials URL.
- If you are authenticating through an SDK or other wrapper, call the method with a curl command. Using curl can help isolate whether you have an authentication issue.

    ```sh
    curl -X {request_method} -u "apikey:{apikey}" "{url}/{method}"
    ```
    {: pre}

For more information, see the **Authentication** section of the [API reference](/docs?tab=api-docs&category=ai) for your service.

If this error persists and your service plan covers it, you can get help by creating a case from [IBM Cloud Support](https://{DomainName}/unifiedsupport/supportcenter){: external}.
