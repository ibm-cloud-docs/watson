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

# Serviceberechtigungsnachweise für Watson-Services

Serviceberechtigungsnachweise für einen {{site.data.keyword.ibmwatson}}-Service bieten eine Authentifizierung für einen Service in {{site.data.keyword.cloud}}. Berechtigungsnachweise können nur mit dem Service verwendet werden, für den sie erstellt wurden.
{: shortdesc}

Sie verwenden die Serviceberechtigungsnachweise für den Service abhängig vom verwendeten Programmiermodell. Weitere Details finden Sie in [Programmiermodelle für {{site.data.keyword.watson}}-Services](/docs/services/watson/getting-started-develop.html).

Serviceberechtigungsnachweise sind Berechtigungsnachweise für die HTTP-Basisauthentifizierung für einen bestimmten {{site.data.keyword.watson}}-Service.

## Berechtigungsnachweise manuell abrufen
{: #getting-credentials-manually}

Sie finden die Serviceberechtigungsnachweise in der {{site.data.keyword.cloud_notm}}-Webschnittstelle.

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window} an.
1.  Gehen Sie zu den vorhandenen Services. ([Aufrufen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Wählen Sie das Menü links oben aus und wählen Sie dann **{{site.data.keyword.watson}}** aus, um die {{site.data.keyword.watson}}-Konsole aufzurufen.
    1.  Wählen Sie **Bestehende Services** aus **Watson-Services** aus, um eine Liste Ihrer Services und Projekte aufzulisten.
1.  Zeigen Sie die Berechtigungsnachweise an:
    - Wenn der Service Teil eines Projekts ist, klicken Sie auf den Namen des Projekts, das den Service enthält. Zeigen Sie die Berechtigungsnachweise im Abschnitt **Berechtigungsnachweise** auf der Seite mit den Projektdetails an.
    - Wenn der Service kein Teil eines Projekts ist, klicken Sie auf den Servicenamen, den sie anzeigen möchten, und wählen Sie dann **Serviceberechtigungsnachweise** aus.

## Serviceberechtigungsnachweise aktualisieren
{: #existing-svcs}

Sie können die Serviceberechtigungsnachweise für eine vorhandene Serviceinstanz über das Service-Dashboard aktualisieren.

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window} an.
1.  Gehen Sie zu den vorhandenen Services. ([Aufrufen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Wählen Sie das Menü links oben aus und wählen Sie dann **{{site.data.keyword.watson}}** aus, um die {{site.data.keyword.watson}}-Konsole aufzurufen.
    1.  Wählen Sie **Bestehende Services** aus **Watson-Services** aus, um eine Liste Ihrer Services aufzulisten.
1.  Wählen Sie den Service aus, den Sie anzeigen oder aktualisieren möchten, und wählen Sie dann **Serviceberechtigungsnachweise** aus.
1.  Berechtigungsnachweise löschen oder hinzufügen:
    - Um Berechtigungsnachweise zu löschen, wählen Sie die Berechtigungsnachweise und dann **Berechtigungsnachweis löschen** aus.
    - Um Berechtigungsnachweise hinzuzufügen, klicken Sie auf **Neuer Berechtigungsnachweis** und dann im Fenster "Neuen Berechtigungsnachweis hinzufügen" auf **Hinzufügen**. Zeigen Sie die Berechtigungsnachweise an und kopieren Sie sie.

Stellen Sie sicher, dass Sie die neuen Berechtigungsnachweise in Ihren Anwendungen verwenden:

- Wenn Ihre Anwendung programmgesteuert Berechtigungsnachweise erhält, beenden Sie die Anwendung und starten Sie sie neu.
- Wenn Ihre Berechtigungsnachweise in Ihren Anwendungscode eingebettet sind, aktualisieren Sie den Code und starten Sie die Anwendung neu.

Weitere Details zum Aktualisieren eine Anwendung mit neuen Berechtigungsnachweisen, ohne dass große Ausfallzeiten auftreten, finden Sie in [Apps aktualisieren](/docs/manageapps/updapps.html).

## Berechtigungsnachweise in {{site.data.keyword.Bluemix_dedicated_notm}}

In einer [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated)-Instanz werden Service-Kacheln so implementiert, dass nur eine Kachel pro Service in einer Organisation zulässig ist.

Führen Sie die folgenden Schritte aus, um auf einen Service in Ihrem {{site.data.keyword.Bluemix_dedicated_notm}}-Workspace zu verweisen:

1.  Erstellen Sie Berechtigungsnachweise für die Referenzkachel für den Service, den Sie verwenden möchten. 
1.  Erstellen Sie einen angepassten, vom Benutzer bereitgestellten Service, der diese Berechtigungsnachweise verwenden:

    1.  Verwenden Sie die {{site.data.keyword.cloud_notm}}-Schnittstelle, um die Kachel für den Service auszuwählen, den Sie verwenden möchten. Erstellen Sie dann Berechtigungsnachweise für den Service. Die Berechtigungsnachweise werden im JSON-Format erstellt und enthalten separate Felder für die URL, mit der mit dem Service kommuniziert werden kann, sowie für den Benutzernamen und das Kennwort, die mit diesem Service verwendet werden sollen.
    1.  Verwenden Sie den Befehl `cf cups` (create user-provided service), um einen benutzerdefinierten Service zu erstellen, der an diese Berechtigungsnachweise gebunden ist. Beispiel für diese Berechtigungsnachweise: 

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Erstellen Sie eine angepasste, vom Benutzer bereitgestellte Serviceinstanz mit dem Namen `Personality-Insights-Std`:

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Stellen Sie sicher, dass der Benutzername und das Kennwort als einzelne Zeichenfolge in doppelten Anführungszeichen und durch einen Doppelpunkt getrennt angegeben werden. Schließen Sie auch die Zeichenfolgen für den Benutzernamen und das Kennwort in doppelte Anführungszeichen ein, und umgehen Sie sie mit einem umgekehrten Schrägstrich.
      {: tip}

### Apps mit {{site.data.keyword.Bluemix_dedicated_notm}} verwenden

Einige {{site.data.keyword.watson}}-Beispielapps und Starter-Kits für GitHub enthalten die Schaltfläche **Auf {{site.data.keyword.cloud_notm}} bereitstellen**. Diese Schaltflächen funktionieren in {{site.data.keyword.Bluemix_dedicated_notm}}-Installationen nicht, dass sie auf eine öffentliche {{site.data.keyword.cloud_notm}}-Umgebung verweisen.

Gehen Sie wie folgt vor, um Beispielanwendungen und Starter-Kits in {{site.data.keyword.Bluemix_dedicated_notm}}-Umgebungen zu verwenden:

1.  Stellen Sie in den Anweisungen für die Anwendung sicher, dass die Datei `manifest.yml`, die Sie erstellen, den Namen des Services verwendet, den Sie mit dem Befehl `cf cups` erstellt haben.
1.  Verwenden Sie in einigen Anweisungen den Befehl `cf cups` anstatt den Befehl `cf create-service` auszuführen. Wenn Sie jedoch eine angepasste, vom Benutzer bereitgestellte Instanz Ihres Services erstellt haben, müssen Sie sie nicht noch einmal erstellen, um sie mit der Beispielanwendung oder dem Starter-Kit zu verwenden.
