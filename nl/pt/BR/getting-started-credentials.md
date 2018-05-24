---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Credenciais de serviço para serviços do Watson

As credenciais de serviço para um serviço do {{site.data.keyword.ibmwatson}} fornecem autenticação para um serviço em
{{site.data.keyword.cloud}}. Um conjunto de credenciais pode ser usado apenas com o serviço para o qual elas são criadas.
{: shortdesc}

Use as credenciais de serviço de forma diferente dependendo do modelo de programação que você usar. Para obter detalhes,
consulte [Modelos de programação para serviços do {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

As credenciais de serviço são credenciais de autenticação básica HTTP para um serviço
específico do {{site.data.keyword.watson}}.

## Obtendo credenciais manualmente
{: #getting-credentials-manually}

É possível localizar as credenciais de serviço na interface da web do {{site.data.keyword.cloud_notm}}.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Acesse os serviços existentes. ([Leve-me lá ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Selecione o menu superior esquerdo e, em seguida, selecione **{{site.data.keyword.watson}}** para obter o console do {{site.data.keyword.watson}}.
    1.  Selecione **Serviços existentes** em **Serviços do Watson** para
visualizar uma lista de seus serviços e projetos.
1.  Visualize as credenciais:
    - Se o serviço fizer parte de um projeto, clique no nome do projeto que contém o serviço. Visualize as credenciais na
seção **Credenciais** na página de detalhes do projeto.
    - Se o serviço não fizer parte de um projeto, clique no nome do serviço que você deseja visualizar e selecione
**Credenciais de serviço**.

## Atualizando as credenciais de serviço
{: #existing-svcs}

É possível atualizar as credenciais de serviço para uma instância de serviço existente no painel de serviço.

1.  Efetue login no [{{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Acesse os serviços existentes. ([Leve-me lá ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Selecione o menu superior esquerdo e, em seguida, selecione **{{site.data.keyword.watson}}** para obter o console do {{site.data.keyword.watson}}.
    1.  Selecione **Serviços existentes** em **Serviços do Watson** para
visualizar uma lista de seus serviços.
1.  Selecione o serviço que você deseja visualizar ou atualizar e, em seguida, selecione **Credenciais
de serviço**.
1.  Exclua ou inclua as credenciais:
    - Para excluir as credenciais, selecione-as e, em seguida, **Excluir credencial**.
    - Para incluir as credenciais, clique em **Nova credencial** e, depois, **Incluir**
na janela "Incluir nova credencial". Visualize e copie as novas credenciais.

Certifique-se de usar as novas credenciais em seus aplicativos:

- Pare e reinicie o aplicativo se ele obtiver suas credenciais programaticamente.
- Se as credenciais forem integradas em seu código do aplicativo, atualize o código e ative novamente o aplicativo.

Para obter detalhes sobre como atualizar um aplicativo com novas credenciais com tempo de inatividade mínimo, consulte
[Atualizando aplicativos](/docs/manageapps/updapps.html).

## Credenciais no {{site.data.keyword.Bluemix_dedicated_notm}}

Em uma instância do
[{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated),
os quadros de serviço são implementados para que apenas um quadro seja permitido por serviço em uma organização.

Siga essas etapas para se referir a um serviço em sua própria área de
trabalho do {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Crie um conjunto de credenciais para o quadro de referência para o serviço que você deseja usar.
1.  Crie um serviço fornecido pelo usuário customizado que use essas credenciais:

    1.  Use a interface do {{site.data.keyword.cloud_notm}} para selecionar o quadro para o serviço que
você deseja usar e crie um conjunto de credenciais para o serviço. As credenciais são criadas no formato JSON e contêm campos
separados para a URL na qual se comunicar com o serviço e para o nome do usuário e a senha para usar com esse serviço.
    1.  Use o comando `cf cups` (criar o serviço fornecido pelo usuário) para criar um serviço
customizado que esteja ligado a essas credenciais. Por exemplo, dadas essas credenciais,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api", "nome do usuário": "583b2e88-c80d-4ec0-93c1-98239f805146", "senha": "RuytRliRvoFN" }
      ```
      {: codeblock}

      Criar uma instância de serviço fornecida pelo usuário customizada denominada
`Personality-Insights-Std`:

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Certifique-se de especificar o nome do usuário e senha como uma única sequência entre aspas duplas e separados por
uma vírgula. Também coloque as sequências de nome do usuário e senha entre aspas duplas e escape-as com uma barra invertida.
      {: tip}

### Usando aplicativos com o {{site.data.keyword.Bluemix_dedicated_notm}}

Alguns aplicativos de amostra e kits do iniciador do {{site.data.keyword.watson}} no GitHub incluem um botão
**Implementar no {{site.data.keyword.cloud_notm}}**. Esses botões não funcionam em
instalações do {{site.data.keyword.Bluemix_dedicated_notm}} porque eles se referem ao ambiente público
do {{site.data.keyword.cloud_notm}}.

Para usar aplicativos de amostra e kits do iniciador em ambientes do {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Nas instruções para o aplicativo, certifique-se de que o arquivo `manifest.yml` que você criar usará o nome
do serviço que você criou com o comando `cf cups`.
1.  Nessas mesmas instruções, em vez de executar o comando `cf create-service`, use um comando `cf
cups`. No entanto, se você criar uma instância fornecida pelo usuário do seu serviço, não precisará criá-la novamente para usá-la com o aplicativo de amostra ou kit do iniciador.
