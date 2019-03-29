---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Watson tokens,Cloud Foundry

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

# Watson 토큰
{: #gs-tokens-watson-tokens}

{{site.data.keyword.watson}} 토큰을 사용하여, 모든 호출에 Cloud Foundry 서비스 인증 정보를 임베드하지 않고도 {{site.data.keyword.ibmwatson}} 서비스에 대한 인증된 요청을 전송하는 애플리케이션을 작성할 수 있습니다. {{site.data.keyword.watson}} 토큰을 얻으려면 서비스에 대한 Cloud Foundry 서비스 인증 정보를 사용해야 합니다. 자세한 정보는 [Cloud Foundry 서비스 인증 정보를 사용한 인증](/docs/services/watson?topic=watson-creating-credentials)을 참조하십시오. 사용자는 특정 서비스 인스턴스에 대한 토큰을 얻으며, 이 토큰은 해당 인스턴스에 대해서만 작동합니다.
{: shortdesc}

기본적으로 {{site.data.keyword.cloud_notm}}는 모든 새 서비스 인스턴스에 대해 Identity and Access Management(IAM) 인증을 사용합니다. 여기서 설명된 토큰은 Cloud Foundry 서비스 인증 정보와 함께 작동합니다. IAM 인증에 대한 자세한 정보는 [IAM 토큰을 사용한 인증](/docs/services/watson?topic=watson-iam#iam)을 참조하십시오.
{: important}

토큰을 가져와서 클라이언트 애플리케이션에 리턴하는 인증 프록시를 {{site.data.keyword.cloud}}에 작성할 수 있습니다. 이후 토큰을 사용하여 직접 서비스를 호출할 수 있습니다. 이 프록시를 사용하면 중간 서버 측 애플리케이션을 통해 모든 서비스 요청을 보낼 필요가 없습니다. 그렇지 않으면 클라이언트 애플리케이션에서 서비스 인증 정보가 노출되지 않도록 하기 위해 이 작업이 필요합니다.

## 토큰 얻기
{: #gs-tokens-obtain}

서비스에 대한 토큰을 얻으려면 `authorization` API의 `token` 메소드에 HTTP `GET` 요청을 전송합니다. 호스트는 사용자가 사용 중인 위치 및 서비스에 따라 달라집니다. 서비스의 API용 엔드포인트에서 호스트를 식별할 수 있습니다.

토큰을 얻을 서비스를 식별하려면 메소드의 `url` 조회 매개변수를 통해 서비스의 기본 URL을 전달합니다. 예를 들어, 다음 curl 명령은 `url` 매개변수를 사용하여 {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 페치합니다. 

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

토큰을 사용하지 않는 호출의 경우와 마찬가지로 요청과 함께 서비스에 대한 HTTP 기본 인증 인증 정보를 전달합니다. 예를 들어, 다음 curl 명령은 {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 얻습니다.

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

`--user` 옵션은 서비스에 대한 사용자의 *사용자 이름* 및 *비밀번호*를 연결하여 전달합니다. 인증 정보는 {{site.data.keyword.cloud_notm}}, 또는 서비스의 인스턴스에 바인드된 애플리케이션에 대한 `VCAP_SERVICES` 환경 변수에서 얻을 수 있습니다. {{site.data.keyword.cloud_notm}} 로그인 ID 및 비밀번호가 아니라 서비스 인증 정보를 사용해야 합니다.

이 메소드는 토큰을 1KB 크기의, Base-64로 인코딩된 암호화된 페이로드로 리턴합니다. 

토큰에는 1시간의 TTL(Time to Live)이 있으며, 이 시간이 경과한 후에는 더 이상 해당 토큰을 사용하여 서비스와의 연결을 설정할 수 없습니다. 해당 토큰을 사용하여 이미 설정된 기존 연결은 제한시간에 영향을 받지 않습니다. 만료되거나 올바르지 않은 토큰을 전달하려고 시도하면 {{site.data.keyword.cloud_notm}}에서 HTTP `401 Unauthorized` 상태 코드가 도출됩니다. 애플리케이션 코드가 이 리턴 코드에 대한 응답으로 토큰을 새로 고칠 준비가 되어 있어야 합니다.

## 토큰 사용
{: #gs-tokens-watson-use}

`X-Watson-Authorization-Token` 요청 헤더, `watson-token` 조회 매개변수 또는 쿠키를 사용하여 서비스의 HTTP 인터페이스에 토큰을 전달합니다. 각 HTTP 요청과 함께 토큰을 전달해야 합니다.

다음 curl 예제는 처음 두 가지 접근 방식을 보여줍니다.

1.  {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 얻어 `token`이라는 파일에 기록합니다. 

    ```bash
    curl -X GET --user {username}:{password} \
    --output token \
    "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
    ```
    {: pre}

1.  호출에서 `token` 파일의 컨텐츠를 {{site.data.keyword.texttospeechshort}} API의 `synthesize` 메소드로 전달합니다. 첫 번째 명령은 `X-Watson-Authorization-Token` 헤더를 통해 토큰을 전달합니다. 두 번째 명령은 `watson-token` 조회 매개변수를 통해 토큰을 전달합니다. 두 개의 명령은 동일합니다. 각 명령은 호출의 출력을 `hello_world.wav`라는 파일에 작성합니다.

    -   *UNIX 또는 Linux 쉘에서는* `token` 파일의 컨텐츠를 명령으로 경로 재지정하십시오.

        ```bash
      curl -X POST \
      --header "X-Watson-Authorization-Token: $(< ./token)" \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
      curl -X POST \
      --header "Content-Type: application/json" \
      --header "Accept: audio/wav" \
      --data "{\"text\":\"hello world\"}" \
      --output hello_world.wav \
      "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token=$(< ./token)"
        ```
        {: pre}

    -   *Windows 명령 프롬프트에서,* 명령에 `token` 파일의 컨텐츠를 삽입하십시오. 

        ```bash
        curl -X POST
        --header "X-Watson-Authorization-Token: {token_string}"
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize"
        ```
        {: pre}

        ```bash
        curl -X POST
        --header "Content-Type: application/json"
        --header "Accept: audio/wav"
        --data "{\"text\":\"hello world\"}"
        --output hello_world.wav
        "https://stream.watsonplatform.net/text-to-speech/api/v1/synthesize?watson-token={token_string}"
        ```
        {: pre}
