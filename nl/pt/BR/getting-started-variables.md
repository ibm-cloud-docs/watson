---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: environment variable,service alias,VCAP services

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

# Variável de ambiente VCAP\_SERVICES
{: #vcapServices}

A variável de ambiente `VCAP_SERVICES` contém informações que podem ser usadas para interagir com uma instância de serviço no {{site.data.keyword.cloud}}. Os campos nessa variável de ambiente são configurados quando você liga uma instância de serviço a um aplicativo.
{: shortdesc}

Seu aplicativo precisa da URL e das credenciais de serviço para autenticar com um serviço do {{site.data.keyword.watson}}. Um aplicativo em execução geralmente lê essas variáveis de ambiente após um serviço ser ligado a ele para extrair os pares nome-valor necessários. É possível ligar os serviços de grupo de recursos ou do Cloud Foundry ao aplicativo, mas os serviços de grupo de recursos precisam de um alias de serviço.

## Aliases de serviço para serviços do IAM
{: #vcapIAMAlias}

É possível usar a variável de ambiente `VCAP_SERVICES` com serviços de grupo de recursos, fornecendo ao serviço um alias. Por exemplo, use o comando a seguir com a interface da linha de comandos do {{site.data.keyword.cloud_notm}} para criar um alias chamado `personality-insights-service` para um serviço chamado `my-personality-insights-service`.

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

É possível, então, ligar seu aplicativo ao alias de serviço. Um aplicativo em execução geralmente lê essas variáveis de ambiente após uma instância de serviço ser ligada a ele para extrair os pares nome-valor necessários.

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Exemplo
{: #gs-variables-vcapIAMAliasExample}

Este exemplo mostra a variável de ambiente `VCAP_SERVICES` para o serviço {{site.data.keyword.personalityinsightsshort}} que está ligado a um aplicativo. A instância de serviço usa a autenticação do Cloud Foundry.

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

A tabela a seguir descreve o conteúdo da variável de ambiente `VCAP_SERVICES`.

| Item     | Descrição                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | A senha das credenciais de autenticação básica HTTP utilizada para se conectar ao serviço |
| URL      | A URL de conexão para o serviço                                                         |
| username | O nome do usuário das credenciais de autenticação básica HTTP usadas para se conectar ao serviço |
| rótulo    | O nome associado ao serviço no {{site.data.keyword.cloud_notm}}                                            |
| nome     | O nome da instância de serviço                                                           |
| planejar     | O plano disponível para o serviço no {{site.data.keyword.cloud_notm}}                                              |
| identificações     | Informações adicionais sobre o serviço                                                   |

## Visualizando variáveis de ambiente
{: #gs-variables-vcapView}

É possível recuperar informações sobre a variável por meio da interface da linha de comandos (CLI) do {{site.data.keyword.cloud_notm}} ou da interface da web.

- Com a CLI do {{site.data.keyword.cloud_notm}}: chame o comando `env` do Cloud Foundry após você criar um serviço e ligá-lo ao seu aplicativo:

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- Na interface do {{site.data.keyword.cloud_notm}}: informações sobre variáveis de ambiente estão disponíveis
na página de resumo para um aplicativo.
