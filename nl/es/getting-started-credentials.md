---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: Cloud Foundry,authentication,service credentials

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

# Autenticación con credenciales de servicio de Cloud Foundry
{: #creating-credentials}

Las credenciales de servicio de Cloud Foundry para servicios de {{site.data.keyword.ibmwatson}}
proporcionan autenticación a un servicio en {{site.data.keyword.cloud}}. Las credenciales de servicio
utilizan autenticación básica HTTP para la autenticación en un servicio específico de
{{site.data.keyword.watson}}.
{: shortdesc}

Los servicios de {{site.data.keyword.watson}} que se crean en un grupo de recursos o que
se migran de Cloud Foundry a un grupo de recursos utilizan autenticación IAM. De forma predeterminada,
todos los servicios nuevos de {{site.data.keyword.watson}} utilizan autenticación IAM. Para obtener más información sobre grupos de recursos y la autenticación
IAM, consulte [Autenticación con señales IAM](/docs/services/watson?topic=watson-iam#iam-getting-credentials-manually).
{: note}

## Obtención de credenciales de servicio de Cloud Foundry
{: #getting-credentials-manually}

Para acceder a los métodos de la API utilizando las credenciales de servicio de Cloud Foundry, antes debe recopilar las credenciales. Puede acceder las credenciales de servicio desde la interfaz web de {{site.data.keyword.cloud_notm}}.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}.

1.  Acceda a su [lista de recursos ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard){: new_window}.

1.  Seleccione una instancia de servicio.
1.  Pulse **Mostrar credenciales** para ver el **Nombre de usuario**, **Contraseña** y **URL** de las credenciales.

## Actualización de credenciales de servicio de Cloud Foundry
{: #existing-svcs}

Puede actualizar credenciales de servicio para una instancia de servicio existente desde el panel de control del servicio.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}.

1.  Acceda a su [lista de recursos ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard){: new_window}.

1.  Seleccione **Credenciales de servicio** en el menú de navegación en la parte
izquierda de la página.
1.  Utilice los menús e iconos para suprimir las credenciales existentes o para añadir nuevas credenciales.

Asegúrese de que actualiza las credenciales en sus aplicaciones para cualquier cambio realizado.

## Credenciales de {{site.data.keyword.Bluemix_dedicated_notm}}

En una instancia de [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated?topic=dedicated-dedicated#dedicated), los mosaicos de servicio se implementan de forma que solo se permite un mosaico por servicio en una organización.

Siga estos pasos para referirse a un servicio en su propio espacio de trabajo de {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Cree un conjunto de credenciales para el mosaico de referencia para el servicio que desee utilizar.
1.  Cree un servicio personalizado proporcionado por el usuario que utilice esas credenciales:

    1.  Utilice la interfaz web para seleccionar el mosaico para el servicio que desee utilizar, y cree un conjunto de credenciales para el servicio. Las credenciales se crean en formato JSON e incluyen el URL de punto final de servicio, el nombre de usuario y la contraseña.
    1.  Utilice el mandato `ibmcloud cf cups` (crear servicio proporcionado por el usuario) para crear un servicio personalizado que esté enlazado con esas credenciales. Por ejemplo:

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      Con estas credenciales, cree una instancia de servicio personalizada proporcionada por el usuario denominada `Personality-Insights-Std`:

      ```bash
      ibmcloud cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Asegúrese de especificar el nombre de usuario y la contraseña como una serie individual entre comillas dobles y separados por dos puntos. Además, ponga las series de nombre de usuario y contraseña entre comillas dobles, y utilice una barra inclinada invertida para escape.
      {: tip}

### Uso de apps con {{site.data.keyword.Bluemix_dedicated_notm}}

Algunas apps de muestra y kits de inicio de {{site.data.keyword.watson}} en GitHub incluyen un botón **Desplegar en {{site.data.keyword.cloud_notm}}**. Estos botones no funcionan en instalaciones de {{site.data.keyword.Bluemix_dedicated_notm}} porque se refieren al entorno público de {{site.data.keyword.cloud_notm}}.

Para utilizar aplicaciones de muestra y kits de inicio en entornos de {{site.data.keyword.Bluemix_dedicated_notm}}, siga estos pasos:

1.  En las instrucciones para la aplicación, asegúrese de que el archivo `manifest.yml` que cree utilice el nombre del servicio que haya creado con el mandato `ibmcloud cf cups`.
1.  En esas mismas instrucciones, en lugar de ejecutar el mandato `ibmcloud cf create-service`,
utilice un mandato `ibmcloud cf cups`. Sin embargo, si ha creado una instancia personalizada proporcionada por el usuario del servicio, no necesita crearla de nuevo para utilizarla con la aplicación de muestra o con el kit de inicio.
