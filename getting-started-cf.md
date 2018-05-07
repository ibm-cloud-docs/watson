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

# Cloud Foundry command-line interface

You can use the `cf` command-line interface (CLI) tool to work with {{site.data.keyword.cloud}} applications on your local system. The tool provides a rich interface for a full development experience.
{: shortdesc}

For more information about working with Cloud Foundry tools for application development, see [How Cloud Foundry works with {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) and the [Cloud Foundry Documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.cloudfoundry.org/){: new_window}.

## Installing the cf command-line interface

You can find the latest version of the tool in the **Downloads** section of the [Cloud Foundry GitHub repository ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#downloads){: new_window}. You'll see two types of packages:

- **Installers** integrate with your platform's package management tool or installed software tracking system to install the tool into its "official" location.
- **Binaries** provide archive files that you can install into any location.

Make sure to include the command on your `PATH` environment variable after you install the tool.
{: tip}

## Commands for service instances

Useful `cf` commands for managing existing {{site.data.keyword.watson}} service instances in {{site.data.keyword.cloud_notm}}:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services) Lists of service instances in the current space.

  ```bash
  cf services
  ```
  {: pre}

  If you know the name of the service, use the `cf service` command to see information about it. For example,

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): Create an instance of the service you can bind to your application that you run in {{site.data.keyword.cloud_notm}}:

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - For *{service-name}* and *{service-plan}* arguments, use the output of the `cf marketplace` command.
    - Specify the {service-instance-name} that you want to create. For many applications, the name is specified in the `manifest.yml` or other configuration file for the application.

- **cf delete-service**: Delete a service instance:

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    By deleting a service that you no longer need, you free up resources in {{site.data.keyword.cloud_notm}} that you don't need.

## Commands to deploy and run applications

Common `cf` commands to deploy and run applications in {{site.data.keyword.cloud_notm}}:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): Specify the location of the {{site.data.keyword.watson}} APIs:

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login). Log into {{site.data.keyword.cloud_notm}} with your {{site.data.keyword.ibmid}} and password:

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): List the services in the {{site.data.keyword.cloud_notm}} catalog that you can connect to:

  ```bash
  cf marketplace
  ```
  {: pre}

  The output includes the name of each service and the list of plans under which the service is available. You need this information when you create an instance of the service for your application running in {{site.data.keyword.cloud_notm}}.

  If you know the name of the service, use the `-s` option. For example,

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): Deploy or update an application to {{site.data.keyword.cloud_notm}}. The output includes the URL to access your application.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  The command finds often uses the `manifest.yml` or other configuration file to find file to be pushed (for example, a `.war` file for Java or a `.zip` file for Node.js). However, you can specify the pathname of the application file by using `-p` option.

## Commands for existing applications

Useful `cf` commands for managing existing applications in {{site.data.keyword.cloud_notm}}:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps) List the applications that are deployed in the current space.

  ```bash
  cf apps
  ```
  {: pre}

  Include the application name to see detailed information about the health and status of an  application:

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: Reload and restart your application in {{site.data.keyword.cloud_notm}}:

  ```bash
  cf restage {application-name}
  ```
  {: pre}

This command restarts the application as it exists in {{site.data.keyword.cloud_notm}}. To upload the latest version of the application to {{site.data.keyword.cloud_notm}} before restarting it, use the `cf push` command instead.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete) Delete an application:

  ```bash
  cf delete application-name
  ```
  {: pre}

  By deleting an app, you free up {{site.data.keyword.cloud_notm}} memory and quota to use for other applications.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service) Binds a service to an application.

  Usually, you use the `cf push` command to bind an instance of a service to an application that uses it. You can bind an existing service instance to an existing application with `cf bind-service` command:

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Then issue `cf restage` or `cf push` to restart the application.

- **cf unbind-service**: Removes the association between an application and service:

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  You might need to issue the `cf restage` or `cf push` command to reflect the change.

  This command removes your authentication credentials from the `VCAP_SERVICES` environment variable. Use `cf delete-service` or `cf delete` to remove the service instance or the application.
  {: tip}
