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

# 使用 Cloud Foundry 服務認證進行鑑別
{: #creating-credentials}

{{site.data.keyword.ibmwatson}} 服務的 Cloud Foundry 服務認證，提供對於 {{site.data.keyword.cloud}} 中服務的鑑別。服務認證使用 HTTP 基本鑑別，對特定 {{site.data.keyword.watson}} 服務進行鑑別。
{: shortdesc}

在資源群組中建立或從 Cloud Foundry 移轉到資源群組中的 {{site.data.keyword.watson}} 服務，會使用 IAM 鑑別。依預設，所有新的 {{site.data.keyword.watson}} 服務都使用 IAM 鑑別。如需資源群組及 IAM 鑑別的相關資訊，請參閱[使用 IAM 記號進行鑑別](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually)。
{: note}

## 取得 Cloud Foundry 服務認證
{: #getting-credentials-manually}

若要使用 Cloud Foundry 服務認證存取 API 方法，您必須先收集認證。您可以從 {{site.data.keyword.cloud_notm}} Web 介面存取服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}){: new_window}。
1.  前往[資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/dashboard){: new_window}。
1.  選取服務實例。
1.  請按一下**顯示認證**，查看認證的**使用者名稱**、**密碼** 及 **URL**。

## 更新 Cloud Foundry 服務認證
{: #existing-svcs}

您可以從服務儀表板更新現有服務實例的服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}){: new_window}。
1.  前往[資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/dashboard){: new_window}。
1.  在頁面左側的導覽功能表中，選取**服務認證**。
1.  使用功能表及圖示以刪除現有的認證或新增認證。

務必更新應用程式中的認證以進行任何變更。

## {{site.data.keyword.Bluemix_dedicated_notm}} 中的認證

在 [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated) 實例中，實作了服務磚，因此，組織中的每個服務只允許一個磚。

請遵循下列步驟，在您自己的 {{site.data.keyword.Bluemix_dedicated_notm}} 工作區中參照服務：

1.  為您要使用的服務建立一組參照磚的認證。
1.  建立使用那些認證的自訂使用者提供的服務：

    1.  使用 Web 介面來選取您想要使用的服務磚，然後為服務建立一組認證。認證會以 JSON 格式建立，並包括服務端點 URL、使用者名稱及密碼。
    1.  使用 `ibmcloud cf cups`（建立使用者提供的服務）指令來建立連結至那些認證的自訂服務。例如，

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      根據這些認證，建立名為 `Personality-Insights-Std` 的自訂使用者提供服務實例：

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      請務必將使用者名稱和密碼指定為雙引號內的單一字串，並以冒號區隔。同時，使用雙引號括住使用者名稱和密碼字串，並以反斜線跳出。
      {: tip}

### 使用應用程式搭配 {{site.data.keyword.Bluemix_dedicated_notm}}

GitHub 上的部分 {{site.data.keyword.watson}} 範例應用程式和入門範本套件包含了一個**部署至 {{site.data.keyword.cloud_notm}}** 按鈕。這些按鈕在 {{site.data.keyword.Bluemix_dedicated_notm}} 安裝中沒有作用，因為它們是參照公用 {{site.data.keyword.cloud_notm}} 環境。

若要在 {{site.data.keyword.Bluemix_dedicated_notm}} 環境中使用範例應用程式及入門範本套件，請遵循下列步驟：

1.  在應用程式的指示中，確定您建立的 `manifest.yml` 檔，會使用您以 `ibmcloud cf cups` 指令建立之服務的名稱。
1.  在同樣的那些指示中，請不要執行 `ibmcloud cf create-service` 指令，而是使用 `ibmcloud cf cups` 指令。不過，如果您已建立服務的自訂使用者提供實例，則不需要重新建立它，即可搭配使用範例應用程式或入門範本套件。
