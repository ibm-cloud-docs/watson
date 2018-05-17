---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-02"

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

# Sviluppo di un'applicazione {{site.data.keyword.watson}} in Node.js

Per illustrare l'uso della sua API REST, ogni servizio {{site.data.keyword.ibmwatson}} fornisce delle applicazioni di esempio basate sul web in Node.js nel progetto {{site.data.keyword.watson}} GitHub.
{: shortdesc}

## Prima di cominciare

- Crea un account su {{site.data.keyword.cloud_notm}} o utilizza un account esistente. [Registrati gratuitamente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. Il tuo account deve avere spazio per almeno 1 applicazione e 1 servizio.
- Il [runtime Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://nodejs.org/#download){: new_window}, incluso il gestore pacchetti [npm](https://www.npmjs.com/){: new_window}.  Assicurati di includere il comando nella tua variabile di ambiente `PATH` dopo l'installazione.
- Il client riga di comando [Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Se l'avevi già installato in precedenza, assicurati che la versione di cui disponi sia aggiornata.

## Come ottenere il codice sorgente

Visita il progetto [{{site.data.keyword.watson}} Github ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud){: new_window} per vedere le applicazioni Node.js di esempio disponibili per {{site.data.keyword.watson}}. Usa quindi GitHub per clonare un repository localmente.

## Installazione in locale
Se vuoi modificare l'applicazione o usarla come base per sviluppare la tua applicazione, installala in locale. Puoi quindi distribuire la tua versione modificata dell'applicazione a {{site.data.keyword.cloud_notm}}.

### Configurazione di un servizio {{site.data.keyword.watson}}

1.  Sulla riga di comando, vai alla directory del progetto locale dell'applicazione che hai clonato.
1.  Accedi a {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Trova il nome del servizio e del piano. Se il file `manifest.yml` dell'applicazione di esempio include una sezione `declared-services`, prendi nota di `label` e `plan`. Altrimenti, usa il comando `cf marketplace` per ottenere il nome del servizio e dei piani.

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Crea un'istanza del servizio in {{site.data.keyword.cloud_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. Per service-name e service-plan, usa le informazioni dal file manifest o dal comando `cf marketplace`.

    Ad esempio, immetti questo comando per creare l'istanza del servizio per il servizio {{site.data.keyword.personalityinsightsshort}}:

    ```bash
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Configurazione dell'ambiente dell'applicazione

1.  Copia il file `.env.example` in un nuovo file `.env`.
1.  Crea una chiave del servizio nel formato `cf create-service-key {istanza_servizio} {chiave_servizio}`. Ad esempio:

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Richiama le credenziali dalla chiave del servizio usando il comando `cf service-key {istanza_servizio} {chiave_servizio}`. Ad esempio:

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    L'output da questo comando è un oggetto JSON, come in questo esempio:

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }
    ```
    {: codeblock}

1.  Incolla i valori `password` e `username` (senza virgolette) dal JSON nelle nuove variabili pertinenti nel file `.env`. Ad esempio:

    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### Installazione e avvio dell'applicazione

1.  Installa il pacchetto dell'applicazione demo nella variabile di runtime Node.js locale:

    ```bash
    npm install
    ```
    {: pre}

1.  Avvia l'applicazione:

    ```bash
    npm start
    ```
    {: pre}

1.  Punta il tuo browser a http://localhost:3000 per provare l'applicazione. La porta è specificata nel file `app.js`.

## Distribuzione a {{site.data.keyword.cloud_notm}}

Puoi usare Cloud Foundry per distribuire la tua versione locale dell'applicazione a {{site.data.keyword.cloud_notm}}.

1.  Nella directory root del progetto, apri il file manifest.yml:
    1.  Nella sezione `applications` del file `manifest.yml`, modifica il valore name in un nome univoco per la tua versione dell'applicazione.
    1.  Nella sezione `services`, specifica il nome dell'istanza del servizio {{site.data.keyword.watson}} che hai creato per la tua applicazione. Se non ricordi il nome del servizio, utilizza il comando `cf services` per elencare i tuoi servizi.

        Il seguente esempio mostra un file `manifest.yml` modificato:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Accedi a {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Esegui il push dell'applicazione in {{site.data.keyword.cloud_notm}} e specifica il nome della tua applicazione dal file `manifest.yml`. Ad esempio:

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Punta il tuo browser all'URL specificato nell'output del comando.
