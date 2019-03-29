---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM tokens,IAM authentication

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

# 使用 IAM 記號進行鑑別
{: #iam}

請使用 {{site.data.keyword.cloud}} Identity and Access Management (IAM) 記號對 {{site.data.keyword.ibmwatson}} 服務提出已鑑別的要求，而不不要在每個呼叫中內嵌服務認證。IAM 鑑別會使用存取記號來進行鑑別，您可以透過傳送要求與 API 金鑰來獲得。
{: shortdesc}

在資源群組中建立或從 Cloud Foundry 移轉到資源群組中的 {{site.data.keyword.watson}} 服務，會使用 IAM 鑑別。依預設，所有新的 {{site.data.keyword.watson}} 服務都會使用 IAM 鑑別。
{: note}

## 取得 IAM 服務認證
{: #iam-getting-credentials-manually}

若要使用 IAM 服務認證存取 API 方法，您必須先收集認證。您可以從 {{site.data.keyword.cloud_notm}} Web 介面存取服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}){: new_window}。
1.  前往[資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/dashboard){: new_window}。
1.  選取服務實例。
1.  請按一下**顯示認證**，查看認證的 **API 金鑰**及 **URL**。
    1.  若要查看認證的記號，在頁面左側的導覽功能表中，選取**服務認證**。
    1.  選取**檢視認證**，以檢視認證的完整 API 金鑰及記號資訊。

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Auto generated apikey during resource-key operation for...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## 更新 IAM 服務認證
{: #update-existing-svcs}

您可以從服務儀表板更新現有服務實例的服務認證。

1.  登入 [{{site.data.keyword.cloud_notm}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}){: new_window}。
1.  前往[資源清單 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/dashboard){: new_window}。
1.  在頁面左側的導覽功能表中，選取**服務認證**。
1.  使用功能表及圖示以刪除現有的認證或新增認證。

務必更新應用程式中的認證以進行任何變更。

## 使用 Watson 服務 API 金鑰，以取得 IAM 記號
{: #iamtoken}

您可以使用為服務實例產生的 API 金鑰，來存取 {{site.data.keyword.ibmwatson_notm}} 服務 API。使用 API 金鑰以產生 IAM 存取金鑰。如果您正在開發需要與其他 {{site.data.keyword.cloud_notm}} 服務搭配使用的應用程式，您也可以使用此處理程序。

如需安全使用 API 金鑰的相關資訊，請參閱 [IAM 服務 API 金鑰](/docs/services/watson?topic=watson-api-key-bp)。
{: tip}

下列 curl 指令會使用 `POST identity/token` 方法，透過傳遞 API 金鑰來產生 IAM 存取記號。`Content-Type` 標頭指出資料是已編碼的 URL。`data-urlencode` 參數將 URL 編碼資料傳遞至方法。

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

使用鑑別以產生存取記號。當您重新整理記號時，請使用相同的鑑別。

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"

```
{: pre}

下列範例顯示預期的回應：

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: pre}

## 使用記號以進行鑑別
{: #use_token}

使用 `POST identity/token` 方法以產生 IAM 存取記號。然後，您可以使用記號進行已鑑別 API 呼叫。API 金鑰與特定的服務實例相關聯。使用 API 金鑰產生的記號只能用於對該服務實例的呼叫。

{{site.data.keyword.watson}} 服務的一般 curl 呼叫會使用下列格式，其中 `{token}` 是您產生的 IAM 存取記號：

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

或者，您可以在記號前面加上 `Authorization: Bearer `（例如 `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`），並將其儲存至檔案中。然後，使用下列指令：

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## 重新整理記號
{: #refresh_token}

您可以使用由 `POST identity/token` 方法產生的 IAM 重新整理記號，以延長存取記號的生命週期。下列指令會重新整理現有的存取記號。在指令中，`{refresh-token}` 是您產生的 IAM 重新整理記號。

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

當您重新整理記號時，請使用您用來建立原始記號的相同鑑別。
{: important}
