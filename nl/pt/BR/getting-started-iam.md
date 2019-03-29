---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM tokens,IAM authentication

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

# Autenticando com tokens do IAM
{: #iam}

Use tokens do {{site.data.keyword.cloud}} Identity and Access Management (IAM) para fazer solicitações autenticadas para os serviços do {{site.data.keyword.ibmwatson}} sem integrar as credenciais de serviço em cada chamada. A autenticação do IAM usa tokens de acesso para autenticação, que você adquire enviando uma solicitação com uma chave de API.
{: shortdesc}

Os serviços do {{site.data.keyword.watson}} que são criados em um grupo de recursos ou migrados do Cloud Foundry para um grupo de recursos usam a autenticação do IAM. Por padrão, todos os novos serviços do {{site.data.keyword.watson}} usam a autenticação do IAM.
{: note}

## Obtendo credenciais de serviço do IAM
{: #iam-getting-credentials-manually}

Para acessar os métodos de API usando as credenciais de serviço do IAM, deve-se primeiro coletar as credenciais. É possível acessar as credenciais de serviço por meio da interface da web do {{site.data.keyword.cloud_notm}}.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
1.  Acesse a [lista de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard){: new_window}.
1.  Selecione uma instância de serviço.
1.  Clique em **Mostrar credenciais** para ver a **Chave de API** e a **URL** para as credenciais.
    1.  Para ver os tokens para as credenciais, selecione **Credenciais de serviço** no menu de navegação à esquerda da página.
    1.  Selecione **Visualizar credenciais** para visualizar as informações completas da chave de API e do token para as credenciais.

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Auto generated apikey during resource-key operation for...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## Atualizando as credenciais de serviço do IAM
{: #update-existing-svcs}

É possível atualizar as credenciais de serviço para uma instância de serviço existente no painel de serviço.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
1.  Acesse a [lista de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard){: new_window}.
1.  Selecione **Credenciais de serviço** no menu de navegação à esquerda da página.
1.  Use os menus e ícones para excluir as credenciais existentes ou para incluir novas credenciais.

Certifique-se de atualizar as credenciais em seus aplicativos para qualquer mudança.

## Obtendo um token do IAM usando uma chave de API de serviço do Watson
{: #iamtoken}

É possível acessar as APIs de serviço do {{site.data.keyword.ibmwatson_notm}} usando as chaves de API que foram geradas para a instância de serviço. Use a chave de API para gerar um token de acesso do IAM. Use também esse processo se estiver desenvolvendo um aplicativo que precisa funcionar com outros serviços do {{site.data.keyword.cloud_notm}}.

Para obter mais informações sobre o uso seguro de chaves de API, consulte [Chaves de API de serviço do IAM](/docs/services/watson?topic=watson-api-key-bp).
{: tip}

O comando curl a seguir usa o método `POST identity/token` para gerar um token de acesso do IAM, transmitindo uma chave de API. O cabeçalho `Content-Type` indica que os dados são codificados pela URL. Os parâmetros `data-urlencode` passam os dados codificados pela URL para o método.

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Use a autenticação para gerar o token de acesso. Use a mesma autenticação ao atualizar o token.

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"

```
{: pre}

A amostra a seguir mostra a resposta esperada:

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: pre}

## Usando um token para se autenticar
{: #use_token}

Gere um token de acesso do IAM usando o método `POST identity/token`. É possível, então, usar o token para fazer chamadas API autenticadas. Uma chave de API está associada a uma instância de serviço específica. Um token que é gerado com uma chave de API pode ser usado somente para chamadas para essa instância de serviço.

Uma chamada de curl típica para um serviço do {{site.data.keyword.watson}} assume o formato a seguir, em que `{token}` é o token de acesso do IAM que você gerou:

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

Como alternativa, é possível prefixar o token com `Authorization: Bearer ` (por exemplo, `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`) e salvá-lo em um arquivo. Em seguida, use o comando a seguir:

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## Atualizando um token
{: #refresh_token}

É possível usar o token de atualização do IAM que é gerado pelo método `POST identity/token` para estender a vida útil do token de acesso. O comando a seguir atualiza um token de acesso existente. No comando, `{refresh-token}` é o token de atualização do IAM que você gerou.

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Ao atualizar um token, use a mesma autenticação que você usou para criar o token original.
{: important}
