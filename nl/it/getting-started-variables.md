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

# Variabile di ambiente VCAP\_SERVICES
{: #vcapServices}

La variabile di ambiente `VCAP_SERVICES` contiene informazioni che puoi usare per interagire con un'istanza del servizio in {{site.data.keyword.Bluemix}}. I campi in questa variabile di ambiente sono impostati quando associ un servizio a un'applicazione.
{: shortdesc}

La tua applicazione ha bisogno dell'URL e delle credenziali del servizio di autenticazione di base HTTP per eseguire l'autenticazione presso un servizio {{site.data.keyword.watson}}.

Un'applicazione in esecuzione di norma legge queste variabili di ambiente, dopo che a essa viene associato un servizio, per estrarre le coppie nome-valore necessarie. Per informazioni sulle variabili, consulta [Distribuzione delle applicazioni](/docs/manageapps/depapps.html#app_env) e cerca "Variabili di ambiente".

## Esempio
Questo esempio mostra la variabile di ambiente `VCAP_SERVICES` per il servizio {{site.data.keyword.personalityinsightsshort}}  associato a un'applicazione:

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

La seguente tabella descrive il contenuto della variabile di ambiente `VCAP_SERVICES`.

| Elemento | Descrizione                                                                                                         |
|----------|--------------------------------------------------------------------------------------------|
| password | La password dalle credenziali di autenticazione di base HTTP utilizzate per stabilire una connessione al servizio   |
| url      | L'URL di connessione per il servizio                                                                                |
| username | Il nome utente dalle credenziali di autenticazione di base HTTP utilizzate per stabilire una connessione al servizio|
| label    | Il nome associato al servizio in {{site.data.keyword.cloud_notm}}                                           |
| name     | Il nome dell'istanza del servizio                                                                                   |
| plan     | Il piano disponibile per il servizio in {{site.data.keyword.cloud_notm}}                                    |
| tags     | Informazioni aggiuntive sul servizio                                                                                |

## Visualizzazione delle variabili di ambiente
Puoi richiamare le informazioni sulla variabile dallo strumento Cloud Foundry o dall'interno dell'interfaccia {{site.data.keyword.cloud_notm}}.

- Con lo strumento di riga di comando Cloud Foundry: usa il comando `cf env` dopo che hai creato un servizio e lo hai associato alla tua applicazione:

    ```bash
    cf env {application-name}
    ```
    {: pre}

- Dall'interfaccia {{site.data.keyword.cloud_notm}}: le informazioni sulle variabili di ambiente sono disponibili dalla pagina di riepilogo per un'applicazione.
