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

# Cloud Foundry 指令行介面

您可以使用 `cf` 指令行介面 (CLI) 工具，在本端系統上處理 {{site.data.keyword.cloud}} 應用程式。此工具提供完整開發體驗的豐富介面。
{: shortdesc}

如需使用 Cloud Foundry 工具來開發應用程式的相關資訊，請參閱 [Cloud Foundry 如何與 {{site.data.keyword.cloud_notm}} 搭配運作](/docs/overview/cf.html)和 [Cloud Foundry 文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://docs.cloudfoundry.org/){: new_window}。

## 安裝 cf 指令行介面

您可以在 [Cloud Foundry GitHub 儲存庫 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://github.com/cloudfoundry/cli#downloads){: new_window} 的 **Downloads** 區段中，找到工具的最新版本。您會看到兩種類型的套件：

- **安裝程式**會與平台的套件管理工具或已安裝軟體追蹤系統整合，以將該工具安裝到其「官方」位置。
- **二進位檔**會提供可安裝至任何位置的保存檔。

安裝工具之後，請務必在 `PATH` 環境變數上包含指令。
{: tip}

## 服務實例的指令

在 {{site.data.keyword.cloud_notm}} 中管理現有 {{site.data.keyword.watson}} 服務實例的實用 `cf` 指令：

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services)：列出現行空間中的服務實例。

  ```bash
  cf services
  ```
  {: pre}

  如果您知道服務的名稱，請使用 `cf service` 指令來查看其相關資訊。例如，

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service)：建立服務的實例，您可以將它連結至 {{site.data.keyword.cloud_notm}} 中執行的應用程式：

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - 對於 *{service-name}* 和 *{service-plan}* 引數，請使用 `cf marketplace` 指令的輸出。
    - 指定您要建立的 {service-instance-name}。對於許多應用程式，會在 `manifest.yml` 或應用程式的其他配置檔中指定名稱。

- **cf delete-service**：刪除服務實例：

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    藉由刪除不再需要的服務，您可以在 {{site.data.keyword.cloud_notm}} 中釋放不需要的資源。

## 用來部署和執行應用程式的指令

用來在 {{site.data.keyword.cloud_notm}} 中部署及執行應用程式的常見 `cf` 指令：

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api)：指定 {{site.data.keyword.watson}} API 的位置：

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login)：使用您的 {{site.data.keyword.ibmid}} 及密碼登入 {{site.data.keyword.cloud_notm}}：

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace)：列出 {{site.data.keyword.cloud_notm}} 型錄中您可以連接的服務：

  ```bash
  cf marketplace
  ```
  {: pre}

  輸出包括每個服務的名稱以及提供服務所用的方案清單。當您為在 {{site.data.keyword.cloud_notm}} 中執行的應用程式建立服務實例時，會需要此資訊。

  如果您知道服務的名稱，請使用 `-s` 選項。例如，

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push)：將應用程式部署或更新至 {{site.data.keyword.cloud_notm}}。輸出包括用來存取應用程式的 URL。

  ```bash
  cf push {application-name}
  ```
  {: pre}

  這個指令通常會使用 `manifest.yml` 或其他配置檔來尋找要推送的檔案（例如，Java 用的 `.war` 檔，或 Node.js 用的 `.zip` 檔）。不過，您可以使用 `-p` 選項來指定應用程式檔案的路徑名稱。

## 現有應用程式的指令

在 {{site.data.keyword.cloud_notm}} 中管理現有應用程式的實用 `cf` 指令：

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps)：列出部署在現行空間的應用程式。

  ```bash
  cf apps
  ```
  {: pre}

  包含應用程式名稱，以查看應用程式之性能與狀態的相關詳細資訊：

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**：重新載入並重新啟動 {{site.data.keyword.cloud_notm}} 中的應用程式：

  ```bash
  cf restage {application-name}
  ```
  {: pre}

這個指令會重新啟動應用程式，因為它存在於 {{site.data.keyword.cloud_notm}} 中。若要在重新啟動應用程式之前，將應用程式的最新版本上傳至 {{site.data.keyword.cloud_notm}}，請改為使用 `cf push` 指令。

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete)：刪除應用程式：

  ```bash
  cf delete application-name
  ```
  {: pre}

  藉由刪除應用程式，您可以釋放 {{site.data.keyword.cloud_notm}} 記憶體和配額，供其他應用程式使用。

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service)：將服務連結至應用程式。

  通常，您會使用 `cf push` 指令，將服務實例連結至使用該服務的應用程式。您可以使用 `cf bind-service` 指令，將現有服務實例連結至現有的應用程式：

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  然後發出 `cf restage` 或 `cf push`，以重新啟動應用程式。

- **cf unbind-service**：移除應用程式與服務之間的關聯：

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  您可能需要發出 `cf restage` 或 `cf push` 指令來反映變更。

  這個指令會移除 `VCAP_SERVICES` 環境變數中的鑑別認證。請使用 `cf delete-service` 或 `cf delete` 來移除服務實例或應用程式。
  {: tip}
