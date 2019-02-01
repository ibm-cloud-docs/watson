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

# Desenvolvendo um aplicativo {{site.data.keyword.watson}} em Node.js

Para demonstrar o uso de sua API de REST, cada serviço do {{site.data.keyword.ibmwatson}} fornece os aplicativos baseados na web de amostra em Node.js no projeto GitHub do {{site.data.keyword.watson}}.
{: shortdesc}

## Antes de iniciar

- Crie uma conta no {{site.data.keyword.cloud_notm}} ou use uma conta existente. [Inscreva-se gratuitamente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. Sua conta deve ter espaço para pelo menos um aplicativo e um serviço.
- O [tempo de execução de Node.js ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://nodejs.org/#download){: new_window}, incluindo o gerenciador de pacote [npm](https://www.npmjs.com/){: new_window}.  Certifique-se de incluir o comando na variável de ambiente `PATH` após a instalação.
- O cliente da linha de comando do [Cloud Foundry ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Se você o instalou anteriormente, certifique-se de que a sua versão esteja atualizada.

## Obtendo o código-fonte

Visite o projeto GitHub do [{{site.data.keyword.watson}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud){: new_window} para ver os aplicativos Node.js de amostra estão disponíveis para o {{site.data.keyword.watson}}. Em seguida, use o GitHub para clonar um repositório localmente.

## Instalando localmente
Se quiser modificar o app ou utilizá-lo como uma base para construir seu próprio app, instale-o localmente. Será possível então implementar sua versão modificada do app para {{site.data.keyword.cloud_notm}}.

### Configurando um serviço {{site.data.keyword.watson}}

1.  Na linha de comandos, acesse o diretório de projeto local do app clonado.
1.  Efetue login no {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Localize o nome do serviço e o plano. Se o arquivo `manifest.yml` do app de amostra incluir uma seção `declared-services`, observe o `label` e o `plan`. Caso contrário, use o comando `cf marketplace` para obter o nome do serviço e os planos:

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Crie uma instância do serviço no {{site.data.keyword.cloud_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. Para nome do serviço e plano de serviço, use as informações do arquivo manifest ou o comando `cf marketplace`.

    Por exemplo, emita o comando a seguir para criar a instância de serviço para o serviço {{site.data.keyword.personalityinsightsshort}}:

    ```bash
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Configurando o ambiente do app

1.  Copie o arquivo `.env.example` para um novo arquivo `.env`.
1.  Crie uma chave de serviço no formato `cf create-service-key {service_instance} {service_key}`. Por exemplo:

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Recupere as credenciais da chave de serviço usando o comando `cf service-key {service_instance} {service_key}`. Por exemplo:

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    A saída desse comando é um objeto JSON, como neste exemplo:

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api", "nome do usuário": "583b2e88-c80d-4ec0-93c1-98239f805146", "senha": "RuytRliRvoFN" }
    ```
    {: codeblock}

1.  Cole os valores `password` e `username` (sem aspas) do JSON em variáveis
relevantes no arquivo `.env`. Por exemplo:

    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### Instalando e iniciando o aplicativo

1.  Instale o pacote do aplicativo demo no ambiente de tempo de execução Node.js local:

    ```bash
    npm install
    ```
    {: pre}

1.  Inicie o aplicativo:

    ```bash
    npm start
    ```
    {: pre}

1.  Aponte seu navegador para http://localhost:3000 para experimentar o aplicativo. A porta é especificada no arquivo `app.js`.

## Implementando no {{site.data.keyword.cloud_notm}}

É possível usar o Cloud Foundry para implementar sua versão local do app no {{site.data.keyword.cloud_notm}}.

1.  No diretório-raiz do projeto, abra o arquivo manifest.yml:
    1.  Na seção `applications` do arquivo `manifest.yml`, mude o valor do nome para um nome exclusivo para sua versão do app.
    1.  Na seção `services`, especifique o nome da instância de serviço do {{site.data.keyword.watson}} criada para o app. Se não se lembrar do nome de serviço, use o comando `cf services` para listar seus serviços.

        O exemplo a seguir mostra um arquivo `manifest.yml` modificado:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memória: 256 M instâncias: 1 serviços: - my-personality-insights-service env: NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Efetue login no {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Envie por push o app para o {{site.data.keyword.cloud_notm}} e especifique o nome do app por meio do arquivo `manifest.yml`. 
Por exemplo:

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Aponte seu navegador para a URL especificada na saída de comando.
