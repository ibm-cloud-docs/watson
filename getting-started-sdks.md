---

copyright:
  years: 2015, 2021
lastupdated: "2022-04-08"

keywords: Watson SDKs,SDK,software developer kit,programming interfaces,wrappers

subcollection: watson

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.watson}} SDKs
{: #using-sdks}

SDKs abstract much of the complexity associated with application development. By providing programming interfaces in languages that you already know, they can help you get up and running quickly with {{site.data.keyword.ibmwatson_notm}} services.
{: shortdesc}

## Supported SDKs
{: #ibm-sdks}

The following {{site.data.keyword.watson}} SDKs are supported by {{site.data.keyword.IBM_notm}}:

* [Java SDK](https://github.com/watson-developer-cloud/java-sdk){: external}
* [Node.js SDK](https://github.com/watson-developer-cloud/node-sdk){: external}
* [Python SDK](https://github.com/watson-developer-cloud/python-sdk){: external}
* [.NET SDK](https://github.com/watson-developer-cloud/dotnet-standard-sdk){: external}

The [API reference](https://{DomainName}/docs?tab=api-docs&category=ai%2Ccloud_pak){: external} for each service includes information and examples for these SDKs.
{: tip}

## Community SDKs
{: #community-sdks}

The following SDKs are available from the {{site.data.keyword.watson}} community of developers:

* [ABAP SDK for IBM Watson](https://github.com/watson-developer-cloud/abap-sdk-nwas){: external}, using SAP NetWeaver
* [Android SDK](https://github.com/watson-developer-cloud/android-sdk){: external}
* [Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external}
* [Ruby SDK](https://github.com/watson-developer-cloud/ruby-sdk){: external}
* [Salesforce SDK](https://github.com/watson-developer-cloud/salesforce-sdk){: external}
* [Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external}
* [Unity SDK](https://github.com/watson-developer-cloud/unity-sdk){: external}

## SDK updates and deprecation
{: #sdk-schedule}

The supported {{site.data.keyword.watson}} SDKs are updated according to the following guidelines.

### Semantic versioning

Supported {{site.data.keyword.watson}} SDKs adhere to semantic versioning with releases labeled as `{major}.{minor}.{patch}`.

### Release frequency

SDKs are released independently and might not update on the same schedule.

* The current releases of the {{site.data.keyword.watson}} SDKs are updated on a 2- to 6-week schedule. These releases are either minor updates or patches that do not include breaking changes. You can update to any version of the SDK with the same major version number.
* Major updates that might include breaking changes are released approximately every 6 months.

### Deprecated release

When a major version is released, support continues on the previous major release for 12 months in a deprecation period. The deprecated release might be updated with bug fixes, but no new features will be added and documentation might not be available.

### Obsolete release

After the 12-month deprecation period, a release is obsolete. The release might be functional but is unsupported and not updated. Update to the current release.
