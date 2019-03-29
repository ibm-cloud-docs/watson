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

# Authentification avec des jetons IAM
{: #iam}

Vous utilisez des jetons IAM (Identity and Access Management) {{site.data.keyword.cloud}} pour écrire des applications qui envoient des demandes authentifiées à des services {{site.data.keyword.ibmwatson}} sans incorporer de données d'identification du service dans chaque appel. L'authentification IAM utilise des jetons pour l'authentification, que vous pouvez acquérir en envoyant une demande avec une clé d'API.
{: shortdesc}

Les services {{site.data.keyword.watson}} qui sont créés dans un groupe de ressources ou qui sont migrés depuis Cloud Foundry vers un groupe de ressources utilisent l'authentification IAM. Par défaut, tous les nouveaux services {{site.data.keyword.watson}} utilisent l'authentification IAM.
{: note}

## Obtention des données d'identification du service IAM
{: #iam-getting-credentials-manually}

Pour accéder aux méthodes d'API à l'aide de données d'identification du service IAM, vous devez commencer par collecter les données d'identification du service. Vous pouvez accéder aux données d'identification du service dans l'interface Web d'{{site.data.keyword.cloud_notm}}.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Accédez à votre [liste de ressources![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/dashboard){: new_window}.
1.  Sélectionnez une instance de service.
1.  Cliquez sur **Afficher les données d'identification** pour voir la **Clé d'API** et l'**URL** des données d'identification. 
    1.  Pour voir les jetons des données d'identification, sélectionnez **Données d'identification du service** dans le menu de navigation sur la gauche de la page.
    1.  Sélectionnez **Afficher les données d'identification** pour voir les informations de clé d'API et de jeton complètes pour les données d'identification.

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

## Mise à jour des données d'identification du service IAM
{: #update-existing-svcs}

Vous pouvez mettre à jour les données d'identification de service d'une instance de service existante à partir du tableau de bord du service.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Accédez à votre [liste de ressources![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/dashboard){: new_window}.
1.  Sélectionnez **Données d'identification du service** dans le menu de navigation sur la gauche de la page.
1.  Utilisez les menus et les icônes pour supprimer des données d'identification existantes ou pour en ajouter de nouvelles.

Assurez-vous de mettre à jour les données d'identification dans vos applications en cas de modifications :

## Obtention d'un jeton IAM à l'aide d'une clé d'API de service Watson
{: #iamtoken}

Vous pouvez accéder aux API de service {{site.data.keyword.ibmwatson_notm}} à l'aide des clés d'API qui ont été générées pour l'instance de service. Vous utilisez la clé d'API pour générer un jeton d'accès IAM. Vous utilisez également ce processus si vous développez une application qui doit être compatible avec d'autres services {{site.data.keyword.cloud_notm}}.

Pour plus d'informations sur l'utilisation en toute sécurité de vos clés d'API, voir [Clés d'API de service IAM](/docs/services/watson?topic=watson-api-key-bp).
{: tip}

La commande curl suivante utilise la méthode `POST identity/token` pour générer un jeton d'accès IAM en transmettant une clé d'API. L'en-tête `Content-Type` indique que les données sont codées en URL. Les paramètres `data-urlencode` transmettent les données codées en URL à la méthode.

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Utilisez l'authentification pour générer le jeton d'accès. Utilisez la même authentification lors de l'actualisation du jeton. 

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

L'exemple suivant illustre la réponse attendue :

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

## Utilisation d'un jeton pour l'authentification
{: #use_token}

Vous générez un jeton d'accès IAM à l'aide de la méthode `POST identity/token`. Vous pouvez ensuite utiliser le jeton pour effectuer des appels d'API authentifiés. Une clé d'API est associée à une instance de service spécifique. Un jeton qui est généré avec une clé d'API peut être utilisé uniquement pour les appels à cette instance de service.

Un appel curl classique à un service {{site.data.keyword.watson}} prend la forme suivante, où `{token}` est le jeton d'accès IAM que vous avez généré :

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

Vous pouvez aussi préfixer le jeton avec `Authorization: Bearer ` (par exemple `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`) et l'enregistrer dans un fichier. Vous utilisez ensuite la commande suivante :


```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## Actualisation d'un jeton
{: #refresh_token}

Vous pouvez utiliser le jeton d'actualisation IAM qui est généré par la méthode `POST identity/token` pour allonger la durée de vie du jeton d'accès. La commande suivante actualise un jeton d'accès existant. Dans la commande, `{refresh-token}` est le jeton d'actualisation IAM que vous avez généré.

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Lorsque vous actualisez un jeton, utilisez la même authentification que celle que vous avez utilisée pour créer le jeton d'origine.
{: important}
