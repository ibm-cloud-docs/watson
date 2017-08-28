---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-21"

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

# VCAP\_SERVICES environment variable
{: #vcapServices}

The `VCAP_SERVICES` environment variable contains information that you can use to interact with a service instance in {{site.data.keyword.Bluemix}}. The fields in this environment variable are set when you bind a service to an application.
{: shortdesc}

Your application needs the URL and the HTTP basic authentication service credentials to authenticate with a {{site.data.keyword.watson}} service.

A running application typically reads these environment variables, after a service is bound to it, to extract the required name-value pairs. For information about the variables, see [Deploying apps](/docs/manageapps/depapps.html#app_env) and search for "Environment variables."

## Example
This example shows the `VCAP_SERVICES` environment variable for the Tradeoff Analytics service bound to an application:

```json
{
  "VCAP_SERVICES": {
    "tradeoff_analytics": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/tradeoff-analytics/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "tradeoff_analytics",
        "name": "tradeoff-analytics-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public"
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
| label    | The name associated with the service in {{site.data.keyword.Bluemix_notm}}                                            |
| name     | The name of the service instance                                                           |
| plan     | The plan available for the service in {{site.data.keyword.Bluemix_notm}}                                              |
| tags     | Additional information about the service                                                   |
## Viewing environment variables
You can retrieve information about the variable from either the Cloud Foundry tool or from within the {{site.data.keyword.Bluemix_notm}} interface.

- With the Cloud Foundry command-line tool: Use the `cf env` command after you have created a service and bound it to your application:

    ```bash
    cf env {application-name}
    ```
    {: pre}

- From the {{site.data.keyword.Bluemix_notm}} interface: Information about environment variables is available from the summary page for an application.
