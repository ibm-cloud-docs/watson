---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Credenziali del servizio per i servizi Watson

Le credenziali del servizio per un servizio {{site.data.keyword.ibmwatson}} forniscono l'autenticazione a un servizio in {{site.data.keyword.cloud}}. Un set di credenziali può essere utilizzato solo con il servizio per cui è stato creato.
{: shortdesc}

Utilizzi le credenziali del servizio in modo diverso, a seconda del modello di programmazione che usi. Per i dettagli, consulta [Modelli di programmazione per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

Le credenziali del servizio sono credenziali di autenticazione di base HTTP per uno specifico servizio {{site.data.keyword.watson}}.

## Ottenimento delle credenziali in modo manuale
{: #getting-credentials-manually}

Puoi trovare le credenziali del servizio dall'interfaccia web {{site.data.keyword.cloud_notm}}.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Vai ai tuoi servizi esistenti. ([Raggiungi ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Seleziona il menu in alto a sinistra e seleziona quindi **{{site.data.keyword.watson}}** per andare alla console {{site.data.keyword.watson}}.
    1.  Seleziona **Servizi esistenti** da **Servizi Watson** per visualizzare un elenco dei tuoi servizi e dei tuoi progetti.
1.  Visualizza le credenziali:
    - Se il servizio fa parte di un progetto, fai clic sul nome del progetto che contiene il servizio. Visualizza le credenziali dalla sezione **Credenziali** nella pagina dei dettagli del progetto.
    - Se il servizio non fa parte di un progetto, fai clic sul nome del servizio che vuoi visualizzare e seleziona quindi **Credenziali del servizio**.

## Aggiornamento delle credenziali del servizio
{: #existing-svcs}

Puoi aggiornare le credenziali del servizio per un'istanza del servizio esistente dal dashboard del servizio.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Vai ai tuoi servizi esistenti. ([Raggiungi ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Seleziona il menu in alto a sinistra e seleziona quindi **{{site.data.keyword.watson}}** per andare alla console {{site.data.keyword.watson}}.
    1.  Seleziona **Servizi esistenti** da **Servizi Watson** per visualizzare un elenco dei tuoi servizi.
1.  Selezionare il servizio che vuoi visualizzare o aggiornare e seleziona quindi **Credenziali del servizio**.
1.  Elimina o aggiungi credenziali:
    - Per eliminare delle credenziali, selezionale e seleziona quindi **Elimina credenziale**.
    - Per aggiungere delle credenziali, fai clic su **Nuova credenziale** e fai quindi clic su **Aggiungi** nella finestra "Aggiungi nuova credenziale". Visualizza e copia le nuove credenziali.

Assicurati di utilizzare le nuove credenziali nelle tue applicazioni:

- Se la tua applicazione ottiene le sue credenziali in modo programmatico, arresta e riavvia la tua applicazione.
- Se le tue credenziali sono integrate nel tuo codice dell'applicazione, aggiorna il codice e riavvia l'applicazione.

Per i dettagli su come aggiornare un'applicazione con delle nuove credenziali con un tempo di inattività minimo, consulta [Aggiornamento di applicazioni](/docs/manageapps/updapps.html).

## Credenziali in {{site.data.keyword.Bluemix_dedicated_notm}}

In un'istanza [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated), i tile di servizio sono implementati in modo tale che è consentito solo un singolo tile per servizio in un'organizzazione.

Attieniti alla seguente procedura per fare riferimento a un servizio nel tuo spazio di lavoro {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Crea un set di credenziali per il tile di riferimento per il servizio che vuoi utilizzare.
1.  Crea un servizio fornito dall'utente personalizzato che usa queste credenziali:

    1.  Usa l'interfaccia {{site.data.keyword.cloud_notm}} per selezionare il tile per il servizio che vuoi usare e crea un set di credenziali per il servizio. Le credenziali vengono create in formato JSON e contengono dei campi separati per l'URL al quale comunicare con il servizio e per il nome utente e la password da utilizzare con tale servizio.
    1.  Usa il comando `cf cups` (create user-provided service) per creare un servizio personalizzato associato a queste credenziali. Ad esempio, date queste credenziali,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      crea un'istanza del servizio fornita dall'utente personalizzata denominata `Personality-Insights-Std`:

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Assicurati di specificare nome utente e password come una singola stringa all'interno di virgolette doppie e separati da un carattere due punti. Racchiudi inoltre entrambe le stringhe di nome utente e password tra virgolette doppie ed eseguine l'escape con una barra rovesciata.
      {: tip}

### Utilizzo di applicazioni con {{site.data.keyword.Bluemix_dedicated_notm}}

Alcune applicazioni di esempio e alcuni kit starter {{site.data.keyword.watson}} su GitHub includono un pulsante **Distribuisci a {{site.data.keyword.cloud_notm}}**. Questi pulsanti non funzioneranno nelle installazioni {{site.data.keyword.Bluemix_dedicated_notm}} perché fanno riferimento all'ambiente {{site.data.keyword.cloud_notm}} pubblico.

Per utilizzare dei kit starter e delle applicazioni di esempio negli ambienti {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Nelle istruzioni per l'applicazione, assicurati che il file `manifest.yml` da te creato utilizzi il nome del servizio da te creato con il comando `cf cups`.
1.  In queste stesse istruzioni, invece di eseguire il comando `cf create-service`, usa un comando `cf cups`. Tuttavia, se hai creato un'istanza fornita dall'utente personalizzata del tuo servizio, non hai bisogno di crearla nuovamente per utilizzarla con il kit starter o l'applicazione di esempio.
