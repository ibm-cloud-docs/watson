---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-22"

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

# Service credentials for Watson services

Service credentials for an {{site.data.keyword.ibmwatson}} service provide authentication to a service in {{site.data.keyword.Bluemix}}. A set of credentials can be used only with the service for which they are created.
{: shortdesc}

You use the service credentials differently depending on the programming model you use. For details, see [Programming models for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-develop.html).

Service credentials are HTTP basic authentication credentials for a specific {{site.data.keyword.watson}} service.

## Getting credentials manually

You can find the service credentials from the {{site.data.keyword.Bluemix_notm}} web interface.

1.  Log in to [{{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Select **Catalog** and then **{{site.data.keyword.watson}}** to get to the list of services
1.  Select a service, and then create an instance of the service by clicking **Create**.
1.  Select **Service credentials** and then **View credentials** on the service dashboard.

See [Getting started with {{site.data.keyword.watson}} and {{site.data.keyword.Bluemix_notm}}](/docs/services/watson/index.html#get-service-credentials) for a visual walkthrough of this process
{: tip}

## Updating service credentials

You can get service credentials for an existing service instance from the {{site.data.keyword.Bluemix_notm}} interface:

1.  Log in to [{{site.data.keyword.Bluemix_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainNAme}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Select your service that you want to update from your {{site.data.keyword.Bluemix_notm}} dashboard.
1.  Under **Service credentials**, select the credential you want to delete.
1.  Click **New credential**, and then **Add** in the "Add new credential" window.
1.  View and copy the new credentials.
1.  Make sure that you use the new credentials in your applications:

    - If your application obtains its credentials programmatically, stop and restart your application.
    - If your credentials are embedded in your application code, update the code and relaunch the application.

For details about how to update an application with new credentials with minimal downtime, see [Updating Apps](/docs/manageapps/updapps.html) in the {{site.data.keyword.Bluemix_notm}} documentation.

## Credentials in Bluemix Dedicated

In a [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) instance, service tiles are implemented so that only one tile is permitted per service in an organization.

{{site.data.keyword.Bluemix_dedicated_notm}} users must follow these steps to refer to a service in their own workspace:

1.  Create a set of credentials for the reference tile for the service that they want to use.
1.  Create a custom user-provided service that uses those credentials:

    1.  Use the {{site.data.keyword.Bluemix_notm}} interface to select the tile for the service that you want to use, and create a set of credentials for the service. The credentials are created in JSON format and contain separate fields for the URL at which to communicate with the service and for the username and password to use with that service.
    1.  Use the `cf cups` (create user-provided service) command to create a  custom service that is bound to those credentials. For example, given these credentials,

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }
    ```
    {: codeblock}

    create a custom user-provided service instance named `Personality-Insights-Std`:

    ```bash
    cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
    ```
    {: pre}

    > **Important:** Make sure to specify the username and password as a single string inside double quotation marks and separated by a colon. Also enclose both the username and password strings with double quotation marks, and escape them with a backslash.

### Using apps with Bluemix Dedicated

The {{site.data.keyword.watson}} sample apps and starter kits on GitHub include a **Deploy to {{site.data.keyword.Bluemix_notm}}** button. These buttons won't work in {{site.data.keyword.Bluemix_dedicated_notm}} installations because they refer to the public {{site.data.keyword.Bluemix_notm}} environment.

To use sample applications and starter kits in {{site.data.keyword.Bluemix_dedicated_notm}} environments:

1.  In the instructions for the application, make sure that the `manifest.yml` file that you create uses the name of the service that you created with the `cf cups` command.
1.  In those same instructions, instead of executing the `cf create-service` command, use a `cf cups` command. However, if you created a custom user-provided instance of your service, you don't need to create it again to use it with the sample application or starter kit.
