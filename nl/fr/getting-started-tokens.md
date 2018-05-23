---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-07"

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

# Jetons d'authentification
Vous utilisez des jetons pour écrire des applications qui envoient des demandes authentifiées à des services {{site.data.keyword.ibmwatson}} sans incorporer de données d'identification du service dans chaque appel.
{: shortdesc}

Vous pouvez écrire dans {{site.data.keyword.cloud}} un proxy d'authentification qui obtient et renvoie un jeton à votre application client, qui peut ensuite utiliser le jeton pour appeler directement le service. Avec ce proxy, vous n'avez plus besoin d'acheminer toutes les demandes de service via une application côté serveur intermédiaire, ce qui est nécessaire pour éviter d'exposer vos données d'identification du service depuis votre application client.

Pour plus d'informations sur le modèle de programmation d'interaction directe, voir [Modèles de programmation pour les services {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

Vous devez disposez des données d'authentification HTTP de base du service pour obtenir un jeton pour le service. Pour plus d'informations, voir [Données d'identification du service pour les services {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html). Vous obtenez un jeton pour un service spécifique et le jeton ne fonctionne que pour ce service.

## Obtention d'un jeton
Pour obtenir un jeton pour un service, envoyez une demande HTTP `GET` à la méthode `token` de l'API `authorization`. L'hôte est fonction du service que vous utilisez. Vous pouvez identifier l'hôte à partir du noeud final de l'API du service.

- **gateway.watsonplatform.net** : les services tels que {{site.data.keyword.personalityinsightsshort}} appellent la méthode `token` au noeud final `https://gateway.watsonplatform.net/authorization/api/v1/token`
- **stream.watsonplatform.net** : les services tels que {{site.data.keyword.speechtotextshort}} et {{site.data.keyword.texttospeechshort}} appellent la méthode `token` à l'adresse https://stream.watsonplatform.net/authorization/api/v1/token`

Pour identifier le service pour lequel vous voulez obtenir un jeton, transmettez l'URL de base du service via le paramètre de requête `url` de la méthode. Par exemple, transmettez la valeur suivante pour le paramètre `url` afin d'extraire un jeton pour le service {{site.data.keyword.texttospeechshort}} :

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

L'option `--user` transmet la concaténation de vos paramètres *username* et *password* pour le service, que vous pouvez obtenir depuis {{site.data.keyword.cloud_notm}} ou la variable d'environnement `VCAP_SERVICES` d'une application liée à une instance du service. Veillez à utiliser vos données d'identification du service et non vos ID de connexion et mot de passe {{site.data.keyword.cloud_notm}}.

La méthode renvoie le jeton sous forme de contenu chiffré codé en Base64 de un kilooctet.

Les jetons ont une durée de vie d'une heure au-delà de laquelle vous ne pouvez plus les utiliser pour établir une connexion au service. Les connexions existantes déjà établies avec le jeton ne sont pas affectées par l'expiration du délai. Toute tentative de transmission d'un jeton expiré ou non valide induit l'émission d'un code d'état HTTP `401 Unauthorized` par {{site.data.keyword.cloud_notm}}. Votre code d'application doit être préparé à actualiser le jeton en réponse à ce code de retour.

## Utilisation d'un jeton

Vous transmettez un jeton à l'interface HTTP d'un service à l'aide de l'en-tête de requête `X-Watson-Authorization-Token`, du paramètre de requête `watson-token` ou d'un cookie. Vous devez transmettre le jeton avec chaque demande HTTP.

Les exemples de commande curl suivants illustrent les deux premières approches.

- Obtention d'un jeton pour le service {{site.data.keyword.texttospeechshort}} et écriture du jeton dans le fichier nommé `token`.

  ```bash
  curl -X GET --user {username}:{password}
  --output token
  "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
  ```
  {: pre}

- Transmission du contenu du fichier `token` dans un appel de la méthode `synthesize` de l'API {{site.data.keyword.texttospeechshort}}. La première commande transmet le jeton via l'en-tête `X-Watson-Authorization-Token`. La seconde commande le transmet via le paramètre de requête `watson-token`. Les deux commandes sont équivalentes. Chaque commande écrit le résultat de l'appel dans un fichier nommé `hello_world.wav`.

    - *A partir d'un interpréteur de commandes UNIX ou Linux,* réacheminez le contenu du fichier `token` vers la commande :

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

    - *A partir d'une invite de commandes Windows,* incorporez le contenu du fichier `token` dans la commande :

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
