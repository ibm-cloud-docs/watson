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

# Running examples from the Node.js SDK

The {{site.data.keyword.watson}} Node.js Software Development Kit (SDK) includes example code for each service.
{: shortdesc}

## Before you begin

1.  Create a {{site.data.keyword.Bluemix}} account, or use an existing account. [Sign-up for free ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Get the service credentials for the {{site.data.keyword.watson}} service you want to run. For details, see [Service credentials for {{site.data.keyword.watson}} services](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Make sure that you have the [Node.js runtime ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://nodejs.org/#download){: new_window},  installed, including the [npm ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.npmjs.com/){: new_window}. package manager.  Make sure to include the command on your `PATH` environment variable after installation.

## Update and run the example

1.  Save a copy of the example code for the {{site.data.keyword.watson}} service you want to run.
    1.  Go to the `/examples` directory in the watson-developer-cloud [Node SDK ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window}.
    1.  Save the code from an example locally. We'll use `personality_insights.v3.js`.
1.  Change to the directory where you saved the code and install the Node.js SDK:

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Update the example code to use your service credentials (api_key or username and password):

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    You might have to make other changes. For example, here you add enough text for {{site.data.keyword.personalityinsightsshort}} to analyze.

1.  Issue the command `node {example_application.vn}.js` to run the code. For example,

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    The response includes information from the service. For example, with the {{site.data.keyword.personalityinsightsshort}} service, you see output similar to this truncated example:

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
