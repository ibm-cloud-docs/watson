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

# Control del registro de solicitud para servicios de {{site.data.keyword.watson}}

De forma predeterminada, todos los servicios de {{site.data.keyword.ibmwatson}} registran solicitudes y sus resultados. El registro se realiza únicamente para mejorar los servicios para usuarios futuros. Los datos registrados no se compartirán ni se harán públicos.
{: shortdesc}

Si está preocupado por la protección de la privacidad de la información personal de los usuarios o, de lo contrario, no desea que {{site.data.keyword.IBM_notm}} utilice sus solicitudes, puede optar por darse de baja.

Para evitar que {{site.data.keyword.IBM_notm}} utilice sus datos para las mejoras de servicio generales, establezca el parámetro de cabecera X-Watson-Learning-Opt-Out en `true` o `1` para la solicitud. (Cualquier valor que no sea false ni 0 inhabilita el registro de solicitud para dicha llamada). Debe establecer la cabecera en cada solicitud que no desee que {{site.data.keyword.IBM_notm}} utilice para las mejoras de servicio generales.

> **Importante:** `X-WDC-PL-OPT-OUT` en desuso: El nombre anterior, X-WDC-PL-OPT-OUT, está en desuso, aunque sigue funcionando por ahora. La cabecera acepta un valor de 1 para darse de baja del registro de la solicitud. Cualquier otro valor permite el registro.
