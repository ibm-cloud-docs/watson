---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-13"

keywords: IAM tokens,IAM authentication,api key

subcollection: watson

---

{{site.data.keyword.attribute-definition-list}}

# Authenticating to {{site.data.keyword.watson}} services
{: #iam}

You use {{site.data.keyword.iamlong}} (IAM) to make authenticated requests to public {{site.data.keyword.ibmwatson}} services. With IAM access policies, you can assign access to more than one resource from a single key. In addition, a user, service ID, and service instance can hold multiple API keys.
{: shortdesc}

Add-ons to {{site.data.keyword.cpd_full_notm}} use a different authentication mechanism. For more information, see the documentation for your add-on.
{: tip}

## Credentials
{: #gs-iam-credentials}

To authenticate to a service through its API, pass your credentials to the API. You can pass either a bearer token in an authorization header or an API key.

- Authenticate with an IAM token.

    IAM tokens are temporary security credentials that are valid for up to 60 minutes. When a token expires, you generate a new one. Tokens can be useful for temporary access to resources. For more information, see [Generating an IBM Cloud IAM token by using an API key](/docs/account?topic=account-iamtoken_from_apikey).

- Authenticate with an {{site.data.keyword.cloud_notm}} API key, a service ID API key, or a service-specific API key.

    API keys are simple to use and don't automatically expire. Anyone with a valid key can access the resource. You can create separate API keys for different users, different applications, or to support key rotation scenarios. You can revoke API keys from the console without interfering with other API keys or the user.

For testing and development, you can pass in an API key directly. However, for production use, unless you use the {{site.data.keyword.watson}} SDKs, use an IAM token. When you pass an API key, the service looks up the API key details, so it might affect performance. For more information, see [Invoking IBM Cloud service APIs](/docs/account?topic=account-iamapikeysforservices).

The {{site.data.keyword.watson}} SDKs support both methods. For more information, see the **Authentication** section of the [API reference](https://{DomainName}/apidocs?category=ai){: external} for your service and SDK.

Users on Premium plans can also use {{site.data.keyword.keymanagementservicefull}} to control access to data. For more information, see [Protecting sensitive information in your {{site.data.keyword.watson}} service](/docs/watson?topic=watson-keyservice).

## About API keys
{: #gs-iam-keys}

Three types of API keys are supported by {{site.data.keyword.watson}} services:

- Service-specific API keys

    Service-specific keys are generated with the service. This kind of API key has access only to a specific service instance. To view service-specific keys, click the name of a {{site.data.keyword.watson}} service from your [resource list](https://{DomainName}/resources?groups=resource-instance){: external}.

- {{site.data.keyword.cloud_notm}} API keys

    {{site.data.keyword.cloud_notm}} API keys are associated with a user's identity. Only the user who is associated with the key can delete it. The same {{site.data.keyword.cloud_notm}} API key can be used to access different services. For more information about working with {{site.data.keyword.cloud_notm}} API keys, see [Managing user API keys](/docs/account?topic=account-userapikey).

- Service ID API keys

    Service IDs enable access to your IBM Cloud services by applications hosted both inside and outside of IBM Cloud. API keys that are associated with service IDs are granted the access that is associated with that service ID. For more information about service ID keys, see [Managing service ID API keys](/docs/account?topic=account-serviceidapikeys).

## API key best practices
{: #gs-iam-api-bp}

Keep your API keys secure to reduce the chance of publicly exposing credentials that compromise your account and applications. To help keep your API keys secure, follow these guidelines.

- Assign the most restrictive service role that works for the level of access you need.

    For example, assign the `Reader` service role for calls from your application to `GET` API methods. This role has only read-only access, so can't create or edit resources.

- Don't embed the API key directly in code.

    API keys that are embedded in code might be exposed to your users. Instead of embedding the API keys in code, store them either in environment variables or in files outside your source code control system.
- Don't store an API key in files inside your application's source code control system.

    If you store API keys in files, keep the files outside of your application's source code. This practice is important if you use a public source code management system such as GitHub.

- Regenerate or rotate your API keys.

    Create new keys periodically, or rotate your keys. And don't forget to delete keys that you no longer use.

## Next steps
{: #gs-iam-next-steps}

- Read an overview of [IBM Cloud IAM](/docs/account?topic=account-iamoverview)
- Learn about [managing access](/docs/account?topic=account-cloudaccess) in {{site.data.keyword.cloud_notm}}
- Dive into [policies, user roles, and permissions](/docs/account?topic=account-userroles)
- See how to [pass API keys and tokens](/docs/account?topic=account-iamapikeysforservices)
