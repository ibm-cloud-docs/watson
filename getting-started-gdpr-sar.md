---

copyright:
  years: 2018, 2020
lastupdated: "2020-02-27"

keywords: GDPR,support,data rights

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:important: .important}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# GDPR Subject Access Request
{: #gdpr-sar}

See the following sections to request support for {{site.data.keyword.cloud}} {{site.data.keyword.watson}} resources:
{: shortdesc}

- For {{site.data.keyword.ibmwatson}} resources that are created in the European Union (EU), see [Requesting support for {{site.data.keyword.cloud_notm}} {{site.data.keyword.watson}} resources created in the European Union](#request-EU).
- For {{site.data.keyword.ibmwatson_notm}} resources that are created outside of the EU, see [Requesting support for resources outside the European Union](#request-non-EU).

## Requesting support for {{site.data.keyword.cloud_notm}} {{site.data.keyword.watson}} resources created in the European Union
{: #request-EU}

European Union support is provided 24 hours a day, 7 days a week by IBM support engineers who are located in Europe for customers who have created a resource in the Germany region and have selected the EU Supported option. Global teams provide additional support only at the discretion and under the direction of the EU support team. Global teams might be contacted, for example, when issues are not resolved by the Level 1 or Level 2 support team in the EU and expertise is needed from a global Level 3 support team member.

You can specify that you want support for your account from the support team that is physically located in Europe if the following criteria are true:

- Your master user or account owner set the EU Supported option for your account.
- Your resources are in the appropriate European data center.
- You select the EU supported ticket level when you open the ticket.

For the {{site.data.keyword.cloud_notm}} platform, only services that are hosted in the Germany region with the EU Supported option can be supported by a team that is physically located in Europe.

Setting the EU Supported option for your account applies to all future tickets that you open for issues on any service or data center that is hosted in the EU region. Any tickets that you opened prior to setting this option are not affected. If you set this option and you add resources outside of an EU data center or the Germany region, issues for those resources are not necessarily handled by a support team in Europe.

For more information about setting up your account to be EU supported, see [Enabling the EU supported option](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported).

### Data rights requests
{: #request-EU-data-rights}

A data rights (DR) request to {{site.data.keyword.IBM_notm}} covers two main areas:

- Copy of customer data: Deletion, correction, or access to customer data to help fulfill the rights of a data subject, as provided under legislation such as General Data Protection Regulation (GDPR).
- Copy of audit log: Access to audit log data by a data controller or owner for the offering that the controller is using.

Requesters are responsible for ensuring that they are authorized to request data on behalf of their organization.

## Requesting support for resources outside the European Union
{: #request-non-EU}

You can use this process to request support for {{site.data.keyword.cloud_notm}} {{site.data.keyword.watson}} resources outside of the EU.

## Opening a support request
{: #request-opening}

To meet a legislative requirement such as the GDPR, or for any of the following request types, submit a Subject Access Request (SAR) through the {{site.data.keyword.cloud_notm}} Service Portal. The process applies to resources created both inside or outside the European Union.

Types of Subject Access Requests

- Copy of customer data request
- Copy of audit log Request
- Dedicated account data delete request ({{site.data.keyword.visualrecognitionshort}} only)

Follow these steps to create a SAR.

1.  Log in to the [{{site.data.keyword.cloud_notm}} Service Portal](https://watson.service-now.com/wcp?id=csp_homepage){: external}. You need a {{site.data.keyword.cloud_notm}} account.
1.  Click **Create a Case**.
1.  Enter the {{site.data.keyword.watson}} services that are affected by the request. You can include multiple services in the same request.
1.  Add the following items to the **Case Description**:
    - Enter `DR Request` for data rights requests in the EU. For other requests, enter `GDPR SAR Request`.
    - Enter the type of request:
        - Customer Data request
        - Audit Log request. For audit log requests, specify the duration period: 30, 60, or 90 days.
        - Customer Data & Audit Log request
        - {{site.data.keyword.visualrecognitionshort}} Data Delete request
    - Include your {{site.data.keyword.cloud_notm}} account {{site.data.keyword.ibmid}}.
    - Include the GDPR user `customer_id` (used in the GDPR labeling process). Submit a separate request for each customer ID. If you have requests for multiple customers, create a separate request for each.

A few minutes after you open a support ticket, you will receive an email notification. Follow the instructions in the email for further communications about the issue.
