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

# Interface de ligne de commande Cloud Foundry

Vous pouvez utiliser l'outil d'interface de ligne de commande (interface CLI) `cf` pour travailler avec des applications {{site.data.keyword.cloud}} sur votre système local. Cet outil offre une interface enrichie permettant une expérience de développement complète.
{: shortdesc}

Pour plus d'informations sur l'utilisation des outils Cloud Foundry pour le développement d'application, voir [Fonctionnement de Cloud Foundry avec {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) ainsi que la [Documentation Cloud Foundry ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.cloudfoundry.org/){: new_window}.

## Installation de l'interface de ligne de commande cf

La dernière version de cet outil est disponible dans la section **Downloads** du [référentiel GitHub de Cloud Foundry![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Deux types de packages sont proposés :

- **Installers**, qui s'intègre à l'outil de gestion des packages ou au système de suivi des logiciels installés de votre plateforme pour installer l'outil à son emplacement "officiel".
- **Binaries**, qui fournit des fichiers archives que vous pouvez installer à n'importe quel emplacement.

Veillez à inclure la commande dans votre variable d'environnement `PATH` après avoir installé l'outil.
{: tip}

## Commandes pour des instances de service

Commandes `cf` utiles pour la gestion des instances de service {{site.data.keyword.watson}} existantes dans {{site.data.keyword.cloud_notm}} :

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services) : dresse la liste des instances de service de l'espace en cours.

  ```bash
  cf services
  ```
  {: pre}

  Si vous connaissez le nom du service, utilisez la commande `cf service` pour afficher les informations concernant le service. Par exemple,

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service) : crée une instance de service que vous pouvez lier à l'application que vous exécutez dans {{site.data.keyword.cloud_notm}} :

    ```bash
    cf create-service {nom-service} {plan-service} {nom-instance-service}
    ```
    {: pre}

    - Pour les arguments *{nom-service}* et *{plan-service}*, servez-vous du résultat de la commande `cf marketplace`.
    - Indiquez le {nom-instance-service} que vous voulez créer. Pour bon nombre des applications, le nom est indiqué dans le fichier manifeste `manifest.yml` ou un autre fichier de configuration de l'application.

- **cf delete-service** : supprime une instance de service.

    ```bash
    cf delete-service {nom-instance-service}
    ```
    {: pre}

    En supprimant un service dont vous n'avez plus besoin, vous libérez dans {{site.data.keyword.cloud_notm}} des ressources dont vous n'avez pas besoin.

## Commandes pour le déploiement et l'exécution des applications

Commandes `cf` communes pour déployer et exécuter des applications dans {{site.data.keyword.cloud_notm}} :

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api) : indique l'emplacement des API {{site.data.keyword.watson}} :

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login). Connectez-vous à {{site.data.keyword.cloud_notm}} avec votre {{site.data.keyword.ibmid}} et votre mot de passe :

  ```bash
  cf login -u {nom d'utilisateur} -p {mot de passe}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace) : dresse la liste des services du catalogue {{site.data.keyword.cloud_notm}} auxquels vous pouvez vous connecter :

  ```bash
  cf marketplace
  ```
  {: pre}

  Le résultat de la commande inclut le nom de chaque service et la liste des plans sous lesquels le service est disponible. Vous avez besoin de ces informations lorsque vous créez une instance de service pour votre application qui s'exécute dans {{site.data.keyword.cloud_notm}}.

  Si vous connaissez le nom du service, utilisez l'option `-s`. Par exemple,

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push) : déploie ou met à jour une application sur {{site.data.keyword.cloud_notm}}. Le résultat de la commande inclut l'adresse URL permettant d'accéder à votre application.

  ```bash
  cf push {nom-application}
  ```
  {: pre}

  La commande utilise souvent le fichier manifeste `manifest.yml` ou autre fichier de configuration pour rechercher le fichier à envoyer (par exemple, un fichier `.war` pour Java ou un fichier `.zip` pour Node.js). Toutefois, vous pouvez indiquer le chemin d'accès au fichier d'application avec l'option `-p`.

## Commandes pour des applications existantes

Commandes `cf` utiles pour la gestion des applications existantes dans {{site.data.keyword.cloud_notm}} :

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps) : dresse la liste des applications déployées dans l'espace en cours.

  ```bash
  cf apps
  ```
  {: pre}

  Incluez le nom d'application pour afficher des informations détaillées concernant l'état de santé et le statut d'une application :

  ```bash
  cf app {nom-application}
  ```
  {: pre}

- **cf restage** : recharge et redémarre votre application dans {{site.data.keyword.cloud_notm}} :

  ```bash
  cf restage {nom-application}
  ```
  {: pre}

Cette commande redémarre l'application telle qu'elle existe dans {{site.data.keyword.cloud_notm}}. Pour télécharger la dernière version de l'application dans {{site.data.keyword.cloud_notm}} avant de la redémarrer, utilisez plutôt la commande `cf push`.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete) : supprime une application :

  ```bash
  cf delete nom-application
  ```
  {: pre}

  La suppression d'une application libère de la mémoire et du quota {{site.data.keyword.cloud_notm}} pour une utilisation avec d'autres applications.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service) : lie un service à une application.

  En général, vous utilisez la commande `cf push` pour lier une instance d'un service à une application qui l'utilise. Vous pouvez lier une instance de service existante à une application existante avec la commande `cf bind-service` :

  ```bash
  cf bind-service {nom-application} {nom-instance-service}
  ```
  {: pre}

  Exécutez ensuite la commande `cf restage` ou `cf push` pour redémarrer l'application.

- **cf unbind-service** : supprime l'association entre une application et un service :

  ```bash
  cf unbind-service {nom-application} {nom-instance-service}
  ```
  {: pre}

  Vous devrez sans doute exécuter la commande `cf restage` ou `cf push` pour répercuter la modification.

  Cette commande supprime vos données d'authentification de la variable d'environnement `VCAP_SERVICES`. Utilisez la commande `cf delete-service` ou `cf delete` pour supprimer l'instance de service ou l'application.
  {: tip}
