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

# Die Protokollierung von Anforderungen für {{site.data.keyword.watson}}-Services steuern

Alle {{site.data.keyword.ibmwatson}}-Services protokollieren standardmäßig Anforderungen und ihre Ergebnisse. Die Protokollierung erfolgt nur, um die Dienste für zukünftige Benutzer zu verbessern. Die protokollierten Daten werden weder freigegeben noch öffentlich gemacht.
{: shortdesc}

Wenn Ihnen der Schutz personenbezogener Daten von Benutzern wichtig ist oder Ihre Anforderungen nicht von {{site.data.keyword.IBM_notm}} verwendet werden sollen, können Sie sich für eine Deaktivierung (Opt-out) der Protokollierung entscheiden.

Um zu verhindern, dass Ihre Daten von {{site.data.keyword.IBM_notm}} für allgemeine Serviceverbesserungen verwendet werden, setzen Sie den Headerparameter X-Watson-Learning-Opt-Out für die Anforderung auf `true` oder `1`. (Ein anderer Wert als false oder 0 deaktiviert die Anforderungsprotokollierung für diesen Aufruf.) Sie müssen den Header für jede Anforderung festlegen, die {{site.data.keyword.IBM_notm}} nicht für allgemeine Serviceverbesserungen verwenden soll.

> **Wichtig:** `X-WDC-PL-OPT-OUT` ist veraltet: Der frühere Name X-WDC-PL-OPT-OUT wird nicht mehr verwendet, obwohl er noch funktioniert. Der Header akzeptiert den Wert 1, um die Anforderungsprotokollierung zu deaktivieren. Jeder andere Wert aktiviert die Protokollierung.
