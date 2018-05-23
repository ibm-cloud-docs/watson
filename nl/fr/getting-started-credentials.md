---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Données d'identification de service pour les services Watson

Les données d'identification de service d'un service {{site.data.keyword.ibmwatson}} garantissent l'authentification d'un service dans {{site.data.keyword.cloud}}. Un ensemble de données d'identification ne peut être utilisé qu'avec le service pour lequel elles a été créées.
{: shortdesc}

Les données d'identification du service sont utilisées différemment selon le modèle de programmation utilisé. Pour plus d'informations, voir [Modèles de programmation pour les services {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

Les données d'identification du service sont les données d'authentification HTTP de base propres à un service {{site.data.keyword.watson}}.

## Obtention des donnée d'identification manuellement
{: #getting-credentials-manually}

Vous trouverez les données d'identification du service dans l'interface Web d'{{site.data.keyword.cloud_notm}}.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Accédez à vos services existants. ([Découvrir ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Sélectionnez le menu en haut à gauche, puis sélectionnez **{{site.data.keyword.watson}}** pour accéder à la console {{site.data.keyword.watson}}.
    1.  Sélectionnez **Services existants** dans **Services Watson** pour afficher la liste de vos services et projets.
1.  Affichez les données d'identification :
    - Si le service fait partie d'un projet, cliquez sur le nom du projet qui contient le service. Affichez les données d'identification depuis la section **Données d'identification** de la page des détails du projet.
    - Si le service ne fait pas partie d'un projet, cliquez sur le nom du service que vous voulez afficher, puis sélectionnez **Données d'identification du service**.

## Mise à jour des données d'identification d'un service
{: #existing-svcs}

Vous pouvez mettre à jour les données d'identification de service d'une instance de service existante à partir du tableau de bord du service.

1.  Connectez-vous à [{{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Accédez à vos services existants. ([Découvrir ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Sélectionnez le menu en haut à gauche, puis sélectionnez **{{site.data.keyword.watson}}** pour accéder à la console {{site.data.keyword.watson}}.
    1.  Sélectionnez **Services existants** dans **Services Watson** pour afficher la liste de vos services.
1.  Sélectionnez le service que vous voulez afficher ou mettre à jour, puis sélectionnez **Données d'identification pour le service**.
1.  Supprimez ou ajoutez des données d'identification :
    - Pour supprimer des données d'identification, sélectionnez-les puis sélectionnez **Supprimer des données d'identification**.
    - Pour ajouter des données d'identification, cliquez sur **Nouvelles données d'identification**, puis sur **Ajouter** dans la fenêtre "Ajouter de nouvelles données d'identification". Affichez et copiez les nouvelles données d'identification.

Assurez-vous d'utiliser les nouvelles données d'identification dans vos applications :

- Si votre application obtient ses données d'identification à l'aide d'un programme, arrêtez puis redémarrez votre application.
- Si vos données d'identification sont intégrées dans votre code d'application, mettez à jour le code, puis relancez l'application.

Pour plus d'informations sur la mise à jour d'une application avec de nouvelles données d'identification avec un temps d'indisponibilité minimal, voir [Mise à jour des applications](/docs/manageapps/updapps.html).

## Données d'identification dans {{site.data.keyword.Bluemix_dedicated_notm}}

Dans une instance [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated), les vignettes de service sont implémentées de manière à n'autoriser qu'une seule vignette par service dans une organisation.

Pour faire référence à un service dans votre espace de travail {{site.data.keyword.Bluemix_dedicated_notm}}, procédez comme suit :

1.  Créez un ensemble de données d'identification pour la vignette de référence de service que vous voulez utiliser.
1.  Créez un service personnalisé fourni par l'utilisateur, qui utilise ces données d'identification :

    1.  Utilisez l'interface {{site.data.keyword.cloud_notm}} pour sélectionner la vignette de service que vous voulez utiliser et créez un ensemble de données d'identification pour le service. Les données d'identification sont créées au format JSON et contiennent des zones distinctes pour l'adresse URL à laquelle communiquer avec le service et pour le nom d'utilisateur et le mot de passe à utiliser avec ce service.
    1.  Utilisez la commande `cf cups` (créer un service fourni par l'utilisateur) pour créer un service personnalisé lié à ces données d'identification. Par exemple, avec les données d'identification suivantes ,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      créez une instance de service personnalisé fourni par l'utilisateur nommée `Personality-Insights-Std`:

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Veillez à indiquer le nom d'utilisateur et le mot de passe dans une chaîne unique encadrée de guillemets et séparée par un signe deux points. Encadrez également les chaînes username et password de guillemets et démarquez-les avec une barre oblique inversée d'échappement.
      {: tip}

### Utilisation d'applications avec {{site.data.keyword.Bluemix_dedicated_notm}}

Certains exemples d'application {{site.data.keyword.watson}} et kits de démarrage sur GitHub présentent un bouton de **déploiement sur {{site.data.keyword.cloud_notm}}**. Ce bouton ne fonctionne pas dans les installations {{site.data.keyword.Bluemix_dedicated_notm}} car il renvoie à l'environnement {{site.data.keyword.cloud_notm}} public.

Pour utiliser les exemples d'application et les kits de démarrage dans les environnements {{site.data.keyword.Bluemix_dedicated_notm}} :

1.  Dans les instructions liées à l'application, vérifiez que le fichier `manifest.yml` que vous créez utilise le nom du service que vous avez créé avec la commande `cf cups`.
1.  Dans ces mêmes instructions, au lieu d'utiliser la commande `cf create-service`, utilisez plutôt une commande `cf cups`. Toutefois, si vous aviez créé une instance de service fourni par l'utilisateur personnalisé, vous n'avez pas besoin de la créer de nouveau pour l'utiliser avec l'exemple d'application ou le kit de démarrage.
