---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-02"

keywords: migrate,watson,Cloud Foundry

subcollection: watson

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Migrating from Cloud Foundry
{: #migrate}

Migrate a service instance to move it from its current Cloud Foundry org and space to a resource group.
{: shortdesc}

{{site.data.keyword.cloud_notm}} moved from using Cloud Foundry to using resource groups. Resource groups offer these benefits over Cloud Foundry:

- Finer-grained access control by using IBM Cloud Identity and Access Management (IAM)
- Ability to connect service instances to apps and services across different regions
- Simplifies the capture of usage data per group

If you created service instances before November 2018, then depending on the location in which your instance is hosted, it might be using Cloud Foundry instead of resource groups.

## Migrating a service instance
{: #migrate-task}

If you want to learn more about the migration process before you begin, see the [{{site.data.keyword.cloud_notm}} migration documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/resources?topic=resources-migrate){: new_window}.

Only the person or group that created the instance or that has the appropriate privileges can migrate it. For more information, see [Required access for service instances](/docs/resources?topic=resources-migrate#required_access_instances){: new_window}.
{: note}

To migrate your service instance, complete these steps:

1.  Determine which resource group you want to move the service instance to.

    See [Best practices for organizing resource in a resource group ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/resources?topic=resources-bp_resourcegroups) for tips.

    With a Premium plan, you can migrate multiple instances. However, the instances must all be migrated to the same resource group.

1.  From the *Cloud Foundry Services* section of the {{site.data.keyword.cloud_notm}} resource list, click the migrate icon ![Migrate](images/migrate.svg) for the instance you want to migrate, and then click **Migrate** from the pop-up.

1.  Click **Continue**, and then choose a resource group.

    If you did not create a resource group, you can create one now. A **default** resource group is available. Take time to understand how the groups are used and create one if necessary. The resource group that you choose here *cannot* be changed later.

1.  Click **Migrate**.

    A message is displayed when the process is done. If you have other service instances to migrate, you can continue migrating other service instances, or click **Done**.

1.  **Premium plan one-time step**: If the service instance you are migrating was created as part of a Premium plan, then you must inform the IBM support team that you are migrating a Premium plan instance. To do so, create a case from [IBM Cloud Support ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/unifiedsupport/supportcenter){: new_window}.

    There are some additional steps that the IBM support team needs to take on your behalf. Add the following information to the case:

    - Region in which the service instance is hosted, such as Dallas or Frankfurt
    - Resource group name
    - Resource group ID

      If you don't know the ID, open the {{site.data.keyword.cloud_notm}} command-line interface (CLI) tool and enter the following command:

      ```bash
      ibmcloud resource groups
      ```
      {: codeblock}

      The response shows the ID of the resource group. For more information about the CLI command, see [Working with resources and resource groups ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_commands_resource).

      Give the support team up to 24 hours from the time you submit the case to complete the work that they need to perform to migrate the instance. There is no disruption of service during the migration process.

      You need to complete this step for the first time that you migrate a service instance for the Premium plan only. When you click *Migrate* for other service instances that belong to the same plan, they are migrated to the same resource group without requiring any involvement from the IBM support team.
      {: important}

The old (Cloud Foundry org-based) service instance that you migrated continues to be listed in the *Cloud Foundry Services* section of the {{site.data.keyword.cloud_notm}} Resource list. The old service instance is now shown as an *alias* of the new (resource group-based) version of the instance.

The following image shows how a {{site.data.keyword.conversationshort}} service instance is listed after it is migrated.

![Shows that the current service instance is now an alias of a resource-based instance](images/alias.png)

For more information about aliases, see the [IBM Cloud Connections documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

You must open the new, resource group-based version of the service instance to be able to access any tools that are associated with it. The new instance is listed in the *Services* section of the {{site.data.keyword.cloud_notm}} Resource list.

If your service was syndicated originally, after the migration, it might be hosted in a different location than you expect. The URL of the migrated service indicates its location.
{: note}

## Authentication
{: #migrate-auth-support}

If you have existing applications that use basic authentication to access the service, they can continue to pass a user name and password to authenticate with the service instance after it is migrated. You can see the credentials information from the **Service credentials** page of the alias.

The basic authentication credentials will stop working on 30 October 2019. For more information, read [this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/bluemix/2018/09/ibm-watson-ai-services-enabling-iam-and-resource-groups/).
{: deprecated}

Your new instance manages authentication with IBM Cloud Identity and Access Management (IAM), which is an enhanced mechanism that uses API keys instead of user name and password credentials. You can see the API key information from the **Service credentials** page of the new service instance.

Update your client applications to take advantage of the improved security that IAM affords. After you update your apps to use the new API key approach, you won't need the alias and can delete it.

All existing custom applications must be updated to use the IAM authentication method before the Cloud Foundry-based authentication mechanism stops working on 30 October 2019.
{: important}
