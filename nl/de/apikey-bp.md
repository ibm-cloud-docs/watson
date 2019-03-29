---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM authentication,service keys,service roles,best practices

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

# API-Schlüssel des IAM-Service
{: #api-key-bp}

API-Schlüssel des {{site.data.keyword.ibmwatson}}-Service werden dazu verwendet, Trägertoken abzurufen, die für die Authentifizierung von Aufrufen an {{site.data.keyword.ibmwatson_notm}}-Services verwendet werden.
{: shortdesc}

Im Gegensatz zur IAM-Standardimplementierung ({{site.data.keyword.cloud_notm}} Identity and Access Management) von API-Schlüsseln werden {{site.data.keyword.ibmwatson_notm}}-IAM-Schlüssel pro Service festgelegt. Berechtigungen werden den einzelnen API-Schlüsseln, nicht den einzelnen Services zugewiesen. Diese Implementierung unterscheidet sich von der {{site.data.keyword.cloud_notm}}-Standardmethode benutzerbasierter API-Schlüssel. Mit {{site.data.keyword.ibmwatson_notm}} wird Benutzern eine Rolle für die einzelnen Services zugewiesen. Weitere Informationen zur allgemeinen Implementierung von {{site.data.keyword.cloud_notm}} IAM finden Sie in [Bewährte Verfahren für API-Schlüssel](/docs/services/iam?topic=iam-iamoverview#iamoverview). 

Sie können einen Watson-API-Schlüssel nur an eine einzelne Serviceinstanz binden.
{: tip}

Bei jeder Erstellung einer {{site.data.keyword.ibmwatson_notm}}-Serviceinstanz wird eine Reihe von Berechtigungsnachweisen für die `Manager`-Rolle automatisch generiert. Diese Berechtigungsnachweise stehen Ihnen auf der Seite **Dashboard** der Serviceinstanz zur Verfügung. Sie können sie verwenden, um *alle* Methoden der API des Service aufzurufen. Sie können die möglichen Aktionen, die bei der Verwendung des Service ausgeführt werden, einschränken, indem Sie einen API-Schlüssel mit einer anderen Rolle erstellen. 

| Servicezugriffsrolle | Beschreibung der Aktionen |
|:-----------------|:-----------------|
| `Leseberechtigter` | Leseberechtigte können Nur-Lese-Aktionen innerhalb eines Service ausführen, z. B. das Anzeigen servicespezifischer Ressource. |
| `Schreibberechtigter` | Schreibberechtigte verfügen über Berechtigungen, die über die der Rolle des Leseberechtigten hinausgehen, wie z. B. die Berechtigung zum Erstellen und Bearbeiten servicespezifischer Ressourcen. |
| `Manager` | Manager verfügen über Berechtigungen, die über die Rolle des Schreibberechtigten hinausgehen und mit denen sie durch den Service definierte privilegierte Aktionen durchführen können. Darüber hinaus können sie servicespezifische Ressourcen erstellen und bearbeiten. |
{: caption="Tabelle 1. Beispiele für Servicezugriffsrollen für Benutzer" caption-side="top"}

Weitere Informationen zur Erstellung zusätzlicher {{site.data.keyword.ibmwatson_notm}}-Serviceberechtigungsnachweise finden Sie in [IAM-Serviceberechtigungsnachweise aktualisieren](/docs/services/watson?topic=watson-iam#update-existing-svcs). 

## Bewährte Verfahren für API-Schlüssel
{: #api-bp}

Als Authentifizierungsmethode für Ihren Service müssen Ihre API-Schlüssel sicher aufbewahrt werden. Die nachfolgend beschriebenen bewährten Verfahren unterstützen Sie dabei, die Sicherheit eines API-Schlüssels zu gewährleisten und die Gefahr zu reduzieren, dass Berechtigungsnachweise öffentlich zugänglich gemacht werden und damit die Anwendung beeinträchtigt wird. 

-   *Verwenden Sie einen API-Schlüssel mit der Rolle, die für die verwendete Funktion geeignet ist. *

    Wenn Sie eine Endbenutzeranwendung erstellen, sollten Sie einen API-Schlüssel mit der Rolle eines `Leseberechtigten` für alle Aufrufe an `GET`-API-Methoden verwenden. Mit der Rolle eines `Leseberechtigten` können keine Funktionen ausgeführt werden, mit denen Lösch- oder Hinzufügeaktionen für den Service vorgenommen werden könnten. 

-   *Integrieren Sie den API-Schlüssel nicht direkt in den Code. *

    API-Schlüssel, die in Code integriert sind, könnten für Benutzer zugänglich gemacht werden. Anstatt API-Schlüssel in Anwendungscode zu integrierten, sollten Sie sie entweder in Umgebungsvariablen speichern oder in Dateien, die sich außerhalb Ihres Quellcodesteuersystems (Source Code Control System) befinden. 

-   *Speichern Sie einen API-Schlüssel nicht in Dateien innerhalb des Quellcodesteuersystems (Source Code Control System) Ihrer Anwendung. *

    Wenn Sie API-Schlüssel in Dateien speichern, stellen Sie sicher, dass sich die Dateien außerhalb des Quellcodes Ihrer Anwendung befinden. Diese Vorgehensweise ist vor allem dann wichtig, wenn Sie ein öffentliches Quellcodeverwaltungssystem wie GitHub verwenden. 

-   *Generieren Sie die API-Schlüssel regelmäßig neu. *

    Sie können zu jedem beliebigen Zeitpunkt neue Berechtigungsnachweise erstellen. 
    1.  Wählen Sie in der Ressourcenliste eine Serviceinstanz aus. 
    1.  Klicken Sie auf **Serviceberechtigungsnachweise** im Navigationsmenü links auf der Seite. 

     Wenn Sie die Anwendung auf die neuen Berechtigungsnachweise migrieren, stellen Sie sicher, dass die alten Berechtigungsnachweise mithilfe des Papierkorbsymbols gelöscht werden.
     {: tip}
