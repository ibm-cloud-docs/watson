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

# Chiavi API del servizio IAM
{: #api-key-bp}

Le chiavi API del servizio {{site.data.keyword.ibmwatson}} vengono utilizzate per acquisire i token di connessione che a loro volta vengono utilizzati per autenticare le chiamate ai servizi {{site.data.keyword.ibmwatson_notm}}.
{: shortdesc}

A differenza dell'implementazione standard di {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) delle chiavi API, le chiavi IAM {{site.data.keyword.ibmwatson_notm}} vengono impostate in base al servizio. Puoi assegnare le autorizzazioni a ciascuna chiave API invece che a ciascun servizio. Questa implementazione è diversa dal metodo {{site.data.keyword.cloud_notm}} standard delle chiavi API basate sull'utente. Con {{site.data.keyword.ibmwatson_notm}}, agli utenti viene assegnato un ruolo per ciascun servizio. Per ulteriori informazioni sull'implementazione generale di {{site.data.keyword.cloud_notm}} IAM, vedi [Procedure consigliate della chiave API](/docs/services/iam?topic=iam-iamoverview#iamoverview).

Puoi associare una chiave API Watson solo a una singola istanza del servizio.
{: tip}

Viene generata automaticamente una serie di credenziali per il ruolo `Gestore` ogni volta che viene creata un'istanza del servizio {{site.data.keyword.ibmwatson_notm}}. Queste credenziali le trovi disponibili nella pagina **Dashboard** dell'istanza del servizio. Puoi utilizzarle per richiamare *tutti* i metodi dell'API del servizio. Puoi limitare le azioni possibili che vengono eseguite quanto utilizzi il servizio creando una chiave API con un ruolo diverso. 

| Ruolo di accesso al servizio | Descrizione delle azioni |
|:-----------------|:-----------------|
| `Lettore` | I lettori possono eseguire azioni di sola lettura all'interno di un servizio, ad esempio visualizzare le risorse specifiche del servizio. |
| `Scrittore` | Gli scrittori dispongono di autorizzazioni al di sopra del ruolo di lettore, incluse la creazione e la modifica di risorse specifiche del servizio. |
| `Gestore` | I gestori dispongono di autorizzazioni al di sopra del ruolo di scrittore per completare le azioni privilegiate definite dal servizio. Inoltre, possono creare e modificare le risorse specifiche del servizio. |
{: caption="Tabella 1. Ruoli di accesso al servizio di esempio per gli utenti" caption-side="top"}

Per ulteriori informazioni sulla creazione di credenziali aggiuntive del servizio {{site.data.keyword.ibmwatson_notm}}, vedi [Aggiornamento delle credenziali del servizio IAM](/docs/services/watson?topic=watson-iam#update-existing-svcs).

## Procedure consigliate della chiave API
{: #api-bp}

Poiché costituiscono il metodo di autenticazione al tuo servizio, le tue chiavi API devono essere protette. Le procedure consigliate riportate di seguito ti aiutano a proteggere una chiave API e a ridurre la possibilità di esporre pubblicamente le credenziali, cosa che comprometterebbe la tua applicazione.

-   *Utilizza una chiave API con il ruolo appropriato per la funzione che stai utilizzando.*

    Quando crei un'applicazione dell'utente finale, prendi in considerazione di utilizzare una chiave API con il ruolo `Lettore` per tutte le chiamate ai metodi API `GET`. Il ruolo `Lettore` non può eseguire funzioni distruttive o addizionali nell'istanza del servizio. 

-   *Non integrare la chiave API direttamente nel codice.*

    Le chiavi API che vengono integrate nel codice potrebbero essere esposte ai tuoi utenti. Invece di integrare le chiavi API nel tuo codice dell'applicazione, prendi in considerazione di memorizzarle nelle variabili di ambiente o nei file al di fuori del tuo sistema di controllo del codice sorgente. 

-   *Non memorizzare una chiave API nei file all'interno del sistema di controllo del codice sorgente della tua applicazione.*

    Se memorizzi le chiavi API nei file, conserva questi file al di fuori del codice sorgente della tua applicazione. Questa procedura è particolarmente importante se utilizzi un sistema di gestione del codice sorgente pubblico come GitHub.

-   *Rigenera periodicamente le tue chiavi API.*

    Puoi creare nuove credenziali in qualsiasi momento.
    1.  Dall'elenco risorse, seleziona un'istanza del servizio. 
    1.  Fai clic su **Credenziali del servizio** dal menu di navigazione di sinistra della pagina. 

     Quando migri la tua applicazione alle nuove credenziali, non dimenticare di eliminare le vecchie credenziali utilizzando l'icona del cestino per le credenziali che vuoi eliminare.
     {: tip}
