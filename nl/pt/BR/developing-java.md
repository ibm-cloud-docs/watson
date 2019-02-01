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

# Desenvolvendo um aplicativo {{site.data.keyword.watson}} em Java

Para demonstrar o uso de sua API de REST, cada serviço do {{site.data.keyword.ibmwatson}} fornece amostras de
aplicativos baseados na web em Java no projeto GitHub do {{site.data.keyword.watson}}.
{: shortdesc}

## Antes de iniciar

- Crie uma conta no {{site.data.keyword.cloud_notm}} ou use uma conta existente. [Inscreva-se gratuitamente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
- Instale o cliente da linha de comando do [Cloud Foundry
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}.
Se ele já estiver instalado, verifique se a versão está atualizada.
- Faça download e instale o [Apache Maven
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://maven.apache.org/download.cgi){: new_window}.
- Instale o perfil do [IBM WebSphere Liberty
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/wasdev/downloads/){: new_window}.
É possível fazer download apenas da versão de runtime do perfil do WebSphere Liberty ou fazer download de uma versão como um
plug-in para o Eclipse IDE. Também instalar e configurar o *Java JDK* ou o *Eclipse IDE*.

## Obtendo o código-fonte
Visite o projeto GitHub do [{{site.data.keyword.watson}}
![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud){: new_window}
para ver os aplicativos Java de amostra que estão disponíveis para o {{site.data.keyword.watson}}. Em seguida, use o GitHub para clonar um repositório localmente.

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
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### Configurando o ambiente do app

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
1.  Copie os valores da chave de serviço no arquivo de origem para o aplicativo, localizado no diretório ativo do
aplicativo:

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### Construindo e executando o aplicativo

Essas instruções usam a versão de runtime do perfil do WebSphere Liberty. É necessário modificar as etapas ao usar o plug-in do Eclipse.
{: tip}

1.  Construa o aplicativo executando o comando Maven a seguir do diretório ativo do aplicativo:

    ```java
    mvn clean package
    ```
    {: pre}

1.  Emita o comando `server create` do perfil do WebSphere Liberty para criar o servidor para o seu
aplicativo. O nome do servidor especificado como o argumento para o comando é arbitrário, mas deve ser exclusivo em sua
variável de ambiente do perfil do WebSphere Liberty local.

    ```bash
    server create server-name
    ```
    {: pre}

1.  Copie o arquivo `.war` do aplicativo para o diretório
`wlp-installation/usr/servers/server-name/dropins`, em que *wlp-installation* é o diretório base
para a sua instalação local do perfil do WebSphere Liberty e *server-name* é o nome especificado para o servidor na etapa
anterior.
1.  Inicie o servidor no perfil do WebSphere Liberty. Especifique o nome do servidor para o aplicativo como o argumento para o
comando.

    ```bash
    server start server-name
    ```
    {: pre}

1.  Aponte seu navegador para http://localhost:9080/webApp para experimentar o aplicativo. A porta é especificada no arquivo `app.js`.

    A porta é especificada no arquivo `wlp-installation/usr/servers/server-name/server.xml`. Para mudar a
porta, mude o valor em `server.xml` e, em seguida, pare e reinicie o servidor.

### Resolução de problemas

Se o servidor falhar ao iniciar corretamente ou falhar ao responder, examine os arquivos de log no diretório
`wlp-installation/usr/servers/server-name/logs` para determinar a causa.

## Implementando no {{site.data.keyword.cloud_notm}}

É possível usar o Cloud Foundry para implementar sua versão local do app no {{site.data.keyword.cloud_notm}}.

1.  No diretório-raiz do projeto, abra o arquivo manifest.yml:
    1. Na seção `applications` do arquivo `manifest.yml`, mude o valor do nome para um nome exclusivo para sua versão do app.
    1. Na seção `services`, especifique o nome da instância de serviço do {{site.data.keyword.watson}} criada para o app. Se não se lembrar do nome de serviço, use o comando `cf services` para listar seus serviços.

        O exemplo a seguir mostra um arquivo `manifest.yml` modificado:

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
          memória: 256 M instâncias: 1 serviços: - my-personality-insights-service env: NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Efetue login no {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Envie por push o app para o {{site.data.keyword.cloud_notm}} e especifique o nome do app por meio do arquivo `manifest.yml`. Por exemplo,

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Aponte seu navegador para a URL especificada na saída de comando.
