---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: environment variable,service alias,VCAP services

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

# Variabile di ambiente VCAP\_SERVICES
{: #vcapServices}

La variabile di ambiente `VCAP_SERVICES` contiene informazioni che puoi usare per interagire con un'istanza del servizio in {{site.data.keyword.cloud}}. I campi in questa variabile di ambiente sono impostati quando associ un'istanza del servizio a un'applicazione.
{: shortdesc}

La tua applicazione ha bisogno dell'URL e delle credenziali del servizio per eseguire l'autenticazione con un servizio {{site.data.keyword.watson}}. Un'applicazione in esecuzione di norma legge queste variabili di ambiente, dopo che a essa viene associato un servizio, per estrarre le coppie nome-valore necessarie. Puoi associare un gruppo di risorse o i servizi Cloud Foundry alla tua applicazione, ma i servizi del gruppo di risorse necessitano di un alias del servizio.

## Alias del servizio per i servizi IAM
{: #vcapIAMAlias}

Puoi utilizzare la variabile di ambiente `VCAP_SERVICES` con i servizi del gruppo di risorse fornendo un alias al servizio. Ad esempio, utilizza il seguente comando con la CLI {{site.data.keyword.cloud_notm}} per creare un alias denominato `personality-insights-service` per un servizio denominato `my-personality-insights-service`.

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

Puoi associare la tua applicazione all'alias del servizio. Un'applicazione in esecuzione di norma legge queste variabili di ambiente, dopo che a essa viene associata un'istanza del servizio, per estrarre le coppie nome-valore necessarie. 

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Esempio
{: #gs-variables-vcapIAMAliasExample}

Questo esempio mostra la variabile di ambiente `VCAP_SERVICES` per il servizio {{site.data.keyword.personalityinsightsshort}} associato a un'applicazione. L'istanza del servizio utilizza l'autenticazione Cloud Foundry.

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

| Elemento     | Descrizione                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | La password dalle credenziali di autenticazione di base HTTP utilizzate per stabilire una connessione al servizio |
| url      | L'URL di connessione per il servizio                                                         |
| username | Il nome utente dalle credenziali di autenticazione di base HTTP utilizzate per stabilire una connessione al servizio |
| label    | Il nome associato al servizio in {{site.data.keyword.cloud_notm}}                                            |
| name     | Il nome dell'istanza del servizio                                                           |
| plan     | Il piano disponibile per il servizio in {{site.data.keyword.cloud_notm}}                                              |
| tags     | Informazioni aggiuntive sul servizio                                                   |

## Visualizzazione delle variabili di ambiente
{: #gs-variables-vcapView}

Puoi richiamare le informazioni sulla variabile dalla CLI (command-line interface) {{site.data.keyword.cloud_notm}} o dall'interfaccia web.

- Con la CLI {{site.data.keyword.cloud_notm}}: richiama il comando `env` di Cloud Foundry dopo che hai creato un servizio e lo hai associato alla tua applicazione:

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- Dall'interfaccia {{site.data.keyword.cloud_notm}}: le informazioni sulle variabili di ambiente sono disponibili dalla pagina di riepilogo per un'applicazione.
