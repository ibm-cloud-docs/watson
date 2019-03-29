---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Watson tokens,Cloud Foundry

subcollection: watson

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Watson 記號
{: #gs-tokens-watson-tokens}

請使用 {{site.data.keyword.watson}} 記號撰寫應用程式，對 {{site.data.keyword.ibmwatson}} 服務提出已鑑別的要求，而不不要在每個呼叫中內嵌 Cloud Foundry 服務認證。您必須使用 Cloud Foundry 服務認證，服務才能取得 {{site.data.keyword.watson}} 記號。如需相關資訊，請參閱[使用 Cloud Foundry 服務認證進行鑑別](/docs/services/watson?topic=watson-creating-credentials)。您會取得特定服務實例的記號，該記號只適用於該實例。
{: shortdesc}

依預設，{{site.data.keyword.cloud_notm}} 會針對所有新服務實例使用 Identity and Access Management (IAM) 鑑別。這裡說明的記號會與 Cloud Foundry 服務認證一起作用。如需 IAM 鑑別的相關資訊，請參閱[使用 IAM 記號進行鑑別](/docs/services/watson?topic=watson-iam#iam)。
{: important}

您可以在 {{site.data.keyword.cloud}} 中撰寫鑑別 Proxy，以取得記號並將其傳回至用戶端應用程式，然後後者便可以利用記號來直接呼叫服務。有了這個 Proxy 便不需要透過中間伺服器端應用程式作為通道來傳送所有服務要求，在其他情況下，則需要這麼做以避免從用戶端應用程式公開服務認證。

## 取得記號
{: #gs-tokens-obtain}

若要取得服務的記號，請傳送 HTTP `GET` 要求給 `authorization` API 的 `token` 方法。主機會根據使用的位置和服務而定。您可以從服務的 API 端點識別主機。

若要識別您要取得其記號的服務，您可以透過方法的 `url` 查詢參數來傳遞服務的基本 URL。例如，下列 curl 指令會使用 `url` 參數提取 {{site.data.keyword.texttospeechshort}} 服務的記號：

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

使用要求傳遞服務的 HTTP 基本鑑別認證，就像對不使用記號的任何呼叫所做的事一樣。例如，下列 curl 指令會取得 {{site.data.keyword.texttospeechshort}} 服務的記號：

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

`--user` 選項會傳遞服務的*使用者名稱* 及*密碼* 的連結。您可以從 {{site.data.keyword.cloud_notm}} 取得認證，或從已連結至服務實例之應用程式的 `VCAP_SERVICES` 環境變數取得認證。請務必使用您的服務認證，而不是您的 {{site.data.keyword.cloud_notm}} 登入 ID 和密碼。

方法會將記號傳回為加密有效負載的 Base64 編碼，大小為 1KB。

記號具有 1 小時的存活時間 (TTL)，在那之後，您就不能使用它們來建立與服務的連線。已經使用記號建立的現有連線不會受到逾時的影響。嘗試傳遞過期或無效的記號，會引發來自 {{site.data.keyword.cloud_notm}} 的 HTTP `401 Unauthorized` 狀態碼。您的應用程式碼必須準備好重新整理記號，以回應這個回覆碼。

## 使用記號
{: #gs-tokens-watson-use}

您可以使用 `X-Watson-Authorization-Token` 要求標頭、`watson-token` 查詢參數或 Cookie，將記號傳遞至服務的 HTTP 介面。您必須與每個 HTTP 要求一起傳遞記號。

下列 curl 範例示範前兩種方法。

1.  取得 {{site.data.keyword.texttospeechshort}} 服務的記號，並將它寫入名為 `token` 的檔案。

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
    {: pre}

1.  在 {{site.data.keyword.texttospeechshort}} API 的 `synthesize` 方法呼叫中，傳遞 `token` 檔的內容。第一個指令會透過 `X-Watson-Authorization-Token` 標頭來傳遞記號。第二個指令透過 `watson-token` 查詢參數傳遞。這兩個指令相等。每一個指令都會將呼叫的輸出寫入名為 `hello_world.wav` 的檔案。

    -   *從 UNIX 或 Linux Shell，*將 `token` 檔的內容重新導向至指令：

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

    -   *從 Windows 命令提示字元，*將 `token` 檔的內容內嵌在指令中：

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
