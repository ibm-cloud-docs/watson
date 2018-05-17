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

# Token per l'autenticazione
Usi i token per scrivere applicazioni che eseguono richieste autenticate ai servizi {{site.data.keyword.ibmwatson}} senza integrare le credenziali del servizio in ogni chiamata.
{: shortdesc}

Puoi scrivere un proxy di identificazione in {{site.data.keyword.cloud}} che ottiene e restituisce un token alla tua applicazione client, che può quindi utilizzare il token per richiamare il servizio direttamente. Grazie a questo proxy, non è necessario incanalare tutte le richieste del servizio tramite un'applicazione lato client intermedia, che è altrimenti necessaria per evitare di esporre le tue credenziali del servizio dalla tua applicazione client.

Per ulteriori informazioni sul modello di programmazione a interazione diretta, consulta [Modelli di programmazione per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

Devi disporre delle credenziali di autenticazione di base HTTP per un servizio per ottenere un token per il servizio; per ulteriori informazioni, consulta [Credenziali del servizio per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html). Ottieni on token per uno specifico servizio e il token funziona solo per il servizio specificato.

## Ottenimento di un token
Per ottenere un token per un servizio, invii una richiesta HTTP `GET` al metodo `token` dell'API `authorization`. L'host dipende dal servizio che stai utilizzando. Puoi identificare l'host dall'endpoint per l'API del servizio.

- **gateway.watsonplatform.net**: i servizi quali {{site.data.keyword.personalityinsightsshort}} richiamano il metodo `token` all'endpoint `https://gateway.watsonplatform.net/authorization/api/v1/token`
- **stream.watsonplatform.net**: i servizi quali {{site.data.keyword.speechtotextshort}} e {{site.data.keyword.texttospeechshort}} richiamano il metodo `token` a https://stream.watsonplatform.net/authorization/api/v1/token`

Per identificare il servizio per cui vuoi ottenere un token, passi l'URL di base del servizio tramite il parametro di query `url` del metodo. Ad esempio, passa il seguente valore per il parametro `url` per recuperare un token per il servizio {{site.data.keyword.texttospeechshort}}:

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

Passi le tue credenziali di autenticazione di base HTTP per il servizio con la richiesta come faresti per qualsiasi chiamata che non utilizza i token. Ad esempio, il seguente comando curl ottiene un token per il servizio {{site.data.keyword.texttospeechshort}}:

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

L'opzione `--user` passa la concatenazione dei tuoi *username* e *password* per il servizio, che puoi ottenere da {{site.data.keyword.cloud_notm}} o dalla variabile di ambiente `VCAP_SERVICES` per un'applicazione associata a un'istanza del servizio. Assicurati di utilizzare le tue credenziali del servizio, non il tuo ID di accesso e la tua password di {{site.data.keyword.cloud_notm}}.

Il metodo restituisce il token come una codifica a Base64 di un kilobyte di un payload crittografato.

I token hanno un TTL (Time-to-live) di un'ora, dopo di che non puoi più utilizzarli per stabilire una connessione con il servizio. Le connessioni esistenti già stabilite con il token non sono influenzate dal timeout. Il tentativo di passare un token scaduto o non valido provoca un codice di stato `401 Unauthorized` da {{site.data.keyword.cloud_notm}}. Il tuo codice applicativo deve essere preparato per aggiornare il token in risposta a questo codice di ritorno.

## Utilizzo di un token

Passi un token all'interfaccia HTTP di un servizio utilizzando l'intestazione della richiesta `X-Watson-Authorization-Token`, il parametro di query `watson-token` oppure un cookie. Devi passare il token con ogni richiesta HTTP.

I seguenti esempi curl illustrano i primi due approcci.

- Ottieni un token per il servizio {{site.data.keyword.texttospeechshort}} e scrivilo in un file denominato `token`.

  ```bash
  curl -X GET --user {username}:{password}
  --output token
  "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
  ```
  {: pre}

- Passa il contenuto del file `token` in una chiamata al metodo `synthesize` dell'API {{site.data.keyword.texttospeechshort}}. Il primo comando passa il token tramite l'intestazione `X-Watson-Authorization-Token`. Il secondo comando lo passa tramite il parametro di query `watson-token`. I due comandi sono equivalenti. Ogni comando scrive l'output della chiamata in un file denominato `hello_world.wav`.

    - *Da una shell UNIX o Linux, * reindirizza il contenuto del file `token` al comando:

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

    - *Da un prompt dei comandi Windows, * integra il contenuto del file `token` nel comando:

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
