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

# Exécution d'exemples à partir du kit de développement de logiciels (SDK) de Node.js

Le kit de développement de logiciels (SDK) Node.js de {{site.data.keyword.watson}} inclut un exemple de code pour chaque service.
{: shortdesc}

## Avant de commencer

1.  Créez un compte {{site.data.keyword.Bluemix}} ou utilisez un compte existant. [Inscription gratuite ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Procurez-vous les données d'identification de service du service {{site.data.keyword.watson}} que vous voulez exécuter. Pour plus d'informations, voir [Données d'identification de service pour les services {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Assurez-vous que l'[environnement d'exécution Node.js ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://nodejs.org/#download){: new_window} est installé, y compris le gestionnaire de package [npm ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.npmjs.com/){: new_window}. Veillez à inclure la commande dans votre variable d'environnement `PATH` après l'installation.

## Mise à jour et exécution de l'exemple

1.  Enregistrer une copie de l'exemple de code pour le service {{site.data.keyword.watson}} que vous voulez exécuter.
    1.  Accédez au répertoire `/examples` dans le [SDK Node ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window} du cloud des développeurs Watson (watson-developer-cloud).
    1.  Sauvegardez le code d'un exemple en local. Nous utiliserons `personality_insights.v3.js`.
1.  Accédez au répertoire dans lequel vous avez sauvegardé le code et installez le SDK Node.js :

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Mettez à jour l'exemple de code de manière à utiliser vos données d'identification de service (clé d'API, nom d'utilisateur et mot de passe) :

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    Vous devrez peut-être apporter d'autres modifications. Par exemple, ajouter ici suffisamment de texte pour une analyse {{site.data.keyword.personalityinsightsshort}}.

1.  Entrez la commande `node {example_application.vn}.js` pour exécuter le code. Par exemple,

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    La réponse inclut des informations issues du service. Par exemple, avec le service {{site.data.keyword.personalityinsightsshort}}, vous obtenez un résultat semblable à cet exemple tronqué :

    ```json
    {
      "word_count": 224,
      "word_count_message": "There were 224 words in the input. We need a minimum of 600, preferably 1,200 or more, to compute statistically significant estimates",
      "processed_lang": "en",
      "personality": [
      {
        "trait_id": "big5_openness",
        "name": "Openness",
        "category": "personality",
        "percentile": 0.9015311332932557,
        "children": [. . .]
      },
      . . .
      ],
      "needs": [. . .],
      "values": [. . .],
      "consumption_preferences": [. . .]
    }
    ```
    {: codeblock}
