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

# Beispiele aus dem Node.js-SDK ausführen

Das {{site.data.keyword.watson}} Node.js-SDK (Software Development Kit) enthält Beispielcode für jeden Service.
{: shortdesc}

## Vorbereitungen

1.  Erstellen Sie ein {{site.data.keyword.Bluemix}}-Konto oder nutzen Sie ein vorhandenes Konto. [Sich für eine kostenlose Testversion anmelden ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. 
1.  Rufen Sie die Serviceberechtigungsnachweise für den {{site.data.keyword.watson}}-Service ab, den Sie ausführen möchten. Weitere Details finden Sie in [Serviceberechtigungsnachweis für {{site.data.keyword.watson}}-Services](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Stellen Sie sicher, dass die [Node.js-Laufzeit ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://nodejs.org/#download){: new_window} installiert ist, einschließlich [npm-Paketmanager ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.npmjs.com/){: new_window}. Stellen Sie sicher, dass der Befehl nach der Installation in die Umgebungsvariable `PATH` eingefügt wird. 

## Das Beispiel aktualisieren und ausführen

1.  Speichern Sie eine Kopie des Beispielcodes für den {{site.data.keyword.watson}}-Service, den Sie ausführen möchten.
    1.  Navigieren Sie zum Verzeichnis `/examples` im watson-developer-cloud [Node-SDK ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window}.
    1.  Speichern Sie den Code eines Beispiels lokal. Wir verwenden hier `personality_insights.v3.js`.
1.  Wechseln Sie in das Verzeichnis, in dem Sie den Code gespeichert haben, und installieren Sie das Node.js-SDK:

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Aktualisieren Sie den Beispielcode, um Ihre Serviceberechtigungsnachweise (API-Schlüssel oder Benutzername und Kennwort) zu verwenden:

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'IHREN BENUTZERNAMEN FÜR DEN SERVICE HIER EINGEBEN',
      password: 'IHR KENNWORT FÜR DEN SERVICE HIER EINGEBEN',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    Sie müssen möglicherweise weitere Änderungen vornehmen. Beispiel: Sie fügen hier ausreichend Text für {{site.data.keyword.personalityinsightsshort}} zur Analyse hinzu.

1.  Geben Sie den Befehl `node {example_application.vn}.js` aus, um den Code auszuführen. Beispiel:

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    Die Antwort enthält Informationen aus dem Service. Beispiel: Für den {{site.data.keyword.personalityinsightsshort}}-Service wird eine ähnliche Ausgabe wie in diesem verkürzten Beispiel angezeigt:

    ```json
    {
      "word_count": 224,
      "word_count_message": "There were 224 words in the input. We need a minimum of 600, preferably 1,200 or more, to compute statistically significant estimates",
      "processed_lang": "en",
      "personality": [
      {
        "trait_id": "big5_openness",
        "name": "Openness",
        "category": "personality",
        "percentile": 0.9015311332932557,
        "children": [. . .]
      },
      . . .
      ],
      "needs": [. . .],
      "values": [. . .],
      "consumption_preferences": [. . .]
    }
    ```
    {: codeblock}
