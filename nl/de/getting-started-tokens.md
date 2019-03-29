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

# Watson-Tokens
{: #gs-tokens-watson-tokens}

{{site.data.keyword.watson}}-Tokens werden zum Schreiben von Anwendungen verwendet, die authentifizierte Anforderungen an {{site.data.keyword.ibmwatson}}-Services senden, ohne dass bei jedem Aufruf Cloud Foundry-Serviceberechtigungsnachweise integriert sein müssen. Damit ein Service ein {{site.data.keyword.watson}}-Token abrufen kann, müssen Sie Cloud Foundry-Serviceberechtigungsnachweise verwenden. Weitere Informationen finden Sie in [Authentifizierung mit Cloud Foundry-Serviceberechtigungsnachweisen](/docs/services/watson?topic=watson-creating-credentials). Sie erhalten ein Token für eine bestimmte Serviceinstanz, das ausschließlich für diese Instanz verwendet werden kann.
{: shortdesc}

Standardmäßig verwendet {{site.data.keyword.cloud_notm}} die Identity and Access Management-Authentifizierung (IAM-Authentifizierung) für alle neuen Serviceinstanzen. Die hier beschriebenen Tokens können mit den Cloud Foundry-Serviceberechtigungsnachweisen verwendet werden. Weitere Informationen zur IAM-Authentifizierung finden Sie in [Authentifizierung mit IAM-Tokens](/docs/services/watson?topic=watson-iam#iam).
{: important}

Sie können einen Authentifizierungsproxy in {{site.data.keyword.cloud}} schreiben, der ein Token an Ihre Clientanwendung abruft und zurückgibt, die dann den Token verwenden kann, um den Service direkt aufzurufen. Dank des Proxys ist es nicht notwendig, alle Serviceanforderungen über eine zwischengeschaltete serverseitige Anwendung zu leiten. Dies wäre ansonsten erforderlich, um das Offenlegen Ihrer Serviceberechtigungsnachweise Ihrer Clientanwendung zu verhindern.

## Ein Token anfordern
{: #gs-tokens-obtain}

Um ein Token für einen Service anzufordern, senden Sie die HTTP-Abrufanforderung `GET` an die `token`-Methode für die `authorization`-API. Der Host ist vom jeweiligen Standort und vom verwendeten Service abhängig. Sie können den Host über den Endpunkt für die API des Services identifizieren.

Um den Service zu identifizieren, für den Sie ein Token erhalten möchten, übergeben Sie die Basis-URL des Services über den Abfrageparameter `url` der Methode. Der folgende curl-Befehl verwendet z. B. den Parameter `url`, um ein Token für den {{site.data.keyword.texttospeechshort}}-Service abzurufen:

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

Sie übergeben Ihre Berechtigungsnachweise für die HTTP-Basisauthentifizierung für den Service mit der Anforderung genauso wie bei einem Aufruf, der keine Token verwendet. Beispiel: Der folgende curl-Befehl ruft ein Token für den {{site.data.keyword.texttospeechshort}}-Service ab:

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

Mit der Option `--user` wird die Verknüpfung Ihres *Benutzernamens* und Ihres *Kennworts* für den Service übergeben. Sie können die Berechtigungsnachweise von {{site.data.keyword.cloud_notm}} oder aus der Umgebungsvariablen `VCAP_SERVICES` für eine Anwendung abrufen, die an eine Instanz des Service gebunden ist. Verwenden Sie Ihre Serviceberechtigungsnachweise, nicht aber Ihre Anmelde-ID und Ihr Kennwort für {{site.data.keyword.cloud_notm}}.

Die Methode gibt das Token als Base64-Codierung verschlüsselter Nutzdaten von einem Kilobyte zurück. 

Token haben eine Lebensdauer (TTL) von 1 Stunde, nach der sie nicht mehr verwendet werden können, um eine Verbindung zum Service herzustellen. Vorhandene Verbindungen, die bereits mit dem Token erstellt wurden, werden durch das Zeitlimit nicht beeinflusst. Der Versuch, ein abgelaufenes oder ungültiges Token zu übergeben, löst den HTTP-Statuscode `401 Unauthorized` für {{site.data.keyword.cloud_notm}} aus. Ihr Anwendungscode muss vorbereitet sein, das Token als Antwort auf diesen Rückgabecode zu aktualisieren.

## Ein Token verwenden
{: #gs-tokens-watson-use}

Sie übergeben ein Token an die HTTP-Schnittstelle eines Services, indem Sie den Anforderungsheader `X-Watson-Authorization-Token`, den Abfrageparameter `watson-token` oder ein Cookie verwenden. Der Token muss mit jeder HTTP-Anforderung übergeben werden.

Die folgenden curl-Beispiele verdeutlichen die ersten zwei Ansätze.

1.  Rufen Sie ein Token für den {{site.data.keyword.texttospeechshort}}-Service ab und schreiben Sie es in eine Datei mit dem Namen `token`. 

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  Übergeben Sie die Inhalte in die Datei `token` in einem Aufruf an die `synthesize`- Methode der {{site.data.keyword.texttospeechshort}}-API. Der Befehl übergibt das Token mithilfe des Headers `X-Watson-Authorization-Token`. Der zweite Befehl übergibt ihn mithilfe des Abfrageparameters `watson-token`. Die Funktionen der zwei Befehle sind identisch. Jeder Befehl schreibt die Ausgabe des Aufrufs in eine Datei mit dem Namen `hello_world.wav`.

    -   *Leiten Sie aus einer UNIX- oder Linux-Shell* die Inhalte der Datei `token` an den Befehl weiter:

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

    -   Integrieren Sie in einer *Windows-Eingabeaufforderung* den Inhalt der Datei `token` in den Befehl:

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
