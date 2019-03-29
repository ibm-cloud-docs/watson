---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM tokens,IAM authentication

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

# Authentifizierung mit IAM-Tokens
{: #iam}

Mit {{site.data.keyword.cloud}} Identity and Access Management-Tokens (IAM-Tokens) können Sie authentifizierte Anforderungen an {{site.data.keyword.ibmwatson}}-Services senden, ohne dass bei jedem Aufruf Serviceberechtigungsnachweise integriert sein müssen. Bei der IAM-Authentifizierung werden Zugriffstokens zum Authentifizieren verwendet, die Sie erhalten, indem Sie eine Anforderung mit einem API-Schlüssel senden. {: shortdesc}

{{site.data.keyword.watson}}-Services, die in einer Ressourcengruppe erstellt oder von Cloud Foundry in eine Ressourcengruppe migriert werden, verwenden die IAM-Authentifizierung. Standardmäßig verwenden alle neuen {{site.data.keyword.watson}}-Services die IAM-Authentifizierung. {: note}

## IAM-Serviceberechtigungsnachweise abrufen
{: #iam-getting-credentials-manually}

Für den Zugriff auf API-Methoden mit IAM-Serviceberechtigungsnachweisen müssen die Berechtigungsnachweise zuerst abgerufen werden. Sie können über die {{site.data.keyword.cloud_notm}}-Webschnittstelle auf die Serviceberechtigungsnachweise zugreifen. 

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window} an. 
1.  Rufen Sie die [Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard){: new_window} auf. 
1.  Wählen Sie eine Serviceinstanz aus. 
1.  Klicken Sie auf **Berechtigungsnachweise anzeigen**, um den **API-Schlüssel** und die **URL** für die Berechtigungsnachweise anzuzeigen. 
    1.  Wenn Sie die Tokens für die Berechtigungsnachweise anzeigen möchten, wählen Sie im Navigationsmenü links auf der Seite die Option **Serviceberechtigungsnachweise** aus. 
    1.  Wählen Sie **Berechtigungsnachweise anzeigen** aus, um den vollständigen API-Schlüssel und die Tokeninformationen für die Berechtigungsnachweise anzuzeigen. 

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Automatisch generierter API-Schlüssel bei der Ressourcenschlüsseloperation für...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## IAM-Serviceberechtigungsnachweise aktualisieren
{: #update-existing-svcs}

Sie können die Serviceberechtigungsnachweise für eine vorhandene Serviceinstanz über das Service-Dashboard aktualisieren.

1.  Melden Sie sich bei [{{site.data.keyword.cloud_notm}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}){: new_window} an. 
1.  Rufen Sie die [Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/dashboard){: new_window} auf. 
1.  Wählen Sie **Serviceberechtigungsnachweise** im Navigationsmenü links auf der Seite aus. 
1.  Verwenden Sie die Menüs und Symbole, um vorhandene Berechtigungsnachweise zu löschen oder neue Berechtigungsnachweise hinzuzufügen. 

Stellen Sie sicher, dass Sie die Berechtigungsnachweise in Ihren Anwendungen bei Änderungen aktualisieren. 

## IAM-Token mithilfe eines API-Schlüssels für einen Watson-Service abrufen
{: #iamtoken}

Für den Zugriff auf die {{site.data.keyword.ibmwatson_notm}}-Service-APIs können die API-Schlüssel verwendet werden, die für die Serviceinstanz generiert wurden. Mithilfe des API-Schlüssels wird ein IAM-Zugriffstoken generiert. Dieser Prozess wird auch bei der Entwicklung einer Anwendung eingesetzt, die mit anderen {{site.data.keyword.cloud_notm}}-Services interagieren muss. 

Weitere Informationen zur sicheren Verwendung von API-Schlüsseln finden Sie im Abschnitt [API-Schlüssel des IAM-Service](/docs/services/watson?topic=watson-api-key-bp).
{: tip}

Im folgenden curl-Befehl wird die Methode `POST identity/token` zur Generierung eines IAM-Zugriffstokens durch die Übergabe eines API-Schlüssels verwendet. Der Header `Content-Type` gibt an, dass für die Daten eine URL-Codierung verwendet wird. Mit den `data-urlencode`-Parametern werden die Daten mit der URL-Codierung an die Methode übergeben. 

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Verwenden Sie die Authentifizierung, um das Zugriffstoken zu generieren. Verwenden Sie dieselbe Authentifizierung, wenn Sie das Token aktualisieren. 

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"

```
{: pre}

Das folgende Beispiel zeigt die erwartete Antwort: 

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: pre}

## Token für die Authentifizierung verwenden
{: #use_token}

Ein IAM-Zugriffstoken wird mithilfe der Methode `POST identity/token` generiert. Das Token kann dann verwendet werden, um authentifizierte API-Aufrufe abzusetzen. Ein API-Schlüssel ist einer bestimmten Serviceinstanz zugeordnet. Ein Token, das mit einem API-Schlüssel generiert wird, kann ausschließlich für Aufrufe an die entsprechende Serviceinstanz verwendet werden. 

Ein typischer curl-Aufruf an einen {{site.data.keyword.watson}}-Service weist das folgende Format auf. Dabei ist `{token}` das generierte IAM-Zugriffstoken: 

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

Alternativ dazu können Sie dem Token das Präfix `Authorization: Bearer ` voranstellen (z. B. `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`) und es in einer Datei speichern. Anschließend verwenden Sie den folgenden Befehl:

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## Token aktualisieren
{: #refresh_token}

Sie können das IAM-Aktualisierungstoken verwenden, das von der Methode `POST identity/token` generiert wird, um die Lebensdauer des Zugriffstokens zu verlängern. Mit dem folgenden Befehl wird ein vorhandenes Zugriffstoken aktualisiert. Im Befehl ist `{refresh-token}` das generierte IAM-Aktualisierungstoken. 

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Verwenden Sie bei der Aktualisierung eines Tokens dieselbe Authentifizierung wie bei der Erstellung des ursprünglichen Tokens.
{: important}
