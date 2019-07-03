---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-03"

keywords: migrate,Cloud Foundry,resource group

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# Migrating {{site.data.keyword.watson}} services from Cloud Foundry
{: #migrate}

Migrate a {{site.data.keyword.watson}} service instance to move it from its current Cloud Foundry org and space to a resource group.
{: shortdesc}

{{site.data.keyword.cloud_notm}} moved from using Cloud Foundry to using resource groups. Resource groups offer these benefits over Cloud Foundry:

- Finer-grained access control by using {{site.data.keyword.iamlong}}
- Ability to connect service instances to apps and services across different regions
- Simplified capture of usage data per group

If you created service instances before November 2018, then they might be using Cloud Foundry instead of resource groups.

## Migrating a service instance
{: #migrate-task}

To learn more about the migration process before you begin, see the [{{site.data.keyword.cloud_notm}} migration documentation](/docs/resources?topic=resources-migrate).

Only the person or group that created the instance or that has the appropriate privileges can migrate it. For more information, see [Required access for service instances](/docs/resources?topic=resources-migrate#required_access_instances).
{: note}

To migrate your service instance, complete these steps:

1.  Determine which resource group you want to move the service instance to.

    See [Best practices for organizing resource in a resource group](/docs/resources?topic=resources-bp_resourcegroups) for tips.

    With a Premium plan, you can migrate multiple instances. However, the instances must all be migrated to the same resource group.

1.  From the *Cloud Foundry Services* section of the {{site.data.keyword.cloud_notm}} [resource list](/resources?groups=cf-services){: external}, click the migrate icon ![Migrate](images/migrate.svg) for the instance you want to migrate, and then click **Migrate** from the pop-up window.

1.  Click **Continue**, and then choose a resource group.

    If you didn't create a resource group, you can create one now. A **default** resource group is available. Take time to understand how the groups are used and create one if necessary. You can't change the resource group later.

1.  Click **Migrate**.

    A message is displayed when the process is done. If you have other service instances to migrate, you can migrate those services now. Otherwise, click **Done**.

1.  **Premium plan one-time step**: If the service instance you are migrating is part of a Premium plan, inform support that you are migrating a Premium plan instance. The {{site.data.keyword.cloud_notm}} support team needs to take some additional steps on your behalf.

    Complete this step the first time that you migrate a service instance for the Premium plan only. When you click *Migrate* for other service instances that belong to the same plan, they are migrated to the same resource group without requiring any involvement from the IBM support team.
    {: important}

    Create a case from [{{site.data.keyword.cloud_notm}} Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}. Add the following information to the case:

    - Region in which the service instance is hosted, such as Dallas or Frankfurt
    - Resource group name
    - Resource group ID

        If you don't know the ID, open the {{site.data.keyword.cloud_notm}} command-line interface (CLI) tool and enter the following command:

        ```bash
        ibmcloud resource groups
        ```
        {: pre}

        The response shows the ID of the resource group. For more information about the CLI command, see [Working with resources and resource groups](/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_commands_resource).

    Give the support team up to 24 hours to complete the migration. There is no disruption of service during the process.

## The migrated service instance
{: #migrate-instance}

The original (Cloud Foundry org-based) service instance that you migrated continues to be listed in the *Cloud Foundry Services* section of the {{site.data.keyword.cloud_notm}} Resource list. The old service instance is now shown as an *alias* of the new (resource group-based) version of the instance.

The following image shows how a {{site.data.keyword.conversationshort}} service instance is listed after it is migrated.

![Shows that the current service instance is now an alias of a resource-based instance](images/alias.png)

For more information about aliases, see [Managing Connections](/docs/resources/connecting_apps?topic=resources-connect_app#what_is_alias).

### Details about the migrated instance
{: #migrate-working}

The new resource group-based version of the instance is listed in the [Services](/resources?groups=resource-instance){: external} section of the {{site.data.keyword.cloud_notm}} Resource list. Open the migrated instance to see the details.

- To access any associated tools.
- To view or manage credentials. You can add credentials by clicking **New credential** from the **Service credentials** page.
- To view the URL. If your service was originally syndicated, it might be hosted in a different location after the migration.

## Applications and authentication
{: #migrate-auth-support}

Your new instance manages authentication with {{site.data.keyword.iamshort}} (IAM), which is an enhanced mechanism that uses API keys instead of user name and password. You can see the API key information from the **Service credentials** page of the new service instance.

Applications that authenticate to the service with user name and password will continue to work until 30 October 2019.

Update your client applications to take advantage of the improved security that IAM affords. After you update your apps to use the new API key approach, you won't need the alias and can delete it.

The username and password credentials will stop working on 30 October 2019. All custom applications must be updated to use the IAM authentication method by then. For more information, read [this blog post](http://ibm.biz/watsoncf2iam){: external}.
{: important}
