---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# Autenticando com as credenciais de serviço do Cloud Foundry
{: #creating-credentials}

As credenciais de serviço do Cloud Foundry para os serviços do {{site.data.keyword.ibmwatson}} fornecem autenticação para um serviço no {{site.data.keyword.cloud}}. As credenciais de serviço usam a autenticação básica HTTP para se autenticar em um serviço específico do {{site.data.keyword.watson}}.
{: shortdesc}

Os serviços do {{site.data.keyword.watson}} que são criados em um grupo de recursos ou migrados do Cloud Foundry para um grupo de recursos usam a autenticação do IAM. Por padrão, todos os novos serviços do {{site.data.keyword.watson}} usam a autenticação do IAM. Para obter mais informações sobre grupos de recursos e autenticação do IAM, consulte [Autenticando com tokens do IAM](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually).
{: note}

## Obtendo as credenciais de serviço do Cloud Foundry
{: #getting-credentials-manually}

Para acessar os métodos de API usando as credenciais de serviço do Cloud Foundry, deve-se primeiro coletar as credenciais. É possível acessar as credenciais de serviço por meio da interface da web do {{site.data.keyword.cloud_notm}}.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
1.  Acesse a [lista de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard){: new_window}.
1.  Selecione uma instância de serviço.
1.  Clique em **Mostrar credenciais** para ver o **Nome do usuário**, a **Senha** e a **URL** para as credenciais.

## Atualizando as credenciais de serviço do Cloud Foundry
{: #existing-svcs}

É possível atualizar as credenciais de serviço para uma instância de serviço existente no painel de serviço.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}){: new_window}.
1.  Acesse a [lista de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/dashboard){: new_window}.
1.  Selecione **Credenciais de serviço** no menu de navegação à esquerda da página.
1.  Use os menus e ícones para excluir as credenciais existentes ou para incluir novas credenciais.

Certifique-se de atualizar as credenciais em seus aplicativos para qualquer mudança.

## Credenciais no {{site.data.keyword.Bluemix_dedicated_notm}}

Em uma instância do
[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated),
os quadros de serviço são implementados para que apenas um quadro seja permitido por serviço em uma organização.

Siga essas etapas para se referir a um serviço em sua própria área de
trabalho do {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Crie um conjunto de credenciais para o quadro de referência para o serviço que você deseja usar.
1.  Crie um serviço fornecido pelo usuário customizado que use essas credenciais:

    1.  Use a interface da web para selecionar o bloco para o serviço que deseja usar e crie um conjunto de credenciais para o serviço. As credenciais são criadas no formato JSON e incluem a URL do terminal em serviço, o nome do usuário e a senha.
    1.  Use o comando `ibmcloud cf cups` (criar serviço fornecido pelo usuário) para criar um serviço customizado que esteja ligado a essas credenciais. Por exemplo,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api", "nome do usuário": "583b2e88-c80d-4ec0-93c1-98239f805146", "senha": "RuytRliRvoFN" }
      ```
      {: codeblock}

      Dadas essas credenciais, crie uma instância de serviço customizada fornecida pelo usuário chamada `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Certifique-se de especificar o nome do usuário e senha como uma única sequência entre aspas duplas e separados por
uma vírgula. Além disso, coloque as duas sequências, de nome de usuário e de senha, entre aspas duplas e escape-as com uma barra invertida.
      {: tip}

### Usando aplicativos com o {{site.data.keyword.Bluemix_dedicated_notm}}

Alguns aplicativos de amostra e kits do iniciador do {{site.data.keyword.watson}} no GitHub incluem um botão
**Implementar no {{site.data.keyword.cloud_notm}}**. Esses botões não funcionam em instalações do {{site.data.keyword.Bluemix_dedicated_notm}} porque se referem ao ambiente público do {{site.data.keyword.cloud_notm}}.

Para usar aplicativos de amostra e kits do iniciador em ambientes do {{site.data.keyword.Bluemix_dedicated_notm}}, siga estas etapas:

1.  Nas instruções para o aplicativo, certifique-se de que o arquivo `manifest.yml` que você cria use o nome do serviço criado com o comando `ibmcloud cf cups`.
1.  Nessas mesmas instruções, em vez de executar o comando `ibmcloud cf create-service`, use um comando `ibmcloud cf cups`. No entanto, se você criar uma instância fornecida pelo usuário do seu serviço, não precisará criá-la novamente para usá-la com o aplicativo de amostra ou kit do iniciador.
