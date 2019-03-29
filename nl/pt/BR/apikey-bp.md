---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM authentication,service keys,service roles,best practices

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

# Chaves de API de serviço do IAM
{: #api-key-bp}

As chaves da API de serviço do {{site.data.keyword.ibmwatson}} são usadas para adquirir tokens de acesso que são, por sua vez, usados para autenticar chamadas para os serviços do {{site.data.keyword.ibmwatson_notm}}.
{: shortdesc}

Diferentemente da implementação padrão das chaves de API do {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM), as chaves do {{site.data.keyword.ibmwatson_notm}} IAM são configuradas em uma base por serviço. É possível designar permissões para cada chave de API em vez de para cada serviço. Essa implementação difere do método padrão do {{site.data.keyword.cloud_notm}} de chaves de API baseadas em usuário. Com o {{site.data.keyword.ibmwatson_notm}}, uma função para cada serviço é designada aos usuários. Para obter mais informações sobre a implementação geral do {{site.data.keyword.cloud_notm}} IAM, consulte [Melhores práticas para chave de API](/docs/services/iam?topic=iam-iamoverview#iamoverview).

É possível ligar uma chave de API do Watson a apenas uma instância de serviço única.
{: tip}

Um conjunto de credenciais para a função `Gerenciador` é gerado automaticamente toda vez que uma instância de serviço do {{site.data.keyword.ibmwatson_notm}} é criada. Essas credenciais estão disponíveis na página **Painel** da instância de serviço. É possível usá-las para chamar *todos* os métodos da API do serviço. É possível limitar as possíveis ações executadas ao usar o serviço criando uma chave de API com uma função diferente.

| Função de Acesso de Serviço | Descrição das ações |
|:-----------------|:-----------------|
| `Reader` | Leitores podem executar ações somente leitura em um serviço, como visualizar recursos específicos do serviço. |
| `Writer` | Escritores têm permissões além da função leitor, incluindo a criação e a edição de recursos específicos do serviço. |
| `Manager` | Gerentes têm permissões além da função de escritor para concluir ações privilegiadas, conforme definido pelo serviço. Além disso, eles podem criar e editar recursos específicos do serviço. |
{: caption="Tabela 1. Exemplo de funções de acesso ao serviço para usuários" caption-side="top"}

Para obter mais informações sobre a criação de credenciais de serviço extras do {{site.data.keyword.ibmwatson_notm}}, consulte [Atualizando as credenciais de serviço do IAM](/docs/services/watson?topic=watson-iam#update-existing-svcs).

## Melhores práticas da chave de API
{: #api-bp}

Como as suas chaves de API são o método de autenticação para o seu serviço, elas devem ser mantidas em segurança. As melhores práticas a seguir ajudam a manter uma chave de API segura e a reduzir a chance de expor publicamente as credenciais que comprometem o seu aplicativo.

-   *Use uma chave de API com a função apropriada para a função que você está usando.*

    Ao criar um aplicativo de usuário final, considere usar uma chave de API com a função `Reader` para todas as chamadas para os métodos de API `GET`. A função `Reader` não pode executar nenhuma função destrutiva ou aditiva para a instância de serviço.

-   *Não integre a chave de API diretamente no código.*

    As chaves de API integradas no código podem ser expostas a seus usuários. Em vez de integrar as chaves de API em seu código do aplicativo, considere armazená-las em variáveis de ambiente ou em arquivos fora do sistema de controle de código-fonte.

-   *Não armazene uma chave de API em arquivos dentro do sistema de controle de código-fonte do aplicativo.*

    Se você armazenar as chaves de API em arquivos, mantenha esses arquivos fora do código-fonte do aplicativo. Essa prática será particularmente importante se você usar um sistema de gerenciamento de código-fonte público, como o GitHub.

-   *Gere novamente as suas chaves de API periodicamente.*

    É possível criar novas credenciais a qualquer momento.
    1.  Na lista de recursos, selecione uma instância de serviço.
    1.  Clique em **Credenciais de serviço** no menu de navegação à esquerda da página.

     Ao migrar seu aplicativo para as novas credenciais, não se esqueça de excluir as credenciais antigas usando o ícone de lixeira para as credenciais que você deseja excluir.
     {: tip}
