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

# Esecuzione di esempi dall'SDK Node.js

L'SDK (Software Development Kit) Node.js {{site.data.keyword.watson}} include il codice di esempio per ogni servizio.
{: shortdesc}

## Prima di cominciare

1.  Crea un account {{site.data.keyword.Bluemix}} o utilizza un account esistente. [Registrati gratuitamente ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Ottieni le credenziali del servizio per il servizio {{site.data.keyword.watson}} che vuoi eseguire. per i dettagli, consulta [Credenziali del servizio per i servizi {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Assicurati di avere il [runtime Node.js ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://nodejs.org/#download){: new_window} installato, compreso il gestore pacchetti [npm ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.npmjs.com/){: new_window}. Assicurati di includere il comando nella tua variabile di ambiente `PATH` dopo l'installazione.

## Aggiorna ed esegui l'esempio

1.  Salva una copia del codice di esempio per il servizio {{site.data.keyword.watson}} che vuoi eseguire.
    1.  Vai alla directory `/examples` in watson-developer-cloud [Node SDK![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window}.
    1.  Salva il codice da un esempio in locale. Utilizzeremo `personality_insights.v3.js`.
1.  Passa alla directory dove hai salvato il codice e installa l'SDK Node.js:

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Aggiorna il codice di esempio per utilizzare le tue credenziali del servizio (api_key oppure nome utente e password):

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    Potresti dover fare altre modifiche. Ad esempio, qui aggiungi abbastanza testo per l'analisi da parte di {{site.data.keyword.personalityinsightsshort}}.

1.  Immetti il comando `node {example_application.vn}.js` per eseguire il codice. Ad esempio,

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    La risposta include le informazioni dal servizio. Ad esempio, con il servizio {{site.data.keyword.personalityinsightsshort}}, vedi un output simile a questo esempio troncato:

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
