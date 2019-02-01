---

copyright:
  years: 2015, 2019
lastupdated: "2018-12-12"

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

# Authenticating with Cloud Foundry service credentials
{: #creating-credentials}

Cloud Foundry service credentials for {{site.data.keyword.ibmwatson}} services provide authentication to a service in {{site.data.keyword.cloud}}. Service credentials use HTTP basic authentication to authenticate to a specific {{site.data.keyword.watson}} service.
{: shortdesc}

{{site.data.keyword.watson}} services that are created in a resource group or are migrated from Cloud Foundry to a resource group use IAM authentication. By default, all new {{site.data.keyword.watson}} services use IAM authentication. For more information about resource groups and IAM authentication, see [Authenticating with IAM tokens](/docs/services/watson/getting-started-iam.html).
{: note}

## Getting Cloud Foundry service credentials
{: #getting-credentials-manually}

To access API methods by using Cloud Foundry service credentials, you must first collect the credentials. You can access the service credentials from the {{site.data.keyword.cloud_notm}} web interface.

1.  Log in to [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Go to your [resource list ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/dashboard){: new_window}.
1.  Select a service instance.
1.  Click **Show Credentials** to see the **Username**, **Password**, and **Url** for the credentials.

## Updating Cloud Foundry service credentials
{: #existing-svcs}

You can update service credentials for an existing service instance from the service dashboard.

1.  Log in to [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Go to your [resource list ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/dashboard){: new_window}.
1.  Select **Service credentials** from the navigation menu on the left of the page.
1.  Use the menus and icons to delete existing credentials or to add new credentials.

Make sure that you update the credentials in your applications for any changes.

## Credentials in {{site.data.keyword.Bluemix_dedicated_notm}}

In an [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) instance, service tiles are implemented so that only one tile is permitted per service in an organization.

Follow these steps to refer to a service in your own {{site.data.keyword.Bluemix_dedicated_notm}} workspace:

1.  Create a set of credentials for the reference tile for the service that you want to use.
1.  Create a custom user-provided service that uses those credentials:

    1.  Use the web interface to select the tile for the service that you want to use, and create a set of credentials for the service. The credentials are created in JSON format and include the service endpoint url, username, and password.
    1.  Use the `ibmcloud cf cups` (create user-provided service) command to create a custom service that is bound to those credentials. For example,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Given these credentials, create a custom user-provided service instance named `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Make sure to specify the username and password as a single string inside double quotation marks and separated by a colon. Also, enclose both the username and password strings with double quotation marks, and escape them with a backslash.
      {: tip}

### Using apps with {{site.data.keyword.Bluemix_dedicated_notm}}

Some {{site.data.keyword.watson}} sample apps and starter kits on GitHub include a **Deploy to {{site.data.keyword.cloud_notm}}** button. These buttons don't work in {{site.data.keyword.Bluemix_dedicated_notm}} installations because they refer to the public {{site.data.keyword.cloud_notm}} environment.

To use sample applications and starter kits in {{site.data.keyword.Bluemix_dedicated_notm}} environments, follow these steps:

1.  In the instructions for the application, make sure that the `manifest.yml` file that you create uses the name of the service that you created with the `ibmcloud cf cups` command.
1.  In those same instructions, instead of running the `ibmcloud cf create-service` command, use a `ibmcloud cf cups` command. However, if you created a custom user-provided instance of your service, you don't need to create it again to use it with the sample application or starter kit.
