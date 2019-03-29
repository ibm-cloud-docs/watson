---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}

# {{site.data.keyword.watson}} 서비스에 대한 요청 로깅 제어
{: #gs-logging-overview}

기본적으로 모든 {{site.data.keyword.ibmwatson}} 서비스는 요청 및 요청의 결과를 로그합니다. 로깅은 향후 사용자를 위해 서비스를 개선할 목적으로만 수행됩니다. 로그된 데이터는 공유되거나 공개되지 않습니다.
{: shortdesc}

사용자의 개인정보 보호에 관심이 있거나 그렇지 않으면 사용자의 요청이 {{site.data.keyword.IBM_notm}}에서 사용되는 것을 원하지 않는 경우 이를 사용하지 않도록 선택할 수 있습니다.

{{site.data.keyword.IBM_notm}}에서 일반 서비스 개선에 사용자의 데이터를 사용하지 못하도록 하려면 요청에 대한 헤더 매개변수 X-Watson-Learning-Opt-Out을 `true` 또는 `1`로 설정하십시오. (false 또는 0 이외의 모든 값은 해당 호출에 대한 요청 로깅을 사용 안함으로 설정합니다.) {{site.data.keyword.IBM_notm}}에서 일반 서비스 개선에 사용하기를 원하지 않는 각 요청에 대해 헤더를 설정해야 합니다.

> **중요:** `X-WDC-PL-OPT-OUT`은 더 이상 사용되지 않음: 이전 이름인 X-WDC-PL-OPT-OUT은 현재 계속 작동하지만 더 이상 사용되지 않습니다. 값을 1로 설정하면 헤더가 요청 로깅을 사용하지 않는 것으로 승인합니다. 다른 모든 값을 설정하면 로깅이 사용으로 설정됩니다.
