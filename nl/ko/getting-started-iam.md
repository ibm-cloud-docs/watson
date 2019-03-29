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

# IAM 토큰을 사용한 인증
{: #iam}

{{site.data.keyword.cloud}} Identity and Access Management(IAM) 토큰을 사용하여, 모든 호출에 서비스 인증 정보를 임베드하지 않고도 {{site.data.keyword.ibmwatson}} 서비스에 대한 인증된 요청을 전송할 수 있습니다. IAM 인증은 인증에 액세스 토큰을 사용하며, 이는 API 키를 포함하는 요청을 전송하여 획득합니다.
{: shortdesc}

리소스 그룹에서 작성되거나 Cloud Foundry에서 리소스 그룹으로 마이그레이션된 {{site.data.keyword.watson}} 서비스는 IAM 인증을 사용합니다. 기본적으로 모든 새 {{site.data.keyword.watson}} 서비스는 IAM 인증을 사용합니다.
{: note}

## IAM 서비스 인증 정보 얻기
{: #iam-getting-credentials-manually}

IAM 서비스 인증 정보를 사용하여 API 메소드에 액세스하려면 먼저 인증 정보를 수집해야 합니다. 서비스 인증 정보는 {{site.data.keyword.cloud_notm}} 웹 인터페이스에서 액세스할 수 있습니다. 

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}){: new_window}에 로그인하십시오. 
1.  [리소스 목록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/dashboard){: new_window}으로 이동하십시오. 
1.  서비스 인스턴스를 선택하십시오. 
1.  **인증 정보 표시**를 클릭하여 인증 정보의 **API 키** 및 **URL**을 보십시오. 
    1.  인증 정보의 토큰을 보려면 페이지 왼쪽의 탐색 메뉴에서 **서비스 인증 정보**를 선택하십시오. 
    1.  **인증 정보 보기**를 선택하여 인증 정보의 전체 API 키 및 토큰 정보를 보십시오. 

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

## IAM 서비스 인증 정보 업데이트
{: #update-existing-svcs}

서비스 대시보드에서 기존 서비스 인스턴스에 대한 서비스 인증 정보를 업데이트할 수 있습니다.

1.  [{{site.data.keyword.cloud_notm}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}){: new_window}에 로그인하십시오. 
1.  [리소스 목록 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/dashboard){: new_window}으로 이동하십시오. 
1.  페이지 왼쪽의 탐색 메뉴에서 **서비스 인증 정보**를 선택하십시오. 
1.  메뉴 및 아이콘을 사용하여 기존 인증 정보를 삭제하거나 새 인증 정보를 추가하십시오. 

변경 사항이 있는 경우에는 이에 맞춰 애플리케이션의 인증 정보를 반드시 업데이트하십시오. 

## Watson 서비스 API 키를 사용하여 IAM 토큰 얻기
{: #iamtoken}

서비스 인스턴스에 대해 생성된 API 키를 사용하여 {{site.data.keyword.ibmwatson_notm}} 서비스 API에 액세스할 수 있습니다. 이 API 키를 사용하면 IAM 액세스 토큰을 생성할 수 있습니다. 다른 {{site.data.keyword.cloud_notm}} 서비스와 함께 작동해야 하는 애플리케이션을 개발하는 경우에도 이 프로세스를 사용할 수 있습니다. 

API 키를 안전하게 사용하는 데 대한 자세한 정보는 [IAM 서비스 API 키](/docs/services/watson?topic=watson-api-key-bp)를 참조하십시오.
{: tip}

다음 curl 명령은 `POST identity/token` 메소드를 사용하여, API 키를 전달해 IAM 액세스 토큰을 생성합니다. `Content-Type` 헤더는 데이터가 URL 인코딩되었음을 나타냅니다. `data-urlencode` 매개변수는 URL 인코딩된 데이터를 메소드에 전달합니다. 

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

인증을 사용하여 액세스 토큰을 생성하십시오. 해당 토큰을 새로 고칠 때는 동일한 인증을 사용하십시오. 

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

다음 샘플은 예상된 응답을 보여줍니다. 

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

## 토큰을 사용한 인증
{: #use_token}

위에서는 `POST identity/token` 메소드를 사용하여 IAM 액세스 토큰을 생성했습니다. 사용자는 이 토큰을 사용하여 인증된 API 호출을 수행할 수 있습니다. 특정 API 키는 특정 서비스 인스턴스와 연관되어 있습니다. API 키를 사용하여 생성된 토큰은 해당 서비스 인스턴스에 대한 호출에만 사용할 수 있습니다. 

{{site.data.keyword.watson}} 서비스에 대한 일반적인 curl 호출은 다음 양식을 사용하며, 여기서 `{token}`은 생성한 IAM 액세스 토큰입니다. 

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

또는, 토큰에 접두부 `Authorization: Bearer`(예: `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`)를 지정하여 파일에 저장할 수도 있습니다. 그 후 다음 명령을 사용할 수 있습니다. 

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## 토큰 새로 고치기
{: #refresh_token}

액세스 토큰의 수명을 연장하기 위해 `POST identity/token` 메소드에 의해 생성된 IAM 새로 고치기 토큰을 사용할 수 있습니다. 다음 명령은 기존 액세스 토큰을 새로 고칩니다. 이 명령에서 `{refresh-token}`은 생성한 IAM 새로 고치기 토큰입니다. 

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

토큰을 새로 고칠 때는 원래 토큰을 작성하는 데 사용한 것과 동일한 인증을 사용하십시오.
{: important}
