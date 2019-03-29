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

# IAM 서비스 API 키
{: #api-key-bp}

{{site.data.keyword.ibmwatson}} 서비스 API 키는 Bearer 토큰을 획득하는 데 사용되며, 이는 {{site.data.keyword.ibmwatson_notm}} 서비스에 대한 호출을 인증하는 데 사용됩니다.
{: shortdesc}

API 키의 표준 {{site.data.keyword.cloud_notm}} Identity and Access Management(IAM) 구현과 달리 {{site.data.keyword.ibmwatson_notm}} IAM 키는 각 서비스 기반으로 설정됩니다. 사용자는 각 서비스가 아니라 각 API 키에 권한을 지정할 수 있습니다. 이 구현은 사용자 기반 API 키인 표준 {{site.data.keyword.cloud_notm}} 방법과는 다릅니다. {{site.data.keyword.ibmwatson_notm}}에서 사용자에게는 각 서비스에 대해 역할이 지정됩니다. 일반 {{site.data.keyword.cloud_notm}} IAM 구현에 대한 자세한 정보는 [API 키 우수 사례](/docs/services/iam?topic=iam-iamoverview#iamoverview)를 참조하십시오. 

하나의 Watson API 키는 하나의 서비스 인스턴스에만 바인드할 수 있습니다.
{: tip}

`Manager` 역할에 대한 인증 정보 세트는 {{site.data.keyword.ibmwatson_notm}} 서비스 인스턴스가 작성될 때마다 자동으로 생성됩니다. 이러한 인증 정보는 서비스 인스턴스의 **대시보드** 페이지에 있습니다. 사용자는 이를 사용하여 서비스 API의 *모든* 메소드를 호출할 수 있습니다. 다른 역할로 API 키를 작성하여 서비스를 사용할 때 수행 가능한 조치를 제한할 수 있습니다. 

| 서비스 액세스 역할 | 조치에 대한 설명 |
|:-----------------|:-----------------|
| `Reader` | Reader는 서비스 고유 리소스 보기와 같이 서비스 내의 읽기 전용 조치만 수행할 수 있습니다. |
| `Writer` | Writer는 서비스 고유 리소스의 작성 및 편집을 포함, Reader 역할보다 큰 권한을 갖습니다. |
| `Manager` | Manager는 서비스에 의해 정의된, 권한이 필요한 조치를 완료하는 데 있어서 Writer 역할보다 큰 권한을 갖습니다. 또한 서비스 고유 리소스를 작성하고 편집할 수 있습니다. |
{: caption="표 1. 사용자의 서비스 액세스 역할 예" caption-side="top"}

추가 {{site.data.keyword.ibmwatson_notm}} 서비스 인증 정보를 작성하는 데 대한 자세한 정보는 [IAM 서비스 인증 정보](/docs/services/watson?topic=watson-iam#update-existing-svcs)를 참조하십시오. 

## API 키 우수 사례
{: #api-bp}

API 키는 서비스에 대한 인증 방법이므로 항상 안전하게 보존해야 합니다. 다음 우수 사례는 API 키를 안전하게 유지하며 인증 정보가 공개적으로 노출되어 애플리케이션의 보안을 위협할 가능성을 줄입니다. 

-   *사용하는 기능에 적합한 역할을 가진 API 키를 사용하십시오. *

    일반 사용자 애플리케이션을 작성할 때는 `GET` API 메소드에 대한 호출에 대해 `Reader` 역할을 가진 API 키를 사용하는 것을 고려하십시오. `Reader` 역할은 서비스 인스턴스에 해롭거나 추가적인 기능을 수행할 수 없습니다. 

-   *API 키를 코드에 직접 임베드하지 마십시오. *

    코드에 임베드된 API 키는 사용자에게 노출될 수 있습니다. 애플리케이션 코드에 API 키를 임베드하는 대신 환경 변수, 또는 소스 코드 제어 시스템 외부의 파일에 저장하는 것을 고려하십시오. 

-   *애플리케이션의 소스 코드 제어 시스템 내 파일에 API 키를 저장하지 마십시오. *

    이러한 파일에 API 키를 저장하는 경우에는 해당 파일을 애플리케이션의 소스 코드에 포함시키지 마십시오. 이러한 조치는 GitHub와 같은 공용 소스 코드 관리 시스템을 사용하는 경우 특히 중요합니다. 

-   *API 키를 주기적으로 다시 생성하십시오. *

    사용자는 언제든지 새 인증 정보를 작성할 수 있습니다. 
    1.  리소스 목록에서 서비스 인스턴스를 선택하십시오. 
    1.  페이지 왼쪽의 탐색 메뉴에서 **서비스 인증 정보**를 클릭하십시오. 

     애플리케이션을 새 인증 정보로 마이그레이션한 후에는 삭제할 인증 정보의 휴지통 아이콘을 사용하여 이전 인증 정보를 삭제하십시오.
     {: tip}
