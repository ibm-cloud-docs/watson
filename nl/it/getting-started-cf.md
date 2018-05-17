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

# CLI (command-line interface) Cloud Foundry

Puoi utilizzare lo strumento CLI (command line interface) `cf` per lavorare con le applicazioni {{site.data.keyword.cloud}} sul tuo sistema locale. Lo strumento fornisce una ricca interfaccia per un'esperienza di sviluppo completa.
{: shortdesc}

Per ulteriori informazioni sull'utilizzo degli strumenti Cloud Foundry per lo sviluppo di applicazioni, consulta [Funzionamento di Cloud Foundry con {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) e la [documentazione di Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.cloudfoundry.org/){: new_window}.

## Installazione della CLI (command-line interface) cf

Puoi trovare la versione più recente dello strumento nella sezione **Downloads** del [repository GitHub di Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Vedrai due tipi di pacchetti:

- I programmi di installazione (**Installers**) si integrano con lo strumento di gestione dei pacchetti della tua piattaforma o con il sistema di traccia del software installato per installare lo strumento nella sua ubicazione "ufficiale".
- I file binari (**Binaries**) forniscono i file di archivio che puoi installare in qualsiasi ubicazione.

Assicurati di includere il comando nella tua variabile di ambiente `PATH` dopo che hai installato lo strumento.
{: tip}

## Comandi per le istanze del servizio

Comandi `cf` utili per gestire le istanze del servizio {{site.data.keyword.watson}} esistenti in {{site.data.keyword.cloud_notm}}:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services) Elenca delle istanze del servizio nello spazio corrente.

  ```bash
  cf services
  ```
  {: pre}

  Se conosci il nome del servizio, utilizza il comando `cf service` per visualizzare informazioni ad esso relative. Ad esempio,

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): Crea un'istanza del servizio che puoi associare alla tua applicazione che esegui in {{site.data.keyword.cloud_notm}}:

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - Per gli argomenti *{service-name}* e *{service-plan}*, usa l'output del comando `cf marketplace`.
    - Specifica il nome dell'istanza del servizio ({service-instance-name}) che vuoi creare. Per molte applicazioni, il nome è specificato in `manifest.yml` o in un altro file di configurazione per l'applicazione.

- **cf delete-service**: Elimina un'istanza del servizio:

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    Eliminando un servizio di cui non hai più bisogno, liberi risorse in {{site.data.keyword.cloud_notm}} che non ti servono.

## Comandi per distribuire ed eseguire applicazioni

Comandi `cf` comuni per distribuire ed eseguire applicazioni in {{site.data.keyword.cloud_notm}}:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): Specifica l'ubicazione delle API {{site.data.keyword.watson}}:

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login). Accedi a {{site.data.keyword.cloud_notm}} con i tuoi {{site.data.keyword.ibmid}} e password:

  ```bash
  cf login -u {nomeutente} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): Elenca i servizi nel catalogo {{site.data.keyword.cloud_notm}} a cui puoi connetterti:

  ```bash
  cf marketplace
  ```
  {: pre}

  L'output include il nome di ciascun servizio e l'elenco di piani in cui è disponibile il servizio. Queste informazioni ti servono quando crei un'istanza del servizio per la tua applicazione in esecuzione in {{site.data.keyword.cloud_notm}}.

  Se conosci il nome del servizio, usa l'opzione `-s`. Ad esempio,

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): Distribuisci o aggiorna un'applicazione a {{site.data.keyword.cloud_notm}}. L'output include l'URL per accedere alla tua applicazione.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  Il comando utilizza spesso `manifest.yml` o un altro file di configurazione per trovare il file di cui eseguire il push (ad esempio, un file `.war` per Java oppure un file `.zip` per Node.js). Tuttavia, puoi specificare il nome percorso del file applicazione utilizzando l'opzione `-p`.

## Comandi per le applicazioni esistenti

Comandi `cf` utili per gestire le applicazioni esistenti in {{site.data.keyword.cloud_notm}}:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps) Elenca le applicazioni distribuite nello spazio corrente.

  ```bash
  cf apps
  ```
  {: pre}

  Includi il nome dell'applicazione per visualizzare informazioni dettagliate sull'integrità e sullo stato di un'applicazione:

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: Ricarica e riavvia la tua applicazione in {{site.data.keyword.cloud_notm}}:

  ```bash
  cf restage {application-name}
  ```
  {: pre}

Questo comando riavvia la tua applicazione così come esiste in {{site.data.keyword.cloud_notm}}. Per caricare la versione più recente dell'applicazione in {{site.data.keyword.cloud_notm}} prima di riavviarla, usa invece il comando `cf push`.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete) Elimina un'applicazione.

  ```bash
  cf delete application-name
  ```
  {: pre}

  Eliminando un'applicazione, liberi memoria e quota di {{site.data.keyword.cloud_notm}} da utilizzare per altre applicazioni.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service) Associa un servizio a un'applicazione.

  Di norma usi il comando `cf push` per associare un'istanza di un servizio a un'applicazione che ne fa uso. Puoi associare un'istanza del servizio esistente a un'applicazione esistente con il comando `cf bind-service`:

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Immetti quindi `cf restage` o `cf push` per riavviare l'applicazione.

- **cf unbind-service**: Rimuove l'associazione tra un'applicazione e il servizio:

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Potresti dover immettere il comando `cf restage` o `cf push` per riflettere la modifica.

  Questo comando rimuove le tue credenziali di autenticazione dalla variabile di ambiente `VCAP_SERVICES`. Usa `cf delete-service` o `cf delete` per rimuovere l'istanza del servizio o l'applicazione.
  {: tip}
