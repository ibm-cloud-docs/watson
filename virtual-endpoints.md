---

copyright:
   years: 2020, 2021
lastupdated: "2021-12-09"

keywords: watsonplatform,virtual private endpoints

subcollection: watson

---

{{site.data.keyword.attribute-definition-list}}

# Virtual Private Endpoints
{: #virtual-private-endpoints}

{{site.data.keyword.cloud}} Virtual Private Endpoints (VPE) for VPC enables you to connect to supported {{site.data.keyword.cloud}} services from your VPC network by using the IP addresses of your choosing, allocated from a subnet within your VPC. See more details [here](https://cloud.ibm.com/docs/vpc?topic=vpc-about-vpe).

This document covers {{site.data.keyword.conversationshort}}, {{site.data.keyword.discoveryshort}}, {{site.data.keyword.speechtotextshort}}, and {{site.data.keyword.texttospeechshort}}.
{: note}

Virtual Private Endpoints (VPEs) are available for these services in Dallas, Washington, Frankfurt, London, Sydney, and Tokyo.
{: note}

## Prerequisites

- Have a [Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started)

- Have [private endpoints enabled](/docs/watson?topic=watson-public-private-endpoints#requirements-endpoints) for your service instance

## Instructions

- Create a VPE Gateway (VPEG) through the [UI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=ui), [CLI](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=cli), or [API](/docs/vpc?topic=vpc-ordering-endpoint-gateway&interface=api).

- Creation of the VPEG is confirmed when an IP address is set in the [details view of the VPEG page in the UI](https://cloud.ibm.com/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=ui), [CLI output of the `endpoint-gateway` command](https://cloud.ibm.com/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=cli), or [API details call](https://cloud.ibm.com/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=api).

You can verify by running `nslookup <endpoint>` on the private service endpoint of the {{site.data.keyword.watson}} service from your VPC, for example:

```bash
# nslookup api.private.us-south.assistant.watson.cloud.ibm.com 
Server:   127.0.0.53
Address:  127.0.0.53#53

Non-authoritative answer:
Name:   api.private.us-south.assistant.watson.cloud.ibm.com 
Address: 10.240.0.9   <---- your VPE IP address
```

To make requests using the assigned IP address instead of just the private service endpoint as suggested [here](/docs/vpc?topic=vpc-faqs-vpe#faq-access-using-cse-adn), you must do your own hostname resolution. For example:

```bash
curl -X POST "https://api.private.us-south.assistant.watson.cloud.ibm.com/v2/assistants" --connect ::10.240.0.9
```

```bash
curl -X POST "https://api.private.us-south.assistant.watson.cloud.ibm.com/v2/assistants" --resolve api.private.us-south.assistant.watson.cloud.ibm.com:443:10.240.0.9
```

## Additional links

- [IBM Cloud VPC](/docs/vpc)
- [VPE FAQ](/docs/vpc?topic=vpc-faqs-vpe)
- [Accessing the VPE after setup](/docs/vpc?topic=vpc-accessing-vpe-after-setup)
- [Viewing Details of a VPE Gateway](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway&interface=ui)
- [VPE Limitations](/docs/vpc?topic=vpc-limitations-vpe)
- [Full VPC CLI reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference)
