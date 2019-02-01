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

# Watson 服務的服務認證

{{site.data.keyword.ibmwatson}} 服務的服務認證提供對 {{site.data.keyword.cloud}} 中服務的鑑別。一組認證只能與它們建立時的目標服務搭配使用。
{: shortdesc}

服務認證的使用會視您使用的程式設計模型而不同。如需詳細資料，請參閱 [{{site.data.keyword.watson}} 服務的程式設計模型](/docs/services/watson/getting-started-develop.html)。

服務認證是特定 {{site.data.keyword.watson}} 服務的 HTTP 基本鑑別認證。

## 手動取得認證
{: #getting-credentials-manually}

您可以從 {{site.data.keyword.cloud_notm}} Web 介面找到服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}。
1.  移至現有的服務。（[帶我過去 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/developer/watson/existing-services){: new_window}）：
    1.  選取左上方功能表，然後選取 **{{site.data.keyword.watson}}** 以前往 {{site.data.keyword.watson}} 主控台。
    1.  從 **Watson Services** 中選取 **Existing Services**，以檢視您的服務和專案清單。
1.  檢視認證：
    - 如果服務是專案的一部分，請按一下包含服務的專案名稱。從專案詳細資料頁面上的**認證**區段，檢視認證。
    - 如果服務不是專案的一部分，請按一下您要檢視的服務名稱，然後選取**服務認證**。

## 更新服務認證
{: #existing-svcs}

您可以從服務儀表板更新現有服務實例的服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}。
1.  移至現有的服務。（[帶我過去 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/developer/watson/existing-services){: new_window}）：
    1.  選取左上方功能表，然後選取 **{{site.data.keyword.watson}}** 以前往 {{site.data.keyword.watson}} 主控台。
    1.  從 **Watson Services** 中選取 **Existing Services**，以檢視您的服務清單。
1.  選取您要檢視或更新的服務，然後選取**服務認證**。
1.  刪除或新增認證：
    - 若要刪除認證，請選取認證，然後選取**刪除認證**。
    - 若要新增認證，請按一下**新建認證**，然後在「新增認證」視窗中，按一下**新增**。請檢視並複製新的認證。

請確定您在應用程式中使用新的認證：

- 如果您的應用程式是以程式化方式取得其認證，請停止並重新啟動應用程式。
- 如果您的認證內嵌在應用程式碼中，請更新程式碼並重新啟動應用程式。

如需如何以新認證來更新應用程式，而又能有最少關閉時間的詳細資料，請參閱[更新應用程式](/docs/manageapps/updapps.html)。

## {{site.data.keyword.Bluemix_dedicated_notm}} 中的認證

在 [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated) 實例中，實作了服務磚，因此，組織中的每個服務只允許一個磚。

請遵循下列步驟，在您自己的 {{site.data.keyword.Bluemix_dedicated_notm}} 工作區中參照服務：

1.  為您要使用的服務建立一組參照磚的認證。
1.  建立使用那些認證的自訂使用者提供的服務：

    1.  使用 {{site.data.keyword.cloud_notm}} 介面來選取您要使用之服務的磚，然後為服務建立一組認證。認證是以 JSON 格式建立，且包含要與服務通訊之 URL 的個別欄位，以及要用於該服務的使用者名稱和密碼的個別欄位。
    1.  使用 `cf cups`（建立使用者提供的服務）指令來建立連結至那些認證的自訂服務。例如，假設這些認證，

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      建立名為 `Personality-Insights-Sid` 的自訂使用者提供服務實例：

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      請務必將使用者名稱和密碼指定為雙引號內的單一字串，並以冒號區隔。同時使用雙引號括住使用者名稱和密碼字串，並以反斜線跳出。
      {: tip}

### 使用應用程式搭配 {{site.data.keyword.Bluemix_dedicated_notm}}

GitHub 上的部分 {{site.data.keyword.watson}} 範例應用程式和入門範本套件包含了一個**部署至 {{site.data.keyword.cloud_notm}}** 按鈕。這些按鈕在 {{site.data.keyword.Bluemix_dedicated_notm}} 安裝中不會起作用，因為它們是參照公用 {{site.data.keyword.cloud_notm}} 環境。

若要在 {{site.data.keyword.Bluemix_dedicated_notm}} 環境中使用範例應用程式和入門範本，請執行下列動作：

1.  在應用程式的指示中，確定您建立的 `manifest.yml` 檔使用您以 `cf cups` 指令所建立之服務的名稱。
1.  在同樣的那些指示中，請不要執行 `cf create-service` 指令，而是使用 `cf cups` 指令。不過，如果您已建立服務的自訂使用者提供實例，則不需要重新建立它，即可搭配使用範例應用程式或入門範本套件。
