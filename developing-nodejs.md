---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-02"

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

# Developing a {{site.data.keyword.watson}} application in Node.js

To demonstrate the use of its REST API, each {{site.data.keyword.ibmwatson}} service provides sample web-based applications in Node.js in {{site.data.keyword.watson}} GitHub project.
{: shortdesc}

## Before you begin

- Create an account on {{site.data.keyword.cloud_notm}}, or use an existing account. [Sign-up for free ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. Your account must have space for at least 1 app and 1 service.
- The [Node.js runtime ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nodejs.org/#download){: new_window},  including the [npm](https://www.npmjs.com/){: new_window}. package manager.  Make sure to include the command on your `PATH` environment variable after installation.
- The [Cloud Foundry ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window} command-line client.  If you installed it earlier, make sure that your version is up to date.

## Getting the source code

Visit the [{{site.data.keyword.watson}} Github ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud){: new_window} project to see the sample Node.js apps that are available for {{site.data.keyword.watson}}. Then use GitHub to clone a repository locally.

## Installing locally
If you want to modify the app or use it as a basis for building your own app, install it locally. You can then deploy your modified version of the app to {{site.data.keyword.cloud_notm}}.

### Setting up a {{site.data.keyword.watson}} service

1.  At the command line, go to the local project directory of the app you cloned.
1.  Log into {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Find the name of the service and plan. If the `manifest.yml` file of the sample app includes a `declared-services` section, note the `label` and `plan`. Otherwise, use the `cf marketplace` command to get the name of the service and the plans:

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Create an instance of the service in {{site.data.keyword.cloud_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. For service-name and service-plan use the information from the manifest file or the `cf marketplace` command.

    For example, issue the following command to create the service instance for the {{site.data.keyword.personalityinsightsshort}} service:

    ```bash
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Configuring the app environment

1.  Copy the `.env.example` file to a new `.env` file.
1.  Create a service key in the format `cf create-service-key {service_instance} {service_key}`. For example:

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Retrieve the credentials from the service key using the command `cf service-key {service_instance} {service_key}`. For example:

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    The output from this command is a JSON object, as in this example:

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }
    ```
    {: codeblock}

1.  Paste the `password` and `username` values (without quotation marks) from the JSON into the relevant variables in the `.env` file. For example:

    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### Installing and starting the app

1.  Install the demo app package into the local Node.js runtime environment:

    ```bash
    npm install
    ```
    {: pre}

1.  Start the app:

    ```bash
    npm start
    ```
    {: pre}

1.  Point your browser to http://localhost:3000 to try out the app. The port is specified in the `app.js` file.

## Deploying to {{site.data.keyword.cloud_notm}}

You can use Cloud Foundry to deploy your local version of the app to {{site.data.keyword.cloud_notm}}.

1.  In the project root directory, open the manifest.yml file:
    1.  In the `applications` section of the `manifest.yml` file, change the name value to a unique name for your version of the app.
    1.  In the `services` section, specify the name of the {{site.data.keyword.watson}} service instance you created for your app. If you don't remember the service name, use the `cf services` command to list your services.

        The following example shows a modified `manifest.yml` file:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Log into {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Push the app to {{site.data.keyword.cloud_notm}} and specify the name of your app from the `manifest.yml` file. For example, :

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Point your browser to the URL specified in the command output.
