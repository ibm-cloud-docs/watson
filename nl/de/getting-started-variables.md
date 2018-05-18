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

# Umgebungsvariable VCAP\_SERVICES
{: #vcapServices}

Die Umgebungsvariable `VCAP_SERVICES` enthält Informationen, die Sie für die Interaktion mit einer Serviceinstanz in {{site.data.keyword.Bluemix}} verwenden können. Die Felder in dieser Umgebungsvariablen werden festgelegt, wenn Sie einen Service an eine Anwendung binden.
{: shortdesc}

Ihre Anwendung benötigt die Serviceberechtigungsnachweise für die URL und HTTP-Basisauthentifizierung, um sich bei einem {{site.data.keyword.watson}}-Service zu authentifizieren.

Eine aktive Anwendung liest in der Regel diese Umgebungsvariablen, nachdem ein Service an sie gebunden wird, um die erforderlichen Name-Wert-Paare zu extrahieren. Weitere Informationen zu den Variablen finden Sie unter [Apps bereitstellen](/docs/manageapps/depapps.html#app_env), wenn Sie nach "Umgebungsvariablen" suchen.

## Beispiel
Dieses Beispiel zeigt die Umgebungsvariable `VCAP_SERVICES` für den {{site.data.keyword.personalityinsightsshort}}-Service, der an eine Anwendung gebunden ist:

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

| Element  | Beschreibung                                                                               |
|----------|--------------------------------------------------------------------------------------------|
| password | Das Kennwort aus den Berechtigungsnachweisen für die HTTP-Basisauthentifizierung für die Verbindung mit dem Service |
| url      | Die Verbindungs-URL für den Service                                                         |
| username | Der Benutzername aus den Berechtigungsnachweisen für die HTTP-Basisauthentifizierung für die Verbindung mit dem Service |
| label    | Der Name, der zum Service in {{site.data.keyword.cloud_notm}} gehört                                           |
| name     | Der Name der Serviceinstanz                                                           |
| plan     | Der Plan, der für den Service in {{site.data.keyword.cloud_notm}} verfügbar ist                                             |
| tags     | Weitere Informationen zum Service                                                   |

## Umgebungsvariablen anzeigen
Sie können Informationen über die Variable entweder vom Cloud Foundry-Tool oder von der {{site.data.keyword.cloud_notm}}-Benutzeroberfläche abrufen. 

- Mit dem Cloud Foundry-Befehlszeilentool: Verwenden Sie den Befehl `cf env`, nachdem Sie den Service erstellt und ihn an Ihre Anwendung gebunden haben:

    ```bash
    cf env {application-name}
    ```
    {: pre}

- Mit der {{site.data.keyword.cloud_notm}}-Schnittstelle: Informationen zu Umgebungsvariablen sind auf der Übersichtsseite für eine Anwendung verfügbar.
