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

# Befehlszeilenschnittstelle von Cloud Foundry

Sie können mit dem Befehlszeilenschnittstellentool `cf` (CLI) mit den {{site.data.keyword.cloud}}-Anwendungen auf Ihrem lokalen System arbeiten. Das Tool bietet eine umfassende Oberfläche für ein umfassendes Entwicklungs-Know-how.
{: shortdesc}

Weitere Informationen zum Arbeiten mit Cloud Foundry-Tools zur Anwendungsentwicklung finden Sie in [Arbeiten mit Cloud Foundry mit {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) und in der [Cloud Foundry-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://docs.cloudfoundry.org/){: new_window}.

## Die Befehlszeilenschnittstelle installieren

Die aktuelle Version des Tools finden Sie im Abschnitt **Downloads** im [Cloud Foundry GitHub-Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Es werden zwei Typen von Paketen angezeigt:

- **Installationsprogramme** können mit dem Paketmanagement-Tool oder dem installierten Software-Überwachungssystem der Plattform integriert werden, um das Tool an seinem "offiziellen" Standort zu installieren.
- **Binärdateien** enthalten Archivdateien, die Sie an jeder beliebigen Position installieren können.

Stellen Sie sicher, dass der Befehl nach der Installation des Tools in die Umgebungsvariable `PATH` eingefügt wird. {: tip}

## Befehle für Serviceinstanzen

Nützliche `cf`-Befehle zum Verwalten von vorhandenen {{site.data.keyword.watson}}-Serviceinstanzen in {{site.data.keyword.cloud_notm}}:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services) Listen mit Serviceinstanzen im aktuellen Bereich.

  ```bash
  cf services
  ```
  {: pre}

  Wenn Sie den Namen des Services kennen, verwenden Sie den Befehl `cf service`, um weitere Informationen dazu anzusehen. Beispiel: 

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): Erstellen Sie eine Instanz des Services, den Sie an Ihre Anwendung binden können, die Sie in {{site.data.keyword.cloud_notm}} ausführen:

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - Verwenden Sie für die Argumente *{service-name}* und *{service-plan}* die Ausgabe des Befehls `cf marketplace`.
    - Geben Sie den {service-instance-name} an, den Sie erstellen möchten. Für viele Anwendungen wird der Name in `manifest.yml` oder in einer anderen Konfigurationsdatei für die Anwendung angegeben.

- **cf delete-service**: Löschen Sie eine Serviceinstanz:

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    Wenn Sie einen Service löschen, den Sie nicht mehr benötigen, geben Sie Ressourcen in {{site.data.keyword.cloud_notm}} frei.

## Befehle zum Bereitstellen und Ausführen von Anwendungen

Allgemeine `cf`-Befehle zum Bereitstellen und Ausführen von Anwendungen in {{site.data.keyword.cloud_notm}}:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): Geben Sie die Position der {{site.data.keyword.watson}}-APIs an:

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login): Melden Sie sich bei {{site.data.keyword.cloud_notm}} mit {{site.data.keyword.ibmid}} und Ihrem Kennwort an:

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): Listen Sie die Services im {{site.data.keyword.cloud_notm}}-Katalog auf, mit dem Sie eine Verbindung herstellen können:

  ```bash
  cf marketplace
  ```
  {: pre}

  Die Ausgabe enthält den Namen jedes Services und die Liste der Pläne, unter denen der Service verfügbar ist. Sie benötigen diese Informationen, wenn Sie eine Instanz des Services für Ihre Anwendung erstellen, die in {{site.data.keyword.cloud_notm}} ausgeführt wird. 

  Wenn Sie den Namen des Services kennen, verwenden Sie die Option `-s`. Beispiel: 

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): Stellen Sie eine Anwendung auf {{site.data.keyword.cloud_notm}} bereit oder aktualisieren Sie sie. Die Ausgabe enthält die URL für den Zugriff auf Ihre Anwendung.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  Der Befehl findet oft die Datei `manifest.yml` oder eine andere Konfigurationsdatei, um die Datei zu finden, die mit einer Push-Operation übertragen werden soll (zum Beispiel die Datei `.war` für Java oder eine `.zip`-Datei für Node.js). Sie können jedoch den Pfadnamen eine Anwendungsdatei angeben, indem Sie die Option `-p` verwenden.

## Befehle für vorhandene Anwendungen

Nützliche `cf`-Befehle zum Verwalten von vorhandenen Anwendungen in {{site.data.keyword.cloud_notm}}:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps): Listen Sie die Anwendungen auf, die im aktuellen Bereich bereitgestellt werden.

  ```bash
  cf apps
  ```
  {: pre}

  Fügen Sie den Anwendungsnamen hinzu, um detaillierte Informationen zum Status und Zustand einer Anwendung anzuzeigen:

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: Laden Sie Ihre Anwendung neu und starten Sie sie in {{site.data.keyword.cloud_notm}} neu:

  ```bash
  cf restage {application-name}
  ```
  {: pre}

Dieser Befehl startet die Anwendung neu, so wie sie in {{site.data.keyword.cloud_notm}} existiert. Um die aktuelle Version der Anwendung auf {{site.data.keyword.cloud_notm}} zu aktualisieren, bevor sie einen Neustart durchführen, verwenden Sie stattdessen den Befehl `cf push`.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete): Löschen Sie eine Anwendung:

  ```bash
  cf delete application-name
  ```
  {: pre}

  Indem Sie eine Anwendung löschen, geben Sie {{site.data.keyword.cloud_notm}}-Speicherplatz und -Kontingent für andere Anwendungen frei.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service) Bindet einen Service an eine Anwendung.

  In der Regel verwenden Sie den Befehl `cf push`, um eine Instanz des Services an eine Anwendung zu binden, die ihn verwendet. Sie können eine vorhandene Serviceinstanz an eine vorhandene Anwendung mithilfe des Befehls `cf bind-service` binden:

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Geben Sie `cf restage` oder `cf push` aus, um die Anwendung neu zu starten.

- **cf unbind-service**: Entfernt die Zuordnung zwischen Anwendung und Service:

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Sie müssen möglicherweise den Befehl `cf restage` oder `cf push` ausgeben, damit die Änderung wirksam wird.

  Mit diesem Befehl werden Ihre Authentifizierungsnachweise aus der Umgebungsvariablen `VCAP_SERVICES` entfernt. Verwenden Sie `cf delete-service` oder `cf delete`, um die Serviceinstanz oder die Anwendung zu entfernen.
  {: tip}
