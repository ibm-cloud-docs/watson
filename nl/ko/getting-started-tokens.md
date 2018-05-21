---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-07"

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

# 인증을 위한 토큰
토큰을 사용하여 모든 호출에 서비스 신임 정보를 임베드하지 않고 {{site.data.keyword.ibmwatson}} 서비스에 대한 인증된 요청을 하는 애플리케이션을 작성합니다.
{: shortdesc}

토큰을 가져와서 클라이언트 애플리케이션에 리턴하는 인증 프록시를 {{site.data.keyword.cloud}}에 작성할 수 있습니다. 이후 토큰을 사용하여 직접 서비스를 호출할 수 있습니다. 이 프록시를 사용하면 중간 서버 측 애플리케이션을 통해 모든 서비스 요청을 보낼 필요가 없습니다. 그렇지 않으면 클라이언트 애플리케이션에서 서비스 신임 정보가 노출되지 않도록 하기 위해 이 작업이 필요합니다.

직접 상호작용 프로그래밍 모델에 대한 자세한 정보는 [{{site.data.keyword.watson}} 서비스에 대한 프로그래밍 모델](/docs/services/watson/getting-started-develop.html)을 참조하십시오.

서비스의 토큰을 얻으려면 서비스에 대한 HTTP 기본 인증 신임 정보가 있어야 합니다. 자세한 정보는 [{{site.data.keyword.watson}} 서비스에 대한 신임 정보](/docs/services/watson/getting-started-credentials.html)를 참조하십시오. 특정 서비스에 대한 토큰을 얻으며 토큰은 지정된 서비스에만 작동합니다.

## 토큰 얻기
서비스에 대한 토큰을 얻으려면 `authorization` API의 `token` 메소드에 HTTP `GET` 요청을 전송합니다. 호스트는 사용 중인 서비스에 따라 다릅니다. 서비스의 API용 엔드포인트에서 호스트를 식별할 수 있습니다.

- **gateway.watsonplatform.net**: {{site.data.keyword.personalityinsightsshort}}와 같은 서비스는 엔드포인트 `https://gateway.watsonplatform.net/authorization/api/v1/token`에서 `token` 메소드를 호출합니다.
- **stream.watsonplatform.net**: {{site.data.keyword.speechtotextshort}} 및 {{site.data.keyword.texttospeechshort}}와 같은 서비스는 https://stream.watsonplatform.net/authorization/api/v1/token 에서 `token` 메소드를 호출합니다.

토큰을 얻을 서비스를 식별하려면 메소드의 `url` 조회 매개변수를 통해 서비스의 기본 URL을 전달합니다. 예를 들어, {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 페치하려면 `url` 매개변수에 다음 값을 전달하십시오.

```bash
url=https://stream.watsonplatform.net/text-to-speech/api
```
{: codeblock}

토큰을 사용하지 않는 호출의 경우와 마찬가지로 요청과 함께 서비스에 대한 HTTP 기본 인증 신임 정보를 전달합니다. 예를 들어, 다음 curl 명령은 {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 얻습니다.

```bash
curl -X GET --user {username}:{password} \
"https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
```
{: pre}

`--user` 옵션은 서비스에 대한 *사용자 이름*과 *비밀번호*의 연결을 전달하며, 이는 서비스의 인스턴스에 바인드된 애플리케이션에 대한 `VCAP_SERVICES` 환경 변수 또는 {{site.data.keyword.cloud_notm}}에서 얻을 수 있습니다. {{site.data.keyword.cloud_notm}} 로그인 ID 및 비밀번호가 아니라 서비스 신임 정보를 사용해야 합니다.

이 메소드는 토큰을 암호화된 페이로드의 1KB Base-64 인코딩으로 리턴합니다. 

토큰에는 1시간의 TTL(Time to Live)이 있습니다. 이 시간 이후에는 토큰을 사용하여 서비스와의 연결을 더 이상 설정할 수 없습니다. 토큰을 사용하여 이미 설정된 기존 연결은 제한시간에 영향을 받지 않습니다. 만료되거나 올바르지 않은 토큰을 전달하려고 시도하면 {{site.data.keyword.cloud_notm}}에서 HTTP `401 Unauthorized` 상태 코드가 도출됩니다. 애플리케이션 코드가 이 리턴 코드에 대한 응답으로 토큰을 새로 고칠 준비가 되어 있어야 합니다.

## 토큰 사용

`X-Watson-Authorization-Token` 요청 헤더, `watson-token` 조회 매개변수 또는 쿠키를 사용하여 서비스의 HTTP 인터페이스에 토큰을 전달합니다. 각 HTTP 요청과 함께 토큰을 전달해야 합니다.

다음 curl 예제는 처음 두 가지 접근 방식을 보여줍니다.

- {{site.data.keyword.texttospeechshort}} 서비스에 대한 토큰을 가져와서 `token`이라는 파일에 작성합니다.

  ```bash
  curl -X GET --user {username}:{password}
  --output token
  "https://stream.watsonplatform.net/authorization/api/v1/token?url=https://stream.watsonplatform.net/text-to-speech/api"
  ```
  {: pre}

- 호출에서 `token` 파일의 컨텐츠를 {{site.data.keyword.texttospeechshort}} API의 `synthesize` 메소드로 전달합니다. 첫 번째 명령은 `X-Watson-Authorization-Token` 헤더를 통해 토큰을 전달합니다. 두 번째 명령은 `watson-token` 조회 매개변수를 통해 토큰을 전달합니다. 두 개의 명령은 동일합니다. 각 명령은 호출의 출력을 `hello_world.wav`라는 파일에 작성합니다.

    - *UNIX 또는 Linux 쉘에서는* `token` 파일의 컨텐츠를 명령으로 경로 재지정하십시오.

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

    - *Windows 명령 프롬프트에서는* `token` 파일의 컨텐츠를 명령에 임베드하십시오.

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
