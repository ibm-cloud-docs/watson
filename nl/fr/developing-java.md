---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-09"

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

# Développement d'une application {{site.data.keyword.watson}} dans Java

Pour illustrer l'utilisation de l'API REST, chaque service {{site.data.keyword.ibmwatson}} fournit des exemples d'application Web dans Java dans le projet GitHub {{site.data.keyword.watson}}.
{: shortdesc}

## Avant de commencer

- Créez un compte sur {{site.data.keyword.cloud_notm}} ou utilisez un compte existant. [Inscription gratuite ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
- Installez le client de ligne de commande [Cloud Foundry ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli#downloads){: new_window}. S'il est déjà installé, vérifiez que votre version est à jour.
- Téléchargez et installez [Apache Maven ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://maven.apache.org/download.cgi){: new_window}.
- Installez me [profil IBM WebSphere Liberty ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/wasdev/downloads/){: new_window}. Vous pouvez télécharger uniquement la version d'exécution du profil WebSphere Liberty ou télécharger une version en tant que plug-in pour l'interface IDE Eclipse. Installez et configurez également soit le *JDK Java * soir l'*interface IDE Eclipse*.

## Obtention du code source
Accédez au projet [GitHub {{site.data.keyword.watson}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud){: new_window} pour voir les exemples d'application Java disponibles pour {{site.data.keyword.watson}}. Utilisez ensuite GitHub pour cloner un référentiel en local.

## Installation en local
Si vous voulez modifier l'application ou l'utiliser comme base pour élaborer votre propre application, installez-la en local. Vous pouvez ensuite déployer votre version modifiée de l'application dans {{site.data.keyword.cloud_notm}}.

### Configuration d'un service {{site.data.keyword.watson}}

1.  Sur la ligne de commande, accédez au répertoire du projet local de l'application clonée.
1.  Connectez-vous à {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Recherchez le nom et le plan du service. Si le fichier `manifest.yml` de l'exemple d'application contient une section `declared-services`, notez la valeur des zones `label` et `plan`. Sinon, utilisez la commande `cf marketplace` pour obtenir le nom et le plan du service :

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Créez une instance du service dans {{site.data.keyword.cloud_notm}} : `cf create-service {nom-service} {plan-service} {nom-instance-service}`. Pour le nom du service et le plan du service utilisez les informations du fichier manifeste ou la commande `cf marketplace`.

    Par exemple, exécutez la commande suivante pour créer l'instance de service pour le service {{site.data.keyword.personalityinsightsshort}} :

    ```bash
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### Configuration de l'environnement d'application

1.  Créez une clé de service au format `cf create-service-key {instance-service} {clé-service}`. Par exemple :

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Extrayez les données d'identification de la clé de service avec la commande `cf service-key {instance-service} {clé-service}`. Par exemple :

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    La sortie de cette commande est un objet JSON, comme dans cet exemple :

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }

    ```
    {: codeblock}
1.  Copiez les valeurs de la clé de service dans le fichier source de l'application, situé dans le répertoire de travail de l'application :

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### Génération et exécution de l'application

Les instructions qui suivent utilisent la version d'exécution du profil WebSphere Liberty. Vous devrez les modifier si vous utilisez le plug-in Eclipse.
{: tip}

1.  Générez l'application en exécutant la commande Maven suivante depuis le répertoire de travail de l'application :

    ```java
    mvn clean package
    ```
    {: pre}

1.  Exécutez la commande de profil WebSphere Liberty `server create` pour créer le serveur pour votre application. Le nom du serveur indiqué en argument de la commande arbitraire, mais il doit être unique au sein de votre environnement de profil WebSphere Liberty local.

    ```bash
    server create server-name
    ```
    {: pre}

1.  Copiez le fichier `.war` de l'application dans le répertoire `wlp-installation/usr/servers/server-name/dropins`, où *wlp-installation* est le répertoire de base de votre installation locale de profil WebSphere Liberty et *server-name* le nom indiqué pour le serveur à l'étape précédente.
1.  Démarrez le serveur dans le profil WebSphere Liberty. Indiquez le nom du serveur de l'application en tant qu'argument de la commande.

    ```bash
    server start server-name
    ```
    {: pre}

1.  Pointez votre navigateur sur http://localhost:9080/webApp pour tester l'application. Le port est indiqué dans le fichier `app.js`.

    Le port est indiqué dans le fichier `wlp-installation/usr/servers/server-name/server.xml`. Pour changer le port modifiez la valeur de `server.xml`, puis arrêtez et redémarrez le serveur.

### Traitement des incidents

Si le serveur ne démarre pas ou ne répond pas, examinez les fichiers journaux dans le répertoire `wlp-installation/usr/servers/server-name/logs` afin de déterminer la cause de l'échec.

## Déploiement dans {{site.data.keyword.cloud_notm}}

Vous pouvez utiliser Cloud Foundry pour déployer votre version locale de l'application dans {{site.data.keyword.cloud_notm}}.

1.  Dans le répertoire racine du projet, ouvrez le fichier manifest.yml :
    1. Dans la section `applications` du fichier `manifest.yml`, remplacez la valeur de nom par un nom unique pour votre version de l'application.
    1. Dans la section `services`, indiquez le nom de l'instance de service {{site.data.keyword.watson}} créée pour votre application. Si vous ne vous souvenez pas du nom du service, utilisez la commande `cf services` pour lister vos services.

        L'exemple suivant présente un fichier `manifest.yml` modifié :

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: tiered
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Connectez-vous à {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Envoyez l'application à {{site.data.keyword.cloud_notm}} et indiquez le nom de votre application issu du fichier `manifest.yml`. Par exemple,

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Pointez votre navigateur sur l'adresse URL spécifiée dans le résultat de la commande.
