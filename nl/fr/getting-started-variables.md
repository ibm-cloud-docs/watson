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

# Variable d'environnement VCAP\_SERVICES
{: #vcapServices}

La variable d'environnement `VCAP_SERVICES` contient des informations que vous pouvez utiliser pour interagir avec une instance de service dans {{site.data.keyword.Bluemix}}. Les zones de cette variable d'environnement sont définies lorsque vous liez un service à une application.
{: shortdesc}

Votre application a besoin de l'adresse URL et des données d'authentification HTTP de base du service pour s'authentifier auprès d'un service {{site.data.keyword.watson}}.

En général, après qu'un service a été lié à une application en cours d'exécution, celle-ci lit ces variables d'environnement afin d'extraire les paires nom-valeur requises. Pour plus d'informations sur les variables, recherchez "Variables d'environnement" dans [Déploiement d'applications](/docs/manageapps/depapps.html#app_env).

## Exemple
Cet exemple illustre la variable d'environnement `VCAP_SERVICES` du service {{site.data.keyword.personalityinsightsshort}} lié à une application :

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

Le tableau suivant décrit le contenu de la variable d'environnement `VCAP_SERVICES`.

| Elément  | Description                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | Mot de passe issu des données d'authentification HTTP de base utilisées pour se connecter  |
|          | au service                                                                                 |
| url      | Adresse URL de connexion du service                                                        |
| username | Nom d'utilisateur issu des données d'authentification HTTP de base utilisées pour se       |
|          | connecter au service                                                                       |
| label    | Nom associé au service dans {{site.data.keyword.cloud_notm}}                       |
| name     | Nom de l'instance de service                                                               |
| plan     | Plan disponible pour le service dans {{site.data.keyword.cloud_notm}}              |
| tags     | Renseignements supplémentaires sur service                                                 |

## Affichage des variables d'environnement
Vous pouvez extraire des informations concernant la variable à partir de l'outil Cloud Foundry ou de l'interface {{site.data.keyword.cloud_notm}}.

- Avec l'outil de ligne de commande Cloud Foundry : utilisez la commande `cf env` après avoir créé un service et l'avoir lié à votre application:

    ```bash
    cf env {nom-application}
    ```
    {: pre}

- A partir de l'interface {{site.data.keyword.cloud_notm}} : les informations concernant les variables d'environnement sont disponibles dans la page de récapitulatif de chaque application.
