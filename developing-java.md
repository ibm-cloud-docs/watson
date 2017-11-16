---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Developing a {{site.data.keyword.watson}} application in Java

To demonstrate the use of its REST API, each {{site.data.keyword.ibmwatson}} service provides sample web-based applications in Java in the {{site.data.keyword.watson}} GitHub project.
{: shortdesc}

## Before you begin

- Create an account on {{site.data.keyword.cloud_notm}}, or use an existing account. [Sign-up for free ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
- Install the [Cloud Foundry ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window} command-line client.  If you have it installed already, make sure that your version is up to date.
- Install the [Apache ant compiler ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://ant.apache.org/bindownload.cgi){: new_window}.  Expand (`unzip`, `gunzip`, or `untar`) the installation package and install the contents in a directory on your local machine. Make sure to include the `bin` directory for the compiler on your `PATH` environment variable after installation.
- Install the [IBM WebSphere Liberty profile ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/wasdev/downloads/){: new_window}.  You can download just the runtime version of the WebSphere Liberty profile or download a version as a plugin for the Eclipse IDE. Also install and configure either the *Java JDK* or the *Eclipse IDE*.

## Getting the source code
Visit the [{{site.data.keyword.watson}} GitHub ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud){: new_window} project to see the sample Java apps that are available for {{site.data.keyword.watson}}. Then use GitHub to clone a repository locally.

## Installing locally
If you want to modify the app or use it as a basis for building your own app, install it locally. You can then deploy your modified version of the app to {{site.data.keyword.Bluemix_notm}}.

### Setting up a {{site.data.keyword.watson}} service

1.  At the command line, go to the local project directory of the app you cloned.
1.  Log into {{site.data.keyword.Bluemix_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Find the name of the service and plan. If the `manifest.yml` file of the sample app includes a `declared-services` section, note the `label` and `plan`. Otherwise, use the `cf marketplace` command to get the name of the service and the plans:

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Create an instance of the service in {{site.data.keyword.Bluemix_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. For service-name and service-plan use the information from the manifest file or the `cf marketplace` command.

    For example, issue the following command to create the service instance for the Personality Insights service:

    ```bash
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Configuring the app environment

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
1.  Copy the values from the service key into the source file for the app, located in the app's working directory:

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### Building and running the app

These instructions use the runtime version of the WebSphere Liberty profile. You'll need to modify the steps if you use the Eclipse plugin.
{: tip}

1.  Build the app.
    1.  Change to the working directory of the app,
    1.  Run the Apache `ant` compiler. The first command cleans up from a previous build, and the second builds the app and generates the `webApp.war` file:

        ```java
        ant clean
        ant build
        ```
        {: codeblock}

1.  Issue the WebSphere Liberty profile `server create` command to create the server for your application. The name of the server specified as the argument to the command is arbitrary, but it must be unique in your local WebSphere Liberty profile environment.

    ```bash
    server create server-name
    ```
    {: pre}

1.  Copy the app's `.war` file to the directory `wlp-installation/usr/servers/server-name/dropins`, where *wlp-installation* is the base directory for your local installation of WebSphere Liberty profile, and *server-name* is the name you specified for the server in the previous step.
1.  Start the server in WebSphere Liberty profile. Specify the name of the server for the app as the argument to the command.

    ```bash
    server start server-name
    ```
    {: pre}

1.  Point your browser to http://localhost:9080/webApp to try out the app. The port is specified in the `app.js` file.

    The port is specified in the file `wlp-installation/usr/servers/server-name/server.xml`. To change the port change the value in `server.xml`, and then stop and restart the server.

### Troubleshooting

If the server fails to start properly or fails to respond, examine the log files in the directory `wlp-installation/usr/servers/server-name/logs` to determine the cause.

## Deploying to {{site.data.keyword.Bluemix_notm}}

You can use Cloud Foundry to deploy your local version of the app to {{site.data.keyword.Bluemix_notm}}.

1.  In the project root directory, open the manifest.yml file:
    1. In the `applications` section of the `manifest.yml` file, change the name value to a unique name for your version of the app.
    1. In the `services` section, specify the name of the {{site.data.keyword.watson}} service instance you created for your app. If you don't remember the service name, use the `cf services` command to list your services.

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

1.  Log into {{site.data.keyword.Bluemix_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Push the app to {{site.data.keyword.Bluemix_notm}} and specify the name of your app from the `manifest.yml` file. For example,

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Point your browser to the URL specified in the command output.
