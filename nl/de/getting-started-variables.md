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

# Umgebungsvariable VCAP\_SERVICES
{: #vcapServices}

Die Umgebungsvariable `VCAP_SERVICES` enthält Informationen, die Sie für die Interaktion mit einer Serviceinstanz in {{site.data.keyword.cloud}} verwenden können. Die Felder in dieser Umgebungsvariablen werden festgelegt, wenn Sie eine Serviceinstanz an eine Anwendung binden.
{: shortdesc}

Ihre Anwendung benötigt die URL und die Serviceberechtigungsnachweise für die Authentifizierung bei einem {{site.data.keyword.watson}}-Service. Eine aktive Anwendung liest diese Umgebungsvariablen normalerweise, nachdem ein Service an die Anwendung gebunden wurde, um die erforderlichen Name/Wert-Paare zu extrahieren. Sie können entweder Ressourcengruppen- oder Cloud Foundry-Services an Ihre App binden, Ressourcengruppenservices benötigen jedoch einen Servicealias. 

## Servicaliasnamen für IAM-Services
{: #vcapIAMAlias}

Sie können die Umgebungsvariable `VCAP_SERVICES` mit Ressourcengruppenservices verwenden, indem Sie für den Service einen Alias festlegen. Verwenden Sie beispielsweise den folgenden Befehl in der {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle, um den Alias `personality-insights-service` für einen Service mit dem Namen `my-personality-insights-service` zu erstellen. 

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

Anschließend können Sie Ihre App an den Service-Alias binden. Eine aktive Anwendung liest diese Umgebungsvariablen normalerweise, nachdem eine Serviceinstanz an die Anwendung gebunden wurde, um die erforderlichen Name/Wert-Paare zu extrahieren. 

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Beispiel
{: #gs-variables-vcapIAMAliasExample}

Dieses Beispiel zeigt die Umgebungsvariable `VCAP_SERVICES` für den {{site.data.keyword.personalityinsightsshort}}-Service, der an eine Anwendung gebunden ist. Die Serviceinstanz verwendet die Cloud Foundry-Authentifizierung. 

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

Die folgende Tabelle beschreibt den Inhalt der Umgebungsvariable `VCAP_SERVICES`.

| Element     | Beschreibung                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | Das Kennwort aus den Berechtigungsnachweisen für die HTTP-Basisauthentifizierung für die Verbindung mit dem Service |
| url      | Die Verbindungs-URL für den Service                                                         |
| username | Der Benutzername aus den Berechtigungsnachweisen für die HTTP-Basisauthentifizierung für die Verbindung mit dem Service |
| label    | Der Name, der zum Service in {{site.data.keyword.cloud_notm}} gehört                                            |
| name     | Der Name der Serviceinstanz                                                           |
| plan     | Der Plan, der für den Service in {{site.data.keyword.cloud_notm}} verfügbar ist                                              |
| tags     | Weitere Informationen zum Service                                                   |

## Umgebungsvariablen anzeigen
{: #gs-variables-vcapView}

Informationen zur Variablen können Sie entweder über die {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle (CLI) oder über die Webschnittstelle abrufen. 

- Über die {{site.data.keyword.cloud_notm}}-Befehlszeilenschnittstelle: Den Cloud Foundry-Befehl `env` aufrufen, nachdem Sie einen Service erstellt und an Ihre Anwendung gebunden haben. 

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- Über die {{site.data.keyword.cloud_notm}}-Schnittstelle: Informationen zu Umgebungsvariablen sind auf der Übersichtsseite für eine Anwendung verfügbar.
