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

# Autenticazione con i token IAM
{: #iam}

Utilizza i token {{site.data.keyword.cloud}} Identity and Access Management (IAM) per eseguire richieste autenticate a servizi {{site.data.keyword.ibmwatson}} senza integrare le credenziali del servizio in ogni chiamata. L'autenticazione IAM utilizza i token di accesso per l'autenticazione, che puoi acquisire inviando una richiesta con una chiave API.
{: shortdesc}

I servizi {{site.data.keyword.watson}} che vengono creati in un gruppo di risorse o che vengono migrati da Cloud Foundry a un gruppo di risorse utilizzano l'autenticazione IAM. Per impostazione predefinita, tutti i nuovi servizi {{site.data.keyword.watson}} utilizzano l'autenticazione IAM.
{: note}

## Come ottenere le credenziali del servizio IAM
{: #iam-getting-credentials-manually}

Per accedere ai metodi API utilizzando le credenziali del servizio IAM, devi innanzitutto raccogliere le credenziali. Puoi accedere alle credenziali del servizio dall'interfaccia web {{site.data.keyword.cloud_notm}}.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}.
1.  Vai al tuo [elenco di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard){: new_window}.
1.  Seleziona un'istanza del servizio.
1.  Fai clic su **Visualizza credenziali** per vedere la **Chiave API** e l'**Url** relativi alle credenziali. 
    1.  Per vedere i token relativi alle credenziali, seleziona **Credenziali del servizio** dal menu di navigazione di sinistra della pagina. 
    1.  Seleziona **Visualizza credenziali** per visualizzare le informazioni complete sulla chiave API e sul token delle credenziali. 

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Auto generated apikey during resource-key operation for...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## Aggiornamento delle credenziali del servizio IAM
{: #update-existing-svcs}

Puoi aggiornare le credenziali del servizio per un'istanza del servizio esistente dal dashboard del servizio.

1.  Accedi a [{{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}){: new_window}.
1.  Vai al tuo [elenco di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/dashboard){: new_window}.
1.  Seleziona **Credenziali del servizio** dal menu di navigazione di sinistra della pagina.
1.  Utilizza i menu e le icone per eliminare le credenziali esistenti o per aggiungerne di nuove.

Assicurati di aggiornare le credenziali nelle tue applicazioni per eventuali modifiche. 

## Come ottenere un token IAM utilizzando una chiave API del servizio Watson
{: #iamtoken}

Puoi accedere alle API del servizio {{site.data.keyword.ibmwatson_notm}} utilizzando le chiavi API che sono state generate per l'istanza del servizio. Utilizza la chiave API per generare un token di accesso IAM. Utilizza questo processo anche se stai sviluppando un'applicazione che deve utilizzare altri servizi {{site.data.keyword.cloud_notm}}.

Per ulteriori informazioni su come utilizzare le chiavi API in modo sicuro, vedi [Chiavi API del servizio IAM](/docs/services/watson?topic=watson-api-key-bp).
{: tip}

Il seguente comando curl utilizza il metodo `POST identity/token` per generare un token di accesso IAM passando una chiave API. L'intestazione `Content-Type` indica che i dati sono codificati tramite l'URL. I parametri `data-urlencode` passano i dati codificati tramite l'URL al metodo. 

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Utilizza l'autenticazione per generare il token di accesso. Utilizza la stessa autenticazione quando aggiorni il token.

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

Il seguente esempio mostra la risposta prevista:

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

## Utilizzo di un token per l'autenticazione
{: #use_token}

Genera un token di accesso IAM utilizzando il metodo `POST identity/token`. Puoi quindi utilizzare il token per effettuare chiamate API autenticate. Viene associata una chiave API a una specifica istanza del servizio. Un token che viene generato con una chiave API può essere utilizzato solo per le chiamate a tale istanza del servizio. 

Una chiamata curl tipica a un servizio {{site.data.keyword.watson}} ha il seguente formato, dove `{token}` è il token di accesso IAM che hai generato:

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

In alternativa, puoi aggiungere `Authorization: Bearer ` al token come prefisso (ad esempio `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`) e salvarlo in un file. Poi, utilizza il seguente comando: 

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## Aggiornamento di un token
{: #refresh_token}

Puoi utilizzare il token di aggiornamento IAM generato dal metodo `POST identity/token` per estendere la durata del token di accesso. Il seguente comando aggiorna un token di accesso esistente. Nel comando, `{refresh-token}` è il token di aggiornamento IAM che hai generato. 

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Quando aggiorni un token, utilizza la stessa autenticazione che hai utilizzato per creare il token originale.
{: important}
