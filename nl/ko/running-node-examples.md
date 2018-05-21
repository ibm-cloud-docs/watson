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

# Node.js SDK에서 예제 실행

{{site.data.keyword.watson}} Node.js SDK(Software Development Kit)에는 각 서비스에 대한 예제 코드가 포함되어 있습니다.
{: shortdesc}

## 시작하기 전에

1.  {{site.data.keyword.Bluemix}} 계정을 작성하거나 기존 계정을 사용하십시오. [무료로 등록하십시오. ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window} 
1.  실행할 {{site.data.keyword.watson}} 서비스에 대한 서비스 신임 정보를 가져오십시오. 세부사항은 [{{site.data.keyword.watson}} 서비스에 대한 서비스 신임 정보](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually)를 참조하십시오.
1.  [npm ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.npmjs.com/){: new_window} 패키지 관리자를 포함한 [Node.js 런타임 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://nodejs.org/#download){: new_window}이 설치되어 있는지 확인하십시오. 설치 후 명령을 `PATH` 환경 변수에 포함시켜야 합니다.

## 예제 업데이트 및 실행

1.  실행할 {{site.data.keyword.watson}} 서비스의 예제 코드 사본을 저장하십시오.
    1.  watson-developer-cloud [Node SDK ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window}의 `/examples` 디렉토리로 이동하십시오.
    1.  예제의 코드를 로컬로 저장하십시오. `personality_insights.v3.js`를 사용합니다.
1.  코드를 저장한 디렉토리로 변경하여 Node.js SDK를 설치하십시오.

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  서비스 신임 정보(api_key 또는 사용자 이름과 비밀번호)를 사용하도록 예제 코드를 업데이트하십시오.

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    다른 변경사항을 작성해야 할 수도 있습니다. 예를 들어, 여기서 {{site.data.keyword.personalityinsightsshort}}가 분석할 수 있는 충분한 텍스트를 추가합니다.

1.  `node {example_application.vn}.js` 명령을 실행하여 코드를 실행하십시오. 예를 들어, 다음과 같습니다.

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    서비스에서 가져온 정보가 응답에 포함됩니다. 예를 들어, {{site.data.keyword.personalityinsightsshort}} 서비스를 사용하는 경우 다음의 잘린 예제와 유사한 출력이 표시됩니다.

    ```json
    {
      "word_count": 224,
      "word_count_message": "There were 224 words in the input. We need a minimum of 600, preferably 1,200 or more, to compute statistically significant estimates",
      "processed_lang": "en",
      "personality": [
      {
        "trait_id": "big5_openness",
        "name": "Openness",
        "category": "personality",
        "percentile": 0.9015311332932557,
        "children": [. . .]
      },
      . . .
      ],
      "needs": [. . .],
      "values": [. . .],
      "consumption_preferences": [. . .]
    }
    ```
    {: codeblock}
