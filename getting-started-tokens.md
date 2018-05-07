---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-07"

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

# Tokens for authentication
You use tokens to write applications that make authenticated requests to {{site.data.keyword.ibmwatson}} services without embedding service credentials in every call.
{: shortdesc}

You can write an authentication proxy in {{site.data.keyword.cloud}} that obtains and returns a token to your client application, which can then use the token to call the service directly. This proxy eliminates the need to channel all service requests through an intermediate server-side application, which is otherwise necessary to avoid exposing your service credentials from your client application.

For more information about the direct-interaction programming model, see [Programming models for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-develop.html).

You must have HTTP basic authentication credentials for a service to obtain a token for the service; for more information, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html). You obtain a token for a specific service, and the token works only for the specified service.

## Obtaining a token
To obtain a token for a service, you send an HTTP `GET` request to the `token` method of the `authorization` API. The host depends on the service your are using. You can identify the host from the endpoint for the service's API.

- **gateway.watsonplatform.net**: Services such as {{site.data.keyword.personalityinsightsshort}} call the `token` method at the endpoint `https://gateway.watsonplatform.net/authorization/api/v1/token`
- **stream.watsonplatform.net**: Services such as {{site.data.keyword.speechtotextshort}} and {{site.data.keyword.texttospeechshort}} call the `token` method at https://stream.watsonplatform.net/authorization/api/v1/token`

To identify the service for which you want to obtain a token, you pass the service's base URL via the method's `url` query parameter. For example, pass the following value for the `url` parameter to fetch a token for the {{site.data.keyword.texttospeechshort}} service:

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

You pass your HTTP basic authentication credentials for the service with the request just as you would for any call that does not use tokens. For example, the following curl command obtains a token for the {{site.data.keyword.texttospeechshort}} service:

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

The `--user` option passes the concatenation of your *username* and *password* for the service, which you can obtain from {{site.data.keyword.cloud_notm}} or from the `VCAP_SERVICES` environment variable for an application that is bound to an instance of the service. Make sure to use your service credentials, not your {{site.data.keyword.cloud_notm}} login ID and password.

The method returns the token as a one-kilobyte Base64 encoding of an encrypted payload.

Tokens have a time to live (TTL) of one hour, after which you can no longer use them to establish a connection with the service. Existing connections already established with the token are unaffected by the timeout. An attempt to pass an expired or invalid token elicits an HTTP `401 Unauthorized` status code from {{site.data.keyword.cloud_notm}}. Your application code needs to be prepared to refresh the token in response to this return code.

## Using a token

You pass a token to the HTTP interface of a service by using the `X-Watson-Authorization-Token` request header, the `watson-token` query parameter, or a cookie. You must pass the token with each HTTP request.

The following curl examples demonstrate the first two approaches.

- Obtain a token for the {{site.data.keyword.texttospeechshort}} service and writes it to a file named `token`.

  ```bash
  curl -X GET --user {username}:{password}
  --output token
  "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
  ```
  {: pre}

- Pass the contents of the `token` file in a call to the `synthesize` method of the {{site.data.keyword.texttospeechshort}} API. The first command passes the token via the `X-Watson-Authorization-Token` header. The second command passes it via the `watson-token` query parameter. The two commands are equivalent. Each command writes the output of the call to a file named `hello_world.wav`.

    - *From a UNIX or Linux shell,* redirect the contents of the `token` file to the command:

      ```bash
      curl -X POST \
      --header "X-Watson-Authorization-Token: $(< ./token)" \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
      ```
      {: pre}

      ```bash
      curl -X POST \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token=$(< ./token)"
      ```
      {: pre}

    - *From a Windows Command Prompt,* embed the contents of the `token` file in the command:

        ```bash
        curl -X POST
        --header "X-Watson-Authorization-Token: {token_string}"
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token={token_string}"
        ```
        {: pre}
