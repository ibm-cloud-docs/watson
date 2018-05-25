---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-03"

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

# Controlando a criação de log de solicitação para os serviços do {{site.data.keyword.watson}}

Por padrão, todas as solicitações de log de serviços do {{site.data.keyword.ibmwatson}} e seus
resultados. A criação de log é feita somente para melhorar os serviços para futuros usuários. Os dados registrados não são
compartilhados ou tornados públicos.
{: shortdesc}

Se você estiver preocupado com a proteção da privacidade das informações pessoais dos usuários ou não quiser que suas
solicitações sejam usadas pelo {{site.data.keyword.IBM_notm}}, será possível optar pelo cancelamento.

Para evitar o {{site.data.keyword.IBM_notm}} use seus dados para melhorias gerais de serviço, configure o
parâmetro de cabeçalho X-Watson-Learning-Opt-Out para `true` ou `1` para a solicitação. (Qualquer
valor diferente de false ou 0 desativa a criação de log da solicitação para essa chamada.) Deve-se configurar o cabeçalho em cada
solicitação que você não deseje que o {{site.data.keyword.IBM_notm}} use para melhorias gerais de serviço.

> **Importante:** `X-WDC-PL-OPT-OUT` descontinuado: o nome anterior, X-WDC-PL-OPT-OUT, foi
descontinuado, embora ele continue a funcionar no momento. O cabeçalho aceita um valor de 1 para cancelar a criação de
log da solicitação. Qualquer outro valor ativa a criação de log.
