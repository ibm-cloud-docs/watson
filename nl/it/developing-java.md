---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-09"

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

# Sviluppo di un'applicazione {{site.data.keyword.watson}} in Java

Per illustrare l'uso della sua API REST, ogni servizio {{site.data.keyword.ibmwatson}} fornisce delle applicazioni di esempio basate sul web in Java nel progetto {{site.data.keyword.watson}} GitHub.
{: shortdesc}

## Prima di cominciare

- Crea un account su {{site.data.keyword.cloud_notm}} o utilizza un account esistente. [Registrati gratuitamente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
- Installa il client riga di comando [Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}.  Se l'hai già installato, assicurati che la versione di cui disponi sia aggiornata.
- Scarica e installa [Apache Maven ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://maven.apache.org/download.cgi){: new_window}.
- Installa il [profilo IBM WebSphere Liberty ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/wasdev/downloads/){: new_window}.  Puoi scaricare solo la versione di runtime del profilo WebSphere Liberty oppure scaricare una versione come un plugin per l'IDE Eclipse. Installa e configura anche *SDK Java* o *IDE Eclipse*.

## Come ottenere il codice sorgente
Visita il progetto [{{site.data.keyword.watson}} GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud){: new_window} per vedere le applicazioni Java di esempio disponibili per {{site.data.keyword.watson}}. Usa quindi GitHub per clonare un repository localmente.

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
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### Configurazione dell'ambiente dell'applicazione

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
1.  Copia i valori dalla chiave del servizio nel file sorgente per l'applicazione che si trova nella directory di lavoro dell'applicazione:

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### Creazione ed esecuzione dell'applicazione

Queste istruzioni utilizzano la versione di runtime del profilo WebSphere Liberty. Dovrai modificare la procedura se usi il plugin Eclipse.
{: tip}

1.  Crea l'applicazione eseguendo questo comando Maven dalla directory di lavoro dell'applicazione:

    ```java
    mvn clean package
    ```
    {: pre}

1.  Immetti il comando `server create` del profilo WebSphere Liberty per creare il server per la tua applicazione. Il nome del server specificato come argomento per il comando è arbitrario ma deve essere univoco nel tuo ambiente di profilo WebSphere Liberty locale.

    ```bash
    server create server-name
    ```
    {: pre}

1.  Copia il file `.war` dell'applicazione nella directory `wlp-installation/usr/servers/server-name/dropins`, dove *wlp-installation* è la directory di base per la tua installazione del profilo WebSphere Liberty e *server-name* è il nome che hai specificato per il server nel passo precedente.
1.  Avvia il server nel profilo WebSphere Liberty. Specifica il nome del server per l'applicazione come argomento per il comando.

    ```bash
    server start server-name
    ```
    {: pre}

1.  Punta il tuo browser a http://localhost:9080/webApp per provare l'applicazione. La porta è specificata nel file `app.js`.

    La porta è specificata nel file `wlp-installation/usr/servers/server-name/server.xml`. Per cambiare la porta, modifica il valore in `server.xml` e quindi arresta e riavvia il server.

### Risoluzione dei problemi

Se l'avvio del server non viene eseguito correttamente o se non risponde, esamina i file di log nella directory `wlp-installation/usr/servers/server-name/logs` per determinare la causa.

## Distribuzione a {{site.data.keyword.cloud_notm}}

Puoi usare Cloud Foundry per distribuire la tua versione locale dell'applicazione a {{site.data.keyword.cloud_notm}}.

1.  Nella directory root del progetto, apri il file manifest.yml:
    1. Nella sezione `applications` del file `manifest.yml`, modifica il valore name in un nome univoco per la tua versione dell'applicazione.
    1. Nella sezione `services`, specifica il nome dell'istanza del servizio {{site.data.keyword.watson}} che hai creato per la tua applicazione. Se non ricordi il nome del servizio, utilizza il comando `cf services` per elencare i tuoi servizi.

        Il seguente esempio mostra un file `manifest.yml` modificato:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: tiered
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

1.  Esegui il push dell'applicazione in {{site.data.keyword.cloud_notm}} e specifica il nome della tua applicazione dal file `manifest.yml`. Ad esempio,

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Punta il tuo browser all'URL specificato nell'output del comando.
