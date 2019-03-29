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

# Authentifizierung mit Cloud Foundry-Serviceberechtigungsnachweisen
{: #creating-credentials}

Cloud Foundry-Serviceberechtigungsnachweise für {{site.data.keyword.ibmwatson}}-Services stellen die Authentifizierung für einen Service in {{site.data.keyword.cloud}} bereit. Die Serviceberechtigungsnachweise verwenden die HTTP-Basisauthentifizierung für die Authentifizierung bei einem bestimmten {{site.data.keyword.watson}}-Service. {: shortdesc}

{{site.data.keyword.watson}}-Services, die in einer Ressourcengruppe erstellt oder von Cloud Foundry in eine Ressourcengruppe migriert werden, verwenden die IAM-Authentifizierung. Standardmäßig verwenden alle neuen {{site.data.keyword.watson}}-Services die IAM-Authentifizierung. Weitere Informationen zu Ressourcengruppen und zur IAM-Authentifizierung finden Sie in [Authentifizierung mit IAM-Tokens](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually).
{: note}

## Cloud Foundry-Serviceberechtigungsnachweise abrufen
{: #getting-credentials-manually}

Für den Zugriff auf API-Methoden mit Cloud Foundry-Serviceberechtigungsnachweisen müssen die Berechtigungsnachweise zuerst abgerufen werden. Sie können über die {{site.data.keyword.cloud_notm}}-Webschnittstelle auf die Serviceberechtigungsnachweise zugreifen. 

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window} an. 
1.  Rufen Sie die [Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard){: new_window} auf. 
1.  Wählen Sie eine Serviceinstanz aus. 
1.  Klicken Sie auf **Berechtigungsnachweise anzeigen**, um den **Benutzernamen**, das **Kennwort** und die **URL** für die Berechtigungsnachweise anzuzeigen. 

## Cloud Foundry-Serviceberechtigungsnachweise aktualisieren
{: #existing-svcs}

Sie können die Serviceberechtigungsnachweise für eine vorhandene Serviceinstanz über das Service-Dashboard aktualisieren.

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window} an. 
1.  Rufen Sie die [Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard){: new_window} auf. 
1.  Wählen Sie **Serviceberechtigungsnachweise** im Navigationsmenü links auf der Seite aus. 
1.  Verwenden Sie die Menüs und Symbole, um vorhandene Berechtigungsnachweise zu löschen oder neue Berechtigungsnachweise hinzuzufügen. 

Stellen Sie sicher, dass Sie die Berechtigungsnachweise in Ihren Anwendungen bei Änderungen aktualisieren. 

## Berechtigungsnachweise in {{site.data.keyword.Bluemix_dedicated_notm}}

In einer [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated)-Instanz werden Service-Kacheln so implementiert, dass nur eine Kachel pro Service in einer Organisation zulässig ist.

Führen Sie die folgenden Schritte aus, um auf einen Service in Ihrem {{site.data.keyword.Bluemix_dedicated_notm}}-Workspace zu verweisen:

1.  Erstellen Sie Berechtigungsnachweise für die Referenzkachel für den Service, den Sie verwenden möchten.
1.  Erstellen Sie einen angepassten, vom Benutzer bereitgestellten Service, der diese Berechtigungsnachweise verwenden:

    1.  Verwenden Sie die Webschnittstelle, um die Kachel für den Service auszuwählen, den Sie verwenden möchten. Erstellen Sie dann Berechtigungsnachweise für den Service. Die Berechtigungsnachweise werden im JSON-Format erstellt und enthalten die Serviceendpunkt-URL, den Benutzernamen und das Kennwort. 
    1.  Verwenden Sie den Befehl `ibmcloud cf cups` (create user-provided service), um einen benutzerdefinierten Service zu erstellen, der an diese Berechtigungsnachweise gebunden ist. Beispiel:

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Erstellen Sie mit diesen Berechtigungsnachweisen eine angepasste, vom Benutzer bereitgestellte Serviceinstanz mit dem Namen `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Stellen Sie sicher, dass der Benutzername und das Kennwort als einzelne Zeichenfolge in doppelten Anführungszeichen und durch einen Doppelpunkt getrennt angegeben werden. Schließen Sie auch die Zeichenfolgen für den Benutzernamen und das Kennwort in doppelte Anführungszeichen ein, und umgehen Sie sie mit einem umgekehrten Schrägstrich.
      {: tip}

### Apps mit {{site.data.keyword.Bluemix_dedicated_notm}} verwenden

Einige {{site.data.keyword.watson}}-Beispielapps und Starter-Kits für GitHub enthalten die Schaltfläche **Auf {{site.data.keyword.cloud_notm}} bereitstellen**. Diese Schaltflächen funktionieren in {{site.data.keyword.Bluemix_dedicated_notm}}-Installationen nicht, dass sie auf eine öffentliche {{site.data.keyword.cloud_notm}}-Umgebung verweisen.

Führen Sie die folgenden Schritte aus, um Beispielanwendungen und Starter-Kits in {{site.data.keyword.Bluemix_dedicated_notm}}-Umgebungen zu verwenden:

1.  Stellen Sie in den Anweisungen für die Anwendung sicher, dass die Datei `manifest.yml`, die Sie erstellen, den Namen des Service verwendet, den Sie mit dem Befehl `ibmcloud cf cups` erstellt haben. 
1.  Verwenden Sie in denselben Anweisungen anstelle der Ausführung des Befehls `ibmcloud cf create-service` den Befehl `ibmcloud cf cups`. Wenn Sie jedoch eine angepasste, vom Benutzer bereitgestellte Instanz Ihres Services erstellt haben, müssen Sie sie nicht noch einmal erstellen, um sie mit der Beispielanwendung oder dem Starter-Kit zu verwenden.
