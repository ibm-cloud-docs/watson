---

copyright:
  years: 2015, 2022
lastupdated: "2022-12-09"

keywords: watson,curl

subcollection: watson

---

{{site.data.keyword.attribute-definition-list}}

# Using curl with {{site.data.keyword.watson}} examples
{: #using-curl}

The `curl` utility lets you make HTTP requests from the command line. The utility provides a simple interface for making requests that use `GET`, `POST`, and other HTTP methods. It lets you pass request headers, data, and other input, and it can display all of the response headers and data that a call returns. Because `curl` eliminates the need to write code to exercise REST APIs, {{site.data.keyword.ibmwatson}} documentation uses `curl` to demonstrate example calls.
{: shortdesc}

The `curl` utility is installed by default with recent versions of Apple® macOS® and Microsoft® Windows®.

-   For more information about installing or refreshing `curl` on macOS, Windows, or Linux®, see [Install curl](https://everything.curl.dev/get){: external}.
-   For more information about using `curl` and its full range of capabilities, see [Everything curl](https://everything.curl.dev/){: external}.

`curl` is not recommended for application development.
{: note}

## Using curl for secure communications
{: #using-curl-secure}

 To test whether `curl` is installed on your system, run the following command at a terminal prompt:

```sh
curl -V
```
{: codeblock}

If the output lists `https` among the supported protocols and `SSL` (Secure Sockets Layer) or `TLS` (Transport Layer Security) among the supported features, you're all set. All {{site.data.keyword.ibmwatson_notm}} Watson services use these features for secure connections between the client and server. A connection request is verified against the local certificate store to ensure authentication, integrity, and confidentiality.

If these protocols are not listed, you can use a self-signed certificate for your requests. But to make a successful connection, you need to disable SSL verification by including the `--insecure` or `-k` option with the request.

Enabling SSL verification is highly recommended. Disabling SSL jeopardizes the security of the connection and data. Disable SSL only if necessary and only with data that is not confidential, and take steps to enable SSL as soon as possible.
{: important}

## Example curl requests
{: #using-curl-examples}

The following examples demonstrate the use of `curl`. The requests use the {{site.data.keyword.speechtotextshort}} service to transcribe audio to text with both {{site.data.keyword.cloud}} and {{site.data.keyword.icp4dfull}}.

![IBM Cloud only](images/ibm-cloud.png) **{{site.data.keyword.cloud_notm}}**

```sh
curl -X POST -u "apikey:{apikey}" \
--header "Content-Type: audio/flac" \
--data-binary ./audio-file.flac \
"{url}/v1/recognize"
```
{: pre}

![Cloud Pak for Data only](images/cloud-pak.png) **{{site.data.keyword.icp4dfull_notm}}**

```sh
curl -X POST \
--header "Authorization: Bearer {token}" \
--header "Content-Type: audio/flac" \
--data-binary ./audio-file.flac \
"{url}/v1/recognize"
```
{: pre}

In both examples, you replace `{url}` with the URL for your service instance. Authentication differs between {{site.data.keyword.cloud}}, where you replace `{apikey}` with the API key for your service instance, and {{site.data.keyword.icp4dfull}}, where you replace `{token}` with an access token that you generate for your service instance. For more information about authenticating to the service on either platform, see [Authenticating to Watson services](/docs/watson?topic=watson-iam).

When submitting requests based on examples in the documentation, omit the braces (`{ }`) from the examples; they indicate variable values that you must replace with actual values. Windows users must replace the backslash (`\`) at the end of each line with a caret (`^`). Make sure there are no trailing spaces after a `\` or `^`.
{: tip}
