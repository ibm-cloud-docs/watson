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

# Token di Watson
{: #gs-tokens-watson-tokens}

Utilizza i token {{site.data.keyword.watson}} per scrivere le applicazioni che effettuano richieste autenticate ai servizi {{site.data.keyword.ibmwatson}} senza integrare le credenziali del servizio Cloud Foundry in ogni chiamata. Devi utilizzare le credenziali del servizio Cloud Foundry per un servizio per ottenere un token {{site.data.keyword.watson}}. Per ulteriori informazioni, vedi [Autenticazione con le credenziali del servizio Cloud Foundry](/docs/services/watson?topic=watson-creating-credentials). Ottieni un token per una specifica istanza del servizio e il token funziona solo per tale istanza.
{: shortdesc}

Per impostazione predefinita, {{site.data.keyword.cloud_notm}} utilizza l'autenticazione IAM (Identity and Access Management) per tutte le nuove istanze del servizio. I token descritti qui utilizzano le credenziali del servizio Cloud Foundry. Per ulteriori informazioni sull'autenticazione IAM, vedi [Autenticazione con i token IAM](/docs/services/watson?topic=watson-iam#iam).
{: important}

Puoi scrivere un proxy di identificazione in {{site.data.keyword.cloud}} che ottiene e restituisce un token alla tua applicazione client, che può quindi utilizzare il token per richiamare il servizio direttamente. Grazie a questo proxy, non è necessario incanalare tutte le richieste del servizio tramite un'applicazione lato client intermedia, che è altrimenti necessaria per evitare di esporre le tue credenziali del servizio dalla tua applicazione client.

## Ottenimento di un token
{: #gs-tokens-obtain}

Per ottenere un token per un servizio, invii una richiesta HTTP `GET` al metodo `token` dell'API `authorization`. L'host dipende dall'ubicazione e dal servizio che stai utilizzando. Puoi identificare l'host dall'endpoint per l'API del servizio.

Per identificare il servizio per cui vuoi ottenere un token, passi l'URL di base del servizio tramite il parametro di query `url` del metodo. Ad esempio, il seguente comando curl utilizza il parametro `url` per recuperare un token per il servizio {{site.data.keyword.texttospeechshort}}:

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

L'opzione `--user` passa la concatenazione dei tuoi *username* e *password* per il servizio. Puoi ottenere le credenziali da {{site.data.keyword.cloud_notm}} o dalla variabile di ambiente `VCAP_SERVICES` per un'applicazione associata a un'istanza del servizio. Assicurati di utilizzare le tue credenziali del servizio, non il tuo ID di accesso e la tua password di {{site.data.keyword.cloud_notm}}.

Il metodo restituisce il token come una codifica a Base64 di un kilobyte di un payload crittografato. 

I token hanno un TTL (Time-to-live) di un'ora, dopo di che non puoi più utilizzarli per stabilire una connessione con il servizio. Le connessioni esistenti già stabilite con il token non sono influenzate dal timeout. Il tentativo di passare un token scaduto o non valido provoca un codice di stato `401 Unauthorized` da {{site.data.keyword.cloud_notm}}. Il tuo codice applicativo deve essere preparato per aggiornare il token in risposta a questo codice di ritorno.

## Utilizzo di un token
{: #gs-tokens-watson-use}

Passi un token all'interfaccia HTTP di un servizio utilizzando l'intestazione della richiesta `X-Watson-Authorization-Token`, il parametro di query `watson-token` oppure un cookie. Devi passare il token con ogni richiesta HTTP.

I seguenti esempi curl illustrano i primi due approcci.

1.  Ottieni un token per il servizio {{site.data.keyword.texttospeechshort}} e scrivilo in un file denominato `token`.

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  Passa il contenuto del file `token` in una chiamata al metodo `synthesize` dell'API {{site.data.keyword.texttospeechshort}}. Il primo comando passa il token tramite l'intestazione `X-Watson-Authorization-Token`. Il secondo comando lo passa tramite il parametro di query `watson-token`. I due comandi sono equivalenti. Ogni comando scrive l'output della chiamata in un file denominato `hello_world.wav`.

    -   *Da una shell UNIX o Linux, * reindirizza il contenuto del file `token` al comando:

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

    -   *Da un prompt dei comandi Windows,* integra il contenuto del file `token` nel comando:

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
