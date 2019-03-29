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

# IAM 服務 API 金鑰
{: #api-key-bp}

{{site.data.keyword.ibmwatson}} 服務 API 金鑰是用來取得載送記號，該記號則是用來鑑別對 {{site.data.keyword.ibmwatson_notm}} 服務的呼叫。
{: shortdesc}

與 API 金鑰的標準 {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) 實作不同，{{site.data.keyword.ibmwatson_notm}} IAM 金鑰會以每個服務為基準而設定。您可以為每個 API 金鑰指派權限，而非為每個服務指派權限。此實作與以使用者為基礎之 API 金鑰的標準 {{site.data.keyword.cloud_notm}} 方法不同。使用 {{site.data.keyword.ibmwatson_notm}}，會為使用者指派每個服務的角色。如需一般 {{site.data.keyword.cloud_notm}} IAM 實作的相關資訊，請參閱 [API 金鑰最佳作法](/docs/services/iam?topic=iam-iamoverview#iamoverview)。

您只能將 Watson API 金鑰連結至單一服務實例。
{: tip}

每次建立 {{site.data.keyword.ibmwatson_notm}} 服務實例時，都會自動產生`管理員`角色的認證集。您可以在服務實例的**儀表板**頁面上使用這些認證。您可以使用它們來呼叫服務 API 的*所有* 方法。您可以透過建立具有不同角色的 API 金鑰來限制在使用服務時執行的可能動作。

| 服務存取角色 | 動作說明 |
|:-----------------|:-----------------|
| `讀者` | 讀者可以在服務中執行唯讀動作，例如檢閱服務特有的資源。 |
| `撰寫者` | 撰寫者具有超出讀者角色的許可權，包括建立及編輯服務特有的資源。 |
| `管理員` | 管理員具有超出撰寫者角色的許可權，以完成服務所定義的特許動作。此外，他們還可以建立及編輯服務特有的資源。|
{: caption="表 1. 使用者的服務存取角色範例" caption-side="top"}

如需建立額外 {{site.data.keyword.ibmwatson_notm}} 服務認證的相關資訊，請參閱[更新 IAM 服務認證](/docs/services/watson?topic=watson-iam#update-existing-svcs)。

## API 金鑰最佳作法
{: #api-bp}

作為服務的驗證方法，您的 API 金鑰必須保持安全。下列最佳作法可協助您維護安全的 API 金鑰，並減少公開洩露危及應用程式認證的機會。

-   *請使用具有適合您正在使用功能之角色的 API 金鑰。*

    建立一般使用者應用程式時，請考慮針對 `GET` API 方法的所有呼叫，使用具有`讀者`角色的 API 金鑰。`讀者`角色不能對服務實例執行任何破壞性或新增功能。

-   *請不要直接在程式碼中內嵌 API 金鑰。*

    內嵌在程式碼中的 API 金鑰可能會公開揭露給您的使用者。應考慮將 API 金鑰儲存在環境變數中或原始碼控制系統之外的檔案中，而非將其內嵌在應用程式碼。


-   *請不要將 API 金鑰儲存在應用程式原始碼控制系統內的檔案中。*

    如果將 API 金鑰儲存在檔案中，請將檔案保留在應用程式原始碼之外。如果您使用公用原始碼管理系統（如 GitHub），這種作法尤其重要。

-   *請定期重新產生 API 金鑰。*

    您可以隨時建立新的認證。
    1.  從資源清單中，選取一個服務實例。
    1.  在頁面左側的導覽功能表中，按一下**服務認證**。

     將應用程式移轉至新認證時，請不要忘記使用您要刪除之認證的垃圾桶圖示來刪除舊認證。
     {: tip}
