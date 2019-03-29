---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# Authentification avec des données d'identification du service Cloud Foundry 
{: #creating-credentials}

Les données d'identification du service Cloud Foundry pour les service {{site.data.keyword.ibmwatson}} permettent l'authentification auprès d'un service dans {{site.data.keyword.cloud}}. Les données d'identification du service utilisent l'authentification de base HTTP pour s'authentifier auprès d'un service {{site.data.keyword.watson}} spécifique.
{: shortdesc}

Les services {{site.data.keyword.watson}} qui sont créés dans un groupe de ressources ou qui sont migrés depuis Cloud Foundry vers un groupe de ressources utilisent l'authentification IAM. Par défaut, tous les nouveaux services {{site.data.keyword.watson}} utilisent l'authentification IAM. Pour plus d'informations sur les groupes de ressources et les authentifications IAM, voir [Authentification avec des jetons IAM](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually).
{: note}

## Obtention des données d'identification du service Cloud Foundry 
{: #getting-credentials-manually}

Pour accéder aux méthodes d'API à l'aide de données d'identification du service Cloud Foundry, vous devez commencer par collecter les données d'identification du service. Vous pouvez accéder aux données d'identification du service dans l'interface Web d'{{site.data.keyword.cloud_notm}}.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Accédez à votre [liste de ressources![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/dashboard){: new_window}.
1.  Sélectionnez une instance de service.
1.  Cliquez sur **Afficher les données d'identification** pour voir le **Nom d'utilisateur**, le **Mot de passe** et l'**URL** des données d'identification.

## Mise à jour des données d'identification du service Cloud Foundry 
{: #existing-svcs}

Vous pouvez mettre à jour les données d'identification de service d'une instance de service existante à partir du tableau de bord du service.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}){: new_window}.
1.  Accédez à votre [liste de ressources![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/dashboard){: new_window}.
1.  Sélectionnez **Données d'identification du service** dans le menu de navigation sur la gauche de la page.
1.  Utilisez les menus et les icônes pour supprimer des données d'identification existantes ou pour en ajouter de nouvelles.

Assurez-vous de mettre à jour les données d'identification dans vos applications en cas de modifications :

## Données d'identification dans {{site.data.keyword.Bluemix_dedicated_notm}}

Dans une instance [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated), les vignettes de service sont implémentées de manière à n'autoriser qu'une seule vignette par service dans une organisation.

Pour faire référence à un service dans votre espace de travail {{site.data.keyword.Bluemix_dedicated_notm}}, procédez comme suit :

1.  Créez un ensemble de données d'identification pour la vignette de référence de service que vous voulez utiliser.
1.  Créez un service personnalisé fourni par l'utilisateur, qui utilise ces données d'identification :

    1.  Utilisez l'interface Web pour sélectionner la vignette de service que vous voulez utiliser et créez un ensemble de données d'identification pour le service. Les données d'identification sont créées au format JSON et elles incluent l'URL, le nom d'utilisateur et le mot de passe du noeud final de service.
    1.  Utilisez la commande `ibmcloud cf cups` (créer un service fourni par l'utilisateur) pour créer un service personnalisé lié à ces données d'identification. Par exemple,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Avec ces données d'identification, créez une instance de service personnalisé fourni par l'utilisateur nommée `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Veillez à indiquer le nom d'utilisateur et le mot de passe dans une chaîne unique encadrée de guillemets et séparée par un signe deux points. Encadrez également les chaînes username et password de guillemets et démarquez-les avec une barre oblique inversée d'échappement.
      {: tip}

### Utilisation d'applications avec {{site.data.keyword.Bluemix_dedicated_notm}}

Certains exemples d'application {{site.data.keyword.watson}} et kits de démarrage sur GitHub présentent un bouton de **déploiement sur {{site.data.keyword.cloud_notm}}**. Ces boutons ne fonctionnent pas dans les installations {{site.data.keyword.Bluemix_dedicated_notm}} car ils renvoient à l'environnement {{site.data.keyword.cloud_notm}} public.

Pour utiliser les exemples d'application et les kits de démarrage dans les environnements {{site.data.keyword.Bluemix_dedicated_notm}}, procédez comme suit :

1.  Dans les instructions liées à l'application, vérifiez que le fichier `manifest.yml` que vous créez utilise le nom du service que vous avez créé avec la commande `ibmcloud cf cups`.
1.  Dans ces mêmes instructions, au lieu d'utiliser la commande `ibmcloud cf create-service`, utilisez plutôt une commande `ibmcloud cf cups`. Toutefois, si vous aviez créé une instance de service fourni par l'utilisateur personnalisé, vous n'avez pas besoin de la créer de nouveau pour l'utiliser avec l'exemple d'application ou le kit de démarrage.
