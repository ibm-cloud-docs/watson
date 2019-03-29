---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Watson tokens,Cloud Foundry

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

# Tokens do Watson
{: #gs-tokens-watson-tokens}

Use tokens do {{site.data.keyword.watson}} para gravar aplicativos que fazem solicitações autenticadas para os serviços do {{site.data.keyword.ibmwatson}} sem integrar as credenciais de serviço do Cloud Foundry em cada chamada. Deve-se usar as credenciais de serviço do Cloud Foundry para que um serviço obtenha um token do {{site.data.keyword.watson}}. Para obter mais informações, consulte [Autenticando com as credenciais de serviço do Cloud Foundry](/docs/services/watson?topic=watson-creating-credentials). Obtenha um token para uma instância de serviço específica e ele funcionará somente para essa instância.
{: shortdesc}

Por padrão, o {{site.data.keyword.cloud_notm}} usa a autenticação do Identity and Access Management (IAM) para todas as novas instâncias de serviço. Os tokens descritos aqui funcionam com as credenciais de serviço do Cloud Foundry. Para obter mais informações sobre a autenticação do IAM, consulte [Autenticando com tokens do IAM](/docs/services/watson?topic=watson-iam#iam).
{: important}

É possível gravar um proxy de autenticação no {{site.data.keyword.cloud}} que obtém e retorna um token para
o seu aplicativo cliente, que pode, então, usar o token para chamar o serviço diretamente. Esse proxy elimina a necessidade de
canalizar todas as solicitações de serviço por meio de um aplicativo do lado do servidor intermediário que, caso contrário, seria necessário para
evitar expor suas credenciais de serviço do seu aplicativo cliente.

## Obtendo um token
{: #gs-tokens-obtain}

Para obter um token para um serviço, você envia uma solicitação HTTP `GET` para o
método `token` da API de `autorização`. O host depende da localização e do serviço que você está usando. É
possível identificar o host do terminal para a API do serviço.

Para identificar o serviço para o qual você deseja obter um token, passe a URL base do serviço por meio do
parâmetro de consulta `url` do método. Por exemplo, o comando curl a seguir usa o parâmetro `url` para buscar um token para o serviço {{site.data.keyword.texttospeechshort}}:

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

Passe as suas credenciais de autenticação básica HTTP para o serviço com a solicitação como você faria para qualquer
chamada que não usa tokens. Por exemplo, o comando curl a seguir obtém um token para o serviço
{{site.data.keyword.texttospeechshort}}:

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

A opção `--user` transmite a concatenação de seu *nome de usuário* e *senha* para o serviço. É possível obter as credenciais do {{site.data.keyword.cloud_notm}} ou da variável de ambiente `VCAP_SERVICES` para um aplicativo que está ligado a uma instância do serviço. Certifique-se de usar suas credenciais de serviço, não o seu ID de login e senha do {{site.data.keyword.cloud_notm}}.

O método retorna o token como uma codificação Base64 de 1 kilobyte de uma carga útil criptografada.

Os tokens têm um tempo de vida (TTL) de 1 hora. Depois disso, não é mais possível usá-los para estabelecer uma conexão com o serviço. As conexões existentes que já estão estabelecidas com o token não são afetadas pelo tempo limite. Uma tentativa de passar um token
inválido ou expirado induz um código de status HTTP `401 Unauthorized` do {{site.data.keyword.cloud_notm}}. O seu código do aplicativo precisa estar preparado para atualizar o token em resposta a esse código de retorno.

## Usando um token
{: #gs-tokens-watson-use}

Passe um token para a interface HTTP de um serviço usando o cabeçalho da
solicitação `X-Watson-Authorization-Token`, o parâmetro de consulta `watson-token` ou um
cookie. Deve-se passar o token com cada solicitação de HTTP.

Os exemplos de curl a seguir demonstram as duas primeiras abordagens.

1.  Obtenha um token para o serviço {{site.data.keyword.texttospeechshort}} e grave-o em um arquivo chamado `token`.

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  Passe o conteúdo do arquivo `token` em uma chamada para o método `synthesize` da API {{site.data.keyword.texttospeechshort}}. O primeiro comando passa o token por meio do cabeçalho `X-Watson-Authorization-Token`. O segundo comando passa-o por
meio do parâmetro de consulta `watson-token`. Os dois comandos são equivalentes. Cada comando grava a saída da
chamada em um arquivo denominado `hello_world.wav`.

    -   *Em um shell UNIX ou Linux,* redirecione o conteúdo do arquivo `token` para o
comando:

        ```bash
        curl -X POST \
      --header "X-Watson-Authorization-Token: $(< ./token)" \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token=$(< ./token)"
        ```
        {: pre}

    -   *Em um prompt de comandos do Windows, * integre o conteúdo do arquivo `token` no comando:

        ```bash
        curl -X POST
        --header "X-Watson-Authorization-Token: {token_string}"
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token={token_string}"
        ```
        {: pre}
