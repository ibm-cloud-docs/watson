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
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# VCAP\_SERVICES environment variable
{: #vcapServices}

The `VCAP_SERVICES` environment variable contains information that you can use to interact with a service instance in {{site.data.keyword.cloud}}. The fields in this environment variable are set when you bind a service instance to an application.
{: shortdesc}

Your application needs the URL and the service credentials to authenticate with a {{site.data.keyword.watson}} service. A running application typically reads these environment variables after a service is bound to it to extract the required name-value pairs. You can bind either resource group or Cloud Foundry services to your app, but resource group services need a service alias.

## Service aliases for IAM services
{: #vcapIAMAlias}

You can use the `VCAP_SERVICES` environment variable with resource group services by giving the service an alias. For example, use the following command with the {{site.data.keyword.cloud_notm}} command-line interface to create an alias that is called `personality-insights-service` for a service called `my-personality-insights-service`.

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

You can then bind your app to the service alias. A running application typically reads these environment variables after a service instance is bound to it to extract the required name-value pairs.

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Example
{: #gs-variables-vcapIAMAliasExample}

This example shows the `VCAP_SERVICES` environment variable for the {{site.data.keyword.personalityinsightsshort}} service that is bound to an application. The service instance uses Cloud Foundry authentication.

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

The following table describes the contents of the `VCAP_SERVICES` environment variable.

| Item     | Description                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | The password from the HTTP basic authentication credentials used to connect to the service |
| url      | The connection URL for the service                                                         |
| username | The username from the HTTP basic authentication credentials used to connect to the service |
| label    | The name associated with the service in {{site.data.keyword.cloud_notm}}                                            |
| name     | The name of the service instance                                                           |
| plan     | The plan available for the service in {{site.data.keyword.cloud_notm}}                                              |
| tags     | Additional information about the service                                                   |

## Viewing environment variables
{: #gs-variables-vcapView}

You can retrieve information about the variable from either the {{site.data.keyword.cloud_notm}} command-line interface (CLI) or from the web interface.

- With the {{site.data.keyword.cloud_notm}} CLI: Invoke the Cloud Foundry `env` command after you create a service and bind it to your application:

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- From the {{site.data.keyword.cloud_notm}} interface: Information about environment variables is available from the summary page for an application.
