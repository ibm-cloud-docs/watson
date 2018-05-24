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

# Variável de ambiente VCAP\_SERVICES
{: #vcapServices}

A variável de ambiente `VCAP_SERVICES` contém informações que podem ser usadas para interagir com uma instância de serviço no {{site.data.keyword.Bluemix}}. Os campos nessa variável de ambiente são
configurados quando você liga um serviço a um aplicativo.
{: shortdesc}

Seu aplicativo precisa da URL e das credenciais de serviço de autenticação básica HTTP para autenticar
com um serviço do {{site.data.keyword.watson}}.

Um aplicativo em execução normalmente lê essas variáveis de ambiente, depois que um serviço é vinculado a ele, para extrair os
pares de nome-valor requeridos. Para obter informações sobre as variáveis, consulte
[Implementando aplicativos](/docs/manageapps/depapps.html#app_env) e procure por "Variáveis de ambiente".

## Exemplo
Esse exemplo mostra a variável de ambiente `VCAP_SERVICES` para o serviço do {{site.data.keyword.personalityinsightsshort}} ligado a um aplicativo:

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
| planejar     | O plano disponível para o serviço no {{site.data.keyword.cloud_notm}}
|
| identificações     | Informações adicionais sobre o serviço                                                     |

## Visualizando variáveis de ambiente
É possível recuperar informações sobre a variável da ferramenta Cloud Foundry ou de dentro da interface do {{site.data.keyword.cloud_notm}}.

- Com a ferramenta de linha de comandos do Cloud Foundry: use o comando `cf env` depois de ter
criado um serviço e ligá-lo ao seu aplicativo:

    ```bash
    cf env {application-name}
    ```
    {: pre}

- Na interface do {{site.data.keyword.cloud_notm}}: informações sobre variáveis de ambiente estão disponíveis
na página de resumo para um aplicativo.
