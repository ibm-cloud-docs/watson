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

# Interface da linha de comandos do Cloud Foundry

É possível usar a ferramenta da interface da linha de comandos (CLI) `cf` para trabalhar com aplicativos {{site.data.keyword.cloud}} em seu sistema local. A ferramenta fornece uma interface rica para uma experiência de desenvolvimento completa.
{: shortdesc}

Para obter mais informações sobre o trabalho com ferramentas do Cloud Foundry para desenvolvimento de aplicativos, consulte [Como o Cloud Foundry funciona com o {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) e a [Documentação do Cloud Foundry ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.cloudfoundry.org/){: new_window}.

## Instalando a interface da linha de comandos cf

É possível localizar a versão mais recente da ferramenta na seção **Downloads** do [repositório GitHub do Cloud Foundry ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Você verá dois tipos de pacotes:

- **Instaladores** se integram com a ferramenta de gerenciamento de pacote ou o sistema de rastreamento de software instalado da sua plataforma para instalar a ferramenta em seu local "oficial".
- **Binários** fornecem archives que podem ser instalados em qualquer local.

Certifique-se de incluir o comando em sua variável de ambiente `PATH` depois de instalar a ferramenta.
{: tip}

## Comandos para instâncias de serviço

Comandos `cf` úteis para gerenciar instâncias de serviço existentes do {{site.data.keyword.watson}} no {{site.data.keyword.cloud_notm}}:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services) Listas de instâncias de serviço no espaço atual.

  ```bash
  cf services
  ```
  {: pre}

  Se você souber o nome do serviço, use o comando `cf service` para ver informações sobre ele. Por exemplo,

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): crie uma instância do serviço que você pode ligar ao seu aplicativo que é executado no {{site.data.keyword.cloud_notm}}:

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - Para argumentos *{service-name}* e *{service-plan}*, use a saída do comando `cf marketplace`.
    - Especifique o {service-instance-name} que deseja criar. Para muitos aplicativos, o nome é especificado no `manifest.yml` ou em outro arquivo de configuração para o aplicativo.

- **cf delete-service**: excluir uma instância de serviço:

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    Ao excluir um serviço que não é mais necessário, você libera recursos desnecessários no {{site.data.keyword.cloud_notm}}.

## Comandos para implementar e executar aplicativos

Comandos `cf` comuns para implementar e executar aplicativos no {{site.data.keyword.cloud_notm}}:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): especificar o local das APIs do {{site.data.keyword.watson}}:

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login). Efetue login no {{site.data.keyword.cloud_notm}} com seu {{site.data.keyword.ibmid}} e sua senha:

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): listar os serviços no catálogo do {{site.data.keyword.cloud_notm}} aos quais é possível se conectar:

  ```bash
  cf marketplace
  ```
  {: pre}

  A saída inclui o nome de cada serviço e a lista de planos sob os quais o serviço está disponível. Você precisa dessas informações ao criar uma instância do serviço para o seu aplicativo em execução no {{site.data.keyword.cloud_notm}}.

  Se você souber o nome do serviço, use a opção `-s`. Por exemplo,

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): implementar ou atualizar um aplicativo no {{site.data.keyword.cloud_notm}}. A saída inclui a URL para acessar o seu aplicativo.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  O comando finds geralmente usa o `manifest.yml` ou outro arquivo de configuração para localizar o arquivo a ser enviado por push (por exemplo, um arquivo `.war` para Java ou um arquivo `.zip` para Node.js). No entanto, é possível especificar o nome do caminho do arquivo de aplicativo usando a opção `-p`.

## Comandos para aplicativos existentes

Comandos `cf` úteis para gerenciar aplicativos existentes no {{site.data.keyword.cloud_notm}}:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps): listar os aplicativos que são implementados no espaço atual.

  ```bash
  cf apps
  ```
  {: pre}

  Inclua o nome do aplicativo para ver informações detalhadas sobre o funcionamento e o status de um aplicativo:

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: recarregar e reiniciar seu aplicativo no {{site.data.keyword.cloud_notm}}:

  ```bash
  cf restage {application-name}
  ```
  {: pre}

Esse comando reinicia o aplicativo, pois ele existe no {{site.data.keyword.cloud_notm}}. Para fazer upload da versão mais recente do aplicativo para {{site.data.keyword.cloud_notm}} antes de reiniciá-lo, use o comando `cf push` no lugar.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete): excluir um aplicativo:

  ```bash
  cf delete application-name
  ```
  {: pre}

  Ao excluir um aplicativo, você libera memória e cota do {{site.data.keyword.cloud_notm}} para uso de outros aplicativos.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service): liga um serviço a um aplicativo.

  Geralmente, você usa o comando `cf push` para ligar uma instância de um serviço a um aplicativo que o utilize. É possível ligar uma instância de serviço existente a um aplicativo existente com o comando `cf bind-service`:

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Então, emita `cf restage` ou `cf push` para reiniciar o aplicativo.

- **cf unbind-service**: remover a associação entre um aplicativo e o serviço:

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Talvez seja necessário emitir o comando `cf restage` ou `cf push` para refletir a mudança.

  Esse comando remove as credenciais de autenticação da variável de ambiente `VCAP_SERVICES`. Use `cf delete-service` ou `cf delete` para remover a instância de serviço ou o aplicativo.
  {: tip}
