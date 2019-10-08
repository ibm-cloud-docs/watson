---

copyright:
  years: 2015, 2019
lastupdated: "2019-10-08"

keywords: Watson SDKs,SDK,software developer kit,programming interfaces,wrappers

subcollection: watson

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
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

# {{site.data.keyword.watson}} SDKs
{: #using-sdks}

SDKs abstract much of the complexity associated with application development. By providing programming interfaces in languages that you already know, they can help you get up and running quickly with {{site.data.keyword.ibmwatson_notm}} services.
{: shortdesc}

## Supported SDKs
{: #ibm-sdks}

The following {{site.data.keyword.watson}} SDKs are supported by {{site.data.keyword.IBM_notm}}:

* [Android SDK](https://github.com/watson-developer-cloud/android-sdk){: external}
* [Go SDK](https://github.com/watson-developer-cloud/go-sdk){: external}
* [Java SDK](https://github.com/watson-developer-cloud/java-sdk){: external}
* [Node.js SDK](https://github.com/watson-developer-cloud/node-sdk){: external}
* [Python SDK](https://github.com/watson-developer-cloud/python-sdk){: external}
* [Ruby SDK](https://github.com/watson-developer-cloud/ruby-sdk){: external}
* [.NET SDK](https://github.com/watson-developer-cloud/dotnet-standard-sdk){: external}
* [Salesforce SDK](https://github.com/watson-developer-cloud/salesforce-sdk){: external}
* [Swift SDK](https://github.com/watson-developer-cloud/swift-sdk){: external}
* [Unity SDK](https://github.com/watson-developer-cloud/unity-sdk){: external}

The [API reference](https://{DomainName}/apidocs?category=ai){: external} for each service includes information and examples for many of the SDKs, including Java, Node.js, Python, Go , .Net, Ruby, and Swift.

## Community SDKs
{: #community-sdks}

The following SDKs are available from the {{site.data.keyword.watson}} community of developers:

* [ABAP SDK for IBM Watson](https://github.com/watson-developer-cloud/abap-sdk-nwas), using SAP NetWeaver

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
