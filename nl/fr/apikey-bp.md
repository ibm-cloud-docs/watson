---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM authentication,service keys,service roles,best practices

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

# Clés d'API de service IAM
{: #api-key-bp}

Les clés d'API de service {{site.data.keyword.ibmwatson}} permettent d'acquérir des jetons bearer qui à leur tout sont utilisés pour authentifier des appels vers les services {{site.data.keyword.ibmwatson_notm}}.
{: shortdesc}

A la différence de l'implémentation IAM (Identity and Access Management) {{site.data.keyword.cloud_notm}} standard des clés d'API, les clés IAM {{site.data.keyword.ibmwatson_notm}} sont définies sur une base par service. Vous pouvez ainsi affecter des droits à chaque clé d'API au lieu de chaque service. Cette implémentation diffère de la méthode {{site.data.keyword.cloud_notm}} standard des clés d'APU basées sur les utilisateurs. Avec {{site.data.keyword.ibmwatson_notm}}, un rôle est affecté aux utilisateurs pour chaque service. Pour plus d'informations sur l'implémentation {{site.data.keyword.cloud_notm}} générale, voir [Meilleures pratiques pour clés d'API](/docs/services/iam?topic=iam-iamoverview#iamoverview).

Vous pouvez lier une clé d'API Watson uniquement à une instance de service unique.
{: tip}

Un ensemble de données d'identification pour le rôle `Responsable` est automatiquement généré chaque fois qu'une instance de service {{site.data.keyword.ibmwatson_notm}} est créée. Ces données d'identification sont à votre disposition sur la page **Tableau de bord** de l'instance de service. Vous pouvez les utiliser pour appeler *toutes* les méthodes de l'API du service. Vous pouvez limiter les actions possibles qui sont effectuée lors de l'utilisation du service en créant une clé d'API avec un rôle différent.

| Rôle Accès au service | Description des actions |
|:-----------------|:-----------------|
| `Lecteur` | Les lecteurs peuvent effectuer des actions en lecture seule au sein d'un service, comme l'affichage de ressources spécifiques à un service. |
| `Auteur` |Les auteurs disposent de droits en plus du rôle de lecteur, par exemple, créer et éditer des ressources pour un service. |
| `Responsable` |Les responsables disposent de droits en plus du rôle Auteur pour effectuer des actions privilégiées définies par le service. De plus, ils peuvent créer et éditer des ressources spécifiques à un service. |
{: caption="Table 1. Exemple de rôles Accès au service pour les utilisateurs" caption-side="top"}

Pour plus d'informations sur la création de données d'identification du service {{site.data.keyword.ibmwatson_notm}} supplémentaires, voir [Mise à jour des données d'identification du service IAM](/docs/services/watson?topic=watson-iam#update-existing-svcs).

## Meilleures pratiques pour clés d'API
{: #api-bp}

Tout comme pour la méthode d'authentification auprès de votre service, vous devez garantir la sécurité de vos clés d'API. Les meilleures pratiques ci-après vous permettent de garantir la sécurité d'une clé d'API et de réduire le risque d'une exposition publique de vos données d'identification qui compromettrait votre application.

-   *Utilisez une clé d'API dont le rôle est approprié à la fonction que vous utilisez.*

    Lorsque vous créez une application pour utilisateur final, pensez à utiliser une clé d'API avec le rôle `Lecteur` pour tous les appels des méthodes d'API `GET`. Le rôle `Lecteur` ne permet pas d'effectuer des actions destructrices ou supplémentaires pour l'instance de service.

-   *N'imbriquez pas la clé d'API directement dans le code.*

    Les clés d'API qui sont imbriqués dans le code peuvent être exposées à vos utilisateurs. Au lieu d'imbriquer les clés d'API dans le code de votre application, envisagez de les stocker dans des variables d'environnement ou dans des fichiers en dehors de votre système de gestion de code source.

-   *Ne stockez pas une clé d'API dans des fichiers situés à l'intérieur du système de gestion de votre code source. *

    Si vous stockez les clés d'API dans des fichiers, conservez-les à l'extérieur du système de gestion de votre code source. Cette pratique est particulièrement importante si vous utilisez un système de gestion de code source public tel que GitHub.

-   *Regénérez régulièrement vos clés d'API.*

    Vous pouvez créer des nouvelles données d'identification  à tout moment.
    1.  Dans la liste de ressources, sélectionnez une instance de service.
    1.  Cliquez sur **Données d'identification du service** dans le menu de navigation sur la gauche de la page.

     Lorsque vous migrez vos application vers les nouvelles données d'identification, n'oubliez pas de supprimer les anciennes données d'identification au moyen de l'icône de corbeille associé aux données d'identification que vous voulez supprimer.
     {: tip}
