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

# Controllo della registrazione delle richieste per i servizi {{site.data.keyword.watson}}

Per impostazione predefinita, tutti i servizi {{site.data.keyword.ibmwatson}} registrano le richieste e i loro risultati. La registrazione viene eseguita solo per migliorare i servizi per i futuri utenti. I dati registrati non vengono condivisi o resi pubblici.
{: shortdesc}

Se sei preoccupato della protezione della privacy delle informazioni personali degli utenti o se per altri motivi non vuoi che le tue richieste vengano utilizzate da {{site.data.keyword.IBM_notm}}, puoi optare per l'esclusione (opt-out).

Per evitare che {{site.data.keyword.IBM_notm}} utilizzi i tuoi dati per dei generici miglioramenti dei servizi, imposta il parametro dell'intestazione X-Watson-Learning-Opt-Out su `true` o `1` per la richiesta. (Qualsiasi valore diverso da false o 0 disabilita la registrazione delle richieste per tale chiamata). Devi impostare l'intestazione su ogni richiesta che non vuoi venga utilizzata da {{site.data.keyword.IBM_notm}} per dei generici miglioramenti dei servizi.

> **Importante:** `X-WDC-PL-OPT-OUT` è stato dichiarato obsoleto. Il nome precedente, X-WDC-PL-OPT-OUT, è stato dichiarato obsoleto, anche se per il momento continua a funzionare. L'intestazione accetta un valore di 1 per optare per l'esclusione dalla registrazione delle richieste. Qualsiasi altro valore abilita la registrazione.
