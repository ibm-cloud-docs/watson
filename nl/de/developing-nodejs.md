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

# Eine {{site.data.keyword.watson}}-Anwendung in Node.js entwickeln

Um die Verwendung der REST-API darzustellen, stellt jeder {{site.data.keyword.ibmwatson}}-Service webbasierte Beispielanwendungen in Node.js im {{site.data.keyword.watson}}-GitHub-Projekt bereit.
{: shortdesc}

## Vorbereitungen

- Erstellen Sie ein Konto in {{site.data.keyword.cloud_notm}} oder nutzen Sie ein vorhandenes Konto. [Sich für eine kostenlose Testversion anmelden ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. Auf Ihrem Konto muss Speicherplatz für mindestens 1 App und 1 Service sein.
- Die [Node.js-Laufzeit ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://nodejs.org/#download){: new_window}, einschließlich [npm](https://www.npmjs.com/){: new_window}-Paketmanager. Stellen Sie sicher, dass der Befehl nach der Installation in die Umgebungsvariable `PATH` eingefügt wird.
- Der [Cloud Foundry ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/cli#downloads){: new_window} Befehlszeilenclient. Wenn Sie es vorab installiert haben, stellen Sie sicher, dass Ihre Version die aktuelle ist.

## Den Quellcode abrufen

Besuchen Sie das [{{site.data.keyword.watson}} Github-Projekt ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud){: new_window}, um die Node.js-Beispiel-Apps anzuzeigen, die für {{site.data.keyword.watson}} verfügbar sind. Verwenden Sie dann GitHub, um ein Repository lokal zu klonen.

## Lokal installieren
Wenn Sie die App ändern oder als Basis für die Erstellung einer eigenen App verwenden möchten, installieren Sie sie lokal. Sie können dann Ihre geänderte Version der App auf {{site.data.keyword.cloud_notm}} bereitstellen.

### Einen {{site.data.keyword.watson}}-Service einrichten

1.  Wechseln Sie in der Befehlszeile zum lokalen Projektverzeichnis der geklonten App.
1.  Melden Sie sich bei {{site.data.keyword.cloud_notm}} an:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Suchen Sie den Namen des Services und Plans. Wenn die Datei `manifest.yml` der Beispiel-App den Abschnitt `declared-services` enthält, beachten Sie `label` und `plan`. Andernfalls verwenden Sie den Befehl `cf marketplace`, um den Namen des Service und die Pläne zu erhalten:

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Erstellen Sie eine Instanz des Services in {{site.data.keyword.cloud_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. Verwenden Sie für service-name und service-plan die Informationen aus der Manifestdatei oder dem Befehl `cf marketplace`.

    Geben Sie beispielsweise den folgenden Befehl aus, um die Serviceinstanz für den {{site.data.keyword.personalityinsightsshort}}-Service zu erstellen:

    ```bash
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Die App-Umgebung konfigurieren

1.  Kopieren Sie die Datei `.env.example` in die neue Datei `.env`.
1.  Erstellen Sie einen Serviceschlüssel im Format `cf create-service-key {service_instance} {service_key}`. Beispiel:

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Rufen Sie die Berechtigungsnachweise aus dem Serviceschlüssel mit dem Befehl `cf service-key {service_instance} {service_key}` ab. Beispiel:

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    Die Ausgabe dieses Befehls ist ein JSON-Objekt, wie in diesem Beispiel:

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }
    ```
    {: codeblock}

1.  Fügen Sie die Werte `password` und `username` (ohne Anführungszeichen) aus JSON in die entsprechenden Variablen in die Datei `.env`. Beispiel:

    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### Die App installieren und starten

1.  Installieren Sie das Demo-App-Paket in der lokalen Node.js-Laufzeitumgebung:

    ```bash
    npm install
    ```
    {: pre}

1.  Starten Sie die App:

    ```bash
    npm start
    ```
    {: pre}

1.  Rufen Sie in Ihrem Browser http://localhost:3000 auf und testen Sie die App. Der Port wird in der Datei `app.js` angegeben.

## Auf {{site.data.keyword.cloud_notm}} bereitstellen

Sie können Cloud Foundry verwenden, um Ihre lokale Version der App auf {{site.data.keyword.cloud_notm}} bereitzustellen.

1.  Öffnen Sie im Projektstammverzeichnis die Datei manifest.yml:
    1.  Ändern Sie im Abschnitt `applications` der Datei `manifest.yml` den Namen in einen eindeutigen Namen für Ihre Version der App.
    1.  Geben Sie im Abschnitt `services` den Namen der {{site.data.keyword.watson}}-Serviceinstanz an, die Sie für Ihre App erstellt haben. Wenn Sie sich nicht mehr an den Servicenamen erinnern können, verwenden Sie den Befehl `cf services`, um eine Liste mit Services anzuzeigen.

        Das folgende Beispiel zeigt eine geänderte Datei `manifest.yml` an:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Melden Sie sich bei {{site.data.keyword.cloud_notm}} an:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Übertragen Sie die App mit einer Push-Operation auf {{site.data.keyword.cloud_notm}} und geben Sie den Namen Ihrer App aus der Datei `manifest.yml` an. Beispiel:

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Rufen Sie in Ihrem Browser die in der Befehlsausgabe angegebene URL auf.
