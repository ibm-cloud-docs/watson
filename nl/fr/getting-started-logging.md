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

# Contrôle de la journalisation des demandes pour les services {{site.data.keyword.watson}}

Par défaut, tous les services {{site.data.keyword.ibmwatson}} consignent les demandes et leurs résultats. La journalisation a pour unique but d'améliorer les services pour les futurs utilisateurs. Les données de journal ne sont ni partagées ni rendues publiques.
{: shortdesc}

Si vous vous préoccupez de protéger la confidentialité des informations personnelles de vos utilisateurs ou que vous ne voulez pas que vos demandes soit utilisées par {{site.data.keyword.IBM_notm}}, vous pouvez vous désinscrire.

Pour qu'{{site.data.keyword.IBM_notm}} n'utilise pas vos données à des fins d'amélioration générale des services, définissez le paramètre d'en-tête X-Watson-Learning-Opt-Out sur `true` ou `1` pour la demande. (Toute valeur différente de false ou 0 désactive la journalisation des demandes pour cet appel.) Vous devez définir l'en-tête sur chaque demande que vous ne voulez pas qu'{{site.data.keyword.IBM_notm}} utilise à des fins d'amélioration générale des services.

> **Important :** `X-WDC-PL-OPT-OUT` obsolète : l'ancien nom, X-WDC-PL-OPT-OUT, est obsolète, même s'il encore fonctionne actuellement. L'en-tête accepte une valeur 1 pour résilier la journalisation des demandes. Toute autre valeur active la journalisation.
