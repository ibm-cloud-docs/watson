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

# Executando exemplos do SDK do Node.js

O {{site.data.keyword.watson}} Node.js Software Development Kit (SDK) inclui código de exemplo para cada serviço.
{: shortdesc}

## Antes de iniciar

1.  Crie uma conta do {{site.data.keyword.Bluemix}} ou use uma conta existente. [Inscreva-se gratuitamente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Obtenha as credenciais de serviço para o serviço do {{site.data.keyword.watson}} que você deseja executar. Para
obter detalhes, consulte
[Credenciais de serviço para
os serviços {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Certifique-se de que você tenha o [tempo de execução do Node.js
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://nodejs.org/#download){: new_window},
instalado, incluindo o [npm ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de linkexterno")](https://www.npmjs.com/){: new_window}. Gerenciador de pacote. Certifique-se de incluir o comando na variável de ambiente `PATH` após a instalação.


## Atualize e execute o exemplo

1.  Salve uma cópia do código de exemplo para o serviço do {{site.data.keyword.watson}} que você deseja executar.
    1.  Acesse o diretório `/examples` no watson-developer-cloud
[Nó SDK
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window}.
    1.  Salve o código de um exemplo localmente. Vamos usar `personality_insights.v3.js`.
1.  Mude para o diretório no qual você salvou o código e instale o SDK Node.js:

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Atualize o código de exemplo para usar suas credenciais de serviço (api_key ou nome do usuário e senha):

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    Você pode ter que fazer outras mudanças. Por exemplo, aqui inclua texto suficiente para o {{site.data.keyword.personalityinsightsshort}} analisar.

1.  Emita o comando `node {example_application.vn}.js` para executar o código. Por exemplo,

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    A resposta inclui informações do serviço. Por exemplo, com o serviço do
{{site.data.keyword.personalityinsightsshort}}, você verá uma saída semelhante a esse exemplo truncado:

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
