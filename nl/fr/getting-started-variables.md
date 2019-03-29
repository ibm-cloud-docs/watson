---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: environment variable,service alias,VCAP services

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

# Variable d'environnement VCAP\_SERVICES
{: #vcapServices}

La variable d'environnement `VCAP_SERVICES` contient des informations que vous pouvez utiliser pour interagir avec une instance de service dans {{site.data.keyword.cloud}}. Les zones de cette variable d'environnement sont définies lorsque vous liez une instance de service à une application.
{: shortdesc}

Votre application a besoin de l'URL et des données d'identification du service pour s'authentifier auprès d'un service {{site.data.keyword.watson}}.En général, après qu'un service a été lié à une application en cours d'exécution, celle-ci lit ces variables d'environnement afin d'extraire les paires nom-valeur requises. Vous pouvez lier les services d'un groupe de ressources ou les services Cloud Foundry à votre appli, mais les services d'un groupe de ressources ont besoin d'un alias de service.

## Alias de service pour les services IAM
{: #vcapIAMAlias}

Vous pouvez utiliser la variable d'environnement `VCAP_SERVICES` avec des services de groupe de ressources en affectant un alias au service. Par exemple, utilisez la commande suivante avec l'interface de ligne de commande {{site.data.keyword.cloud_notm}} pour créer un alias appelé `personality-insights-service` pour un service nommé `my-personality-insights-service`.

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

Vous pouvez ensuite lier votre application à l'alias de service. En général, après qu'une instance de service a été liée à une application en cours d'exécution, celle-ci lit ces variables d'environnement afin d'extraire les paires nom-valeur requises. 

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Exemple
{: #gs-variables-vcapIAMAliasExample}

Cet exemple illustre la variable d'environnement `VCAP_SERVICES` du service {{site.data.keyword.personalityinsightsshort}} lié à une application. L'instance de service utilise l'authentification Cloud Foundry.

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

| Elément     | Description                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | Mot de passe issu des données d'authentification HTTP de base utilisées pour se connecter  |
|          | au service |
| url      | Adresse URL de connexion du service                                                         |
| username | Nom d'utilisateur issu des données d'authentification HTTP de base utilisées pour se       |
|          | connecter au service |
| label    | Nom associé au service dans {{site.data.keyword.cloud_notm}}                                            |
| name     | Nom de l'instance de service                                                           |
| plan     | Plan disponible pour le service dans {{site.data.keyword.cloud_notm}}                                              |
| tags     | Renseignements supplémentaires sur service                                                   |

## Affichage des variables d'environnement
{: #gs-variables-vcapView}

Vous pouvez extraire des informations concernant la variable à partir de l'interface de ligne de commande {{site.data.keyword.cloud_notm}} ou à partir de l'interface Web.

- Avec l'interface CLI {{site.data.keyword.cloud_notm}} : Appelez la commande Cloud Foundry `env` après avoir créé un service et l'avoir lié à votre application :

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- A partir de l'interface {{site.data.keyword.cloud_notm}} : les informations concernant les variables d'environnement sont disponibles dans la page de récapitulatif de chaque application.
