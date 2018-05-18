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

# Tokens für die Authentifizierung
Sie können Tokens zum Schreiben von Anwendungen verwenden, um authentifizierte Anforderungen an {{site.data.keyword.ibmwatson}}-Services zu senden, ohne dass die Serviceberechtigungsnachweise in jedem Aufruf integriert werden.
{: shortdesc}

Sie können einen Authentifizierungsproxy in {{site.data.keyword.cloud}} schreiben, der ein Token an Ihre Clientanwendung abruft und zurückgibt, die dann den Token verwenden kann, um den Service direkt aufzurufen. Dank des Proxys ist es nicht notwendig, alle Serviceanforderungen über eine zwischengeschaltete serverseitige Anwendung zu leiten. Dies wäre ansonsten erforderlich, um das Offenlegen Ihrer Serviceberechtigungsnachweise Ihrer Clientanwendung zu verhindern.

Weitere Informationen zum Programmiermodell mit direkter Interaktion finden Sie in [Programmiermodelle für {{site.data.keyword.watson}}-Services](/docs/services/watson/getting-started-develop.html).

Sie müssen über Berechtigungsnachweise für die HTTP-Basisauthentifizierung für einen Service verfügen, um ein Token für den Service zu erhalten. Weitere Informationen finden Sie in [Serviceberechtigungsnachweise für {{site.data.keyword.watson}}-Services](/docs/services/watson/getting-started-credentials.html). Sie erhalten ein Token für einen bestimmten Service und das Token funktioniert dann nur für den angegebenen Service.

## Ein Token anfordern
Um ein Token für einen Service anzufordern, senden Sie die HTTP-Abrufanforderung `GET` an die `token`-Methode für die `authorization`-API. Der Host ist vom verwendeten Service abhängig. Sie können den Host über den Endpunkt für die API des Services identifizieren.

- **gateway.watsonplatform.net**: Services wie {{site.data.keyword.personalityinsightsshort}} rufen die `token`-Methode am Endpunkt `https://gateway.watsonplatform.net/authorization/api/v1/token` ab. 
- **stream.watsonplatform.net**: Services wie {{site.data.keyword.speechtotextshort}} und {{site.data.keyword.texttospeechshort}} rufen die `token`-Methode unter "https://stream.watsonplatform.net/authorization/api/v1/token" ab.

Um den Service zu identifizieren, für den Sie ein Token erhalten möchten, übergeben Sie die Basis-URL des Services über den Abfrageparameter `url` der Methode. Beispiel: Übergeben Sie den folgenden Wert für den Parameter `url`, um ein Token für den {{site.data.keyword.texttospeechshort}}-Service abzurufen:

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

Die Option `--user` übergibt die Verknüpfung *username* und *password* für den Service, die Sie von {{site.data.keyword.cloud_notm}} oder aus der Umgebungsvariablen `VCAP_SERVICES` für eine Anwendung abrufen können, die an eine Instanz des Services gebunden ist. Verwenden Sie Ihre Serviceberechtigungsnachweise, nicht aber Ihre Anmelde-ID und Ihr Kennwort für {{site.data.keyword.cloud_notm}}.

Die Methode gibt das Token als eine Base64-Codierung verschlüsselter Nutzdaten von einem Kilobyte zurück.

Token haben eine Lebenszeit von einer Stunde, nach der Sie sie nicht mehr verwenden können, um eine Verbindung mit dem Service herzustellen. Bestehende Verbindungen, die bereits mit dem Token hergestellt wurden, sind von dieser Zeitlimitüberschreitung nicht betroffen. Der Versuch, ein abgelaufenes oder ungültiges Token zu übergeben, löst den HTTP-Statuscode `401 Unauthorized` für {{site.data.keyword.cloud_notm}} aus. Ihr Anwendungscode muss vorbereitet sein, das Token als Antwort auf diesen Rückgabecode zu aktualisieren.

## Ein Token verwenden

Sie übergeben ein Token an die HTTP-Schnittstelle eines Services, indem Sie den Anforderungsheader `X-Watson-Authorization-Token`, den Abfrageparameter `watson-token` oder ein Cookie verwenden. Der Token muss mit jeder HTTP-Anforderung übergeben werden.

Die folgenden curl-Beispiele verdeutlichen die ersten zwei Ansätze.

- Rufen Sie ein Token für den {{site.data.keyword.texttospeechshort}}-Service ab und schreiben Sie ihn in eine Datei mit dem Namen `token`.

  ```bash
  curl -X GET --user {username}:{password}
  --output token
  "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
  ```
  {: pre}

- Übergeben Sie die Inhalte in die Datei `token` in einem Aufruf an die `synthesize`- Methode der {{site.data.keyword.texttospeechshort}}-API. Der Befehl übergibt das Token mithilfe des Headers `X-Watson-Authorization-Token`. Der zweite Befehl übergibt ihn mithilfe des Abfrageparameters `watson-token`. Die Funktionen der zwei Befehle sind identisch. Jeder Befehl schreibt die Ausgabe des Aufrufs in eine Datei mit dem Namen `hello_world.wav`.

    - *Leiten Sie aus einer UNIX- oder Linux-Shell* die Inhalte der Datei `token` an den Befehl weiter:

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

    - *Integrieren Sie aus einer Windows-Eingabeaufforderung* die Inhalte der Datei `token` in den Befehl:

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
