---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Watson tokens,Cloud Foundry

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

# Jetons Watson
{: #gs-tokens-watson-tokens}

Vous utilisez des jetons {{site.data.keyword.watson}} pour écrire des applications qui envoient des demandes authentifiées à des services {{site.data.keyword.ibmwatson}} sans incorporer de données d'identification du service Cloud Foundry dans chaque appel. Vous devez utiliser des données d'identification du service Cloud Foundry pour un service afin d'obtenir un jeton {{site.data.keyword.watson}}. Pour plus d'informations, voir [Authentification avec des données d'identification du service Cloud Foundry](/docs/services/watson?topic=watson-creating-credentials). Vous obtenez un jeton pour un service spécifique et le jeton ne fonctionne que pour cette instance.
{: shortdesc}

Par défaut, {{site.data.keyword.cloud_notm}} utilise l'authentification IAM (Identity and Access Management) pour toutes les nouvelles instances de service. Les jetons décrits ici fonctionnent avec les données d'identification du service Cloud Foundry. Pour plus d'informations sur l'authentification IAM, voir [Authentification avec des jetons IAM](/docs/services/watson?topic=watson-iam#iam).
{: important}

Vous pouvez écrire dans {{site.data.keyword.cloud}} un proxy d'authentification qui obtient et renvoie un jeton à votre application client, qui peut ensuite utiliser le jeton pour appeler directement le service. Avec ce proxy, vous n'avez plus besoin d'acheminer toutes les demandes de service via une application côté serveur intermédiaire, ce qui est nécessaire pour éviter d'exposer vos données d'identification du service depuis votre application client.

## Obtention d'un jeton
{: #gs-tokens-obtain}

Pour obtenir un jeton pour un service, envoyez une demande HTTP `GET` à la méthode `token` de l'API `authorization`. L'hôte dépend de l'emplacement et du service que vous utilisez. Vous pouvez identifier l'hôte à partir du noeud final de l'API du service.

Pour identifier le service pour lequel vous voulez obtenir un jeton, transmettez l'URL de base du service via le paramètre de requête `url` de la méthode. Par exemple, la commande curl suivante utilise le paramètre `url` afin d'extraire un jeton pour le service {{site.data.keyword.texttospeechshort}} :

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

Vous transmettez les données d'authentification de base HTTP du service avec la requête comme vous le feriez pour n'importe quel appel qui n'utilise pas de jetons. Par exemple, la commande curl suivante permet d'obtenir un jeton pour le service {{site.data.keyword.texttospeechshort}} :

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

L'option `--user` transmet la concaténation de vos paramètres *username* et *password* pour le service. Vous pouvez obtenir les données d'identification auprès de {{site.data.keyword.cloud_notm}} ou à partir de la variable d'environnement `VCAP_SERVICES` pour une application qui est liée à une instance du service. Veillez à utiliser vos données d'identification du service et non vos ID de connexion et mot de passe {{site.data.keyword.cloud_notm}}.

La méthode renvoie le jeton sous forme de contenu chiffré codé en Base64 de 1 kilooctet.

Les jetons ont une durée de vie d'une heure au-delà de laquelle vous ne pouvez plus les utiliser pour établir une connexion au service. Les connexions existantes déjà établies avec le jeton ne sont pas affectées par l'expiration du délai. Toute tentative de transmission d'un jeton expiré ou non valide induit l'émission d'un code d'état HTTP `401 Unauthorized` par {{site.data.keyword.cloud_notm}}. Votre code d'application doit être préparé à actualiser le jeton en réponse à ce code de retour.

## Utilisation d'un jeton
{: #gs-tokens-watson-use}

Vous transmettez un jeton à l'interface HTTP d'un service à l'aide de l'en-tête de requête `X-Watson-Authorization-Token`, du paramètre de requête `watson-token` ou d'un cookie. Vous devez transmettre le jeton avec chaque demande HTTP.

Les exemples de commande curl suivants illustrent les deux premières approches.

1.  Obtention d'un jeton pour le service {{site.data.keyword.texttospeechshort}} et écriture du jeton dans le fichier nommé `token`.

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  Transmission du contenu du fichier `token` dans un appel de la méthode `synthesize` de l'API {{site.data.keyword.texttospeechshort}}. La première commande transmet le jeton via l'en-tête `X-Watson-Authorization-Token`. La seconde commande le transmet via le paramètre de requête `watson-token`. Les deux commandes sont équivalentes. Chaque commande écrit le résultat de l'appel dans un fichier nommé `hello_world.wav`.

    -   *A partir d'un interpréteur de commandes UNIX ou Linux,* réacheminez le contenu du fichier `token` vers la commande :

        ```bash
        curl -X POST \
      --header "X-Watson-Authorization-Token: $(< ./token)" \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token=$(< ./token)"
        ```
        {: pre}

    -   *A partir d'une invite de commandes Windows,* incorporez le contenu du fichier `token` dans la commande :

        ```bash
        curl -X POST
        --header "X-Watson-Authorization-Token: {token_string}"
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token={token_string}"
        ```
        {: pre}
