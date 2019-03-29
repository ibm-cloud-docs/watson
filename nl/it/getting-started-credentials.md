---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# Autenticazione con le credenziali del servizio Cloud Foundry
{: #creating-credentials}

Le credenziali del servizio Cloud Foundry per il servizio {{site.data.keyword.ibmwatson}} forniscono l'autenticazione a un servizio in {{site.data.keyword.cloud}}. Le credenziali del servizio utilizzano l'autenticazione di base HTTP per eseguire l'autenticazione a un servizio {{site.data.keyword.watson}} specifico.
{: shortdesc}

I servizi {{site.data.keyword.watson}} che vengono creati in un gruppo di risorse o che vengono migrati da Cloud Foundry a un gruppo di risorse utilizzano l'autenticazione IAM. Per impostazione predefinita, tutti i nuovi servizi {{site.data.keyword.watson}} utilizzano l'autenticazione IAM. Per ulteriori informazioni sui gruppi di risorse e sull'autenticazione IAM, vedi [Autenticazione con i token IAM](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually).
{: note}

## Come ottenere le credenziali del servizio Cloud Foundry
{: #getting-credentials-manually}

Per accedere ai metodi API utilizzando le credenziali del servizio Cloud Foundry, devi innanzitutto raccogliere le credenziali. Puoi accedere alle credenziali del servizio dall'interfaccia web {{site.data.keyword.cloud_notm}}.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}.
1.  Vai al tuo [elenco di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard){: new_window}.
1.  Seleziona un'istanza del servizio. 
1.  Fai clic su **Visualizza credenziali** per vedere **Nome utente**, **Password** e **Url** relativi alle credenziali.

## Aggiornamento delle credenziali del servizio Cloud Foundry
{: #existing-svcs}

Puoi aggiornare le credenziali del servizio per un'istanza del servizio esistente dal dashboard del servizio.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}.
1.  Vai al tuo [elenco di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard){: new_window}.
1.  Seleziona **Credenziali del servizio** dal menu di navigazione di sinistra della pagina. 
1.  Utilizza i menu e le icone per eliminare le credenziali esistenti o per aggiungerne di nuove. 

Assicurati di aggiornare le credenziali nelle tue applicazioni per eventuali modifiche. 

## Credenziali in {{site.data.keyword.Bluemix_dedicated_notm}}

In un'istanza [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated), i tile di servizio sono implementati in modo tale che è consentito solo un singolo tile per servizio in un'organizzazione.

Attieniti alla seguente procedura per fare riferimento a un servizio nel tuo spazio di lavoro {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Crea un set di credenziali per il tile di riferimento per il servizio che vuoi utilizzare.
1.  Crea un servizio fornito dall'utente personalizzato che usa queste credenziali:

    1.  Utilizza l'interfaccia web per selezionare il tile per il servizio che vuoi utilizzare e crea una serie di credenziali per il servizio. Le credenziali vengono create in formato JSON e includono l'url, il nome utente e la password dell'endpoint.
    1.  Utilizza il comando `ibmcloud cf cups` (create user-provided service) per creare un servizio personalizzato associato a tali credenziali. Ad esempio,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Date queste credenziali, crea un'istanza del servizio fornita dall'utente personalizzata denominata `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Assicurati di specificare nome utente e password come una singola stringa all'interno di virgolette doppie e separati da un carattere due punti. Inoltre, racchiudi entrambe le stringhe di nome utente e password tra virgolette doppie ed eseguine l'escape con una barra rovesciata.
      {: tip}

### Utilizzo di applicazioni con {{site.data.keyword.Bluemix_dedicated_notm}}

Alcune applicazioni di esempio e alcuni kit starter {{site.data.keyword.watson}} su GitHub includono un pulsante **Distribuisci a {{site.data.keyword.cloud_notm}}**. Questi pulsanti non funzionano nelle installazioni {{site.data.keyword.Bluemix_dedicated_notm}} perché fanno riferimento all'ambiente {{site.data.keyword.cloud_notm}} pubblico. 

Per utilizzare dei kit starter e delle applicazioni di esempio negli ambienti {{site.data.keyword.Bluemix_dedicated_notm}}, segui questi passi: 

1.  Nelle istruzioni per l'applicazione, assicurati che il file `manifest.yml` da te creato utilizzi il nome del servizio da te creato con il comando `ibmcloud cf cups`.
1.  In tali istruzioni, invece di eseguire il comando `ibmcloud cf create-service`, utilizza un comando `ibmcloud cf cups`. Tuttavia, se hai creato un'istanza fornita dall'utente personalizzata del tuo servizio, non hai bisogno di crearla nuovamente per utilizzarla con il kit starter o l'applicazione di esempio.
