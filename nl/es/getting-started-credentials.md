---

copyright:
  years: 2015, 2017
lastupdated: "2017-11-16"

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

# Credenciales de servicio para servicios de Watson

Las credenciales de servicio para un servicio de {{site.data.keyword.ibmwatson}} proporcionan autenticación a un servicio en {{site.data.keyword.cloud}}. Un conjunto de credenciales solo se puede utilizar con el servicio para el que se crean.
{: shortdesc}

Utilice las credenciales de servicio de forma distinta dependiendo del modelo de programación que utilice. Para obtener detalles, consulte [Programación de modelos para servicios de {{site.data.keyword.watson}}](/docs/services/watson/getting-started-develop.html).

Las credenciales de servicio son credenciales de autenticación básica HTTP para un servicio específico de {{site.data.keyword.watson}}.

## Obtención de credenciales manualmente
{: #getting-credentials-manually}

Puede encontrar las credenciales de servicio desde la interfaz web de {{site.data.keyword.cloud_notm}}.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Vaya a los servicios existentes. ([Llévame allí ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Seleccione el menú de la parte superior izquierda y, a continuación, seleccione **{{site.data.keyword.watson}}** para ir a la consola de {{site.data.keyword.watson}}.
    1.  Seleccione **Servicios existentes** desde **Servicios de Watson** para ver una lista de los servicios y proyectos.
1.  Ver las credenciales:
    - Si el servicio forma parte de un proyecto, pulse el nombre del proyecto que contiene el servicio. Vea las credenciales desde la sección **Credenciales** de la página de detalles del proyecto.
    - Si el servicio no forma parte de un proyecto, pulse el nombre de servicio que desee ver y, a continuación, seleccione **Credenciales de servicio**.

## Actualización de credenciales de servicio
{: #existing-svcs}

Puede actualizar credenciales de servicio para una instancia de servicio existente desde el panel de control del servicio.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/registration/?target=%2Fdeveloper%2Fwatson%2Fdashboard){: new_window}.
1.  Vaya a los servicios existentes. ([Llévame allí ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/developer/watson/existing-services){: new_window}):
    1.  Seleccione el menú de la parte superior izquierda y, a continuación, seleccione **{{site.data.keyword.watson}}** para ir a la consola de {{site.data.keyword.watson}}.
    1.  Seleccione **Servicios existentes** desde **Servicios de Watson** para ver una lista de los servicios.
1.  Seleccione el servicio que desee ver o actualizar y, a continuación, seleccione **Credenciales de servicio**.
1.  Suprimir o añadir credenciales:
    - Para suprimir credenciales, seleccione las credenciales y, a continuación, **Suprimir credencial**.
    - Para añadir credenciales, pulse **Nueva credencial** y, a continuación, **Añadir** en la ventana "Añadir nueva credencial". Vea y copie las credenciales nuevas.

Asegúrese de utilizar las credenciales nuevas en las aplicaciones:

- Si la aplicación obtiene sus credenciales mediante programación, detenga y reinicie la aplicación.
- Si las credenciales están incorporadas en el código de aplicación, actualice el código y vuelva a lanzar la aplicación.

Para obtener detalles sobre cómo actualizar una aplicación con credenciales nuevas con tiempo de inactividad mínimo, consulte [Actualización de apps](/docs/manageapps/updapps.html).

## Credenciales de {{site.data.keyword.Bluemix_dedicated_notm}}

En una instancia de [{{site.data.keyword.Bluemix_dedicated_notm}}](/docs/dedicated/index.html#dedicated), los mosaicos de servicio se implementan de forma que solo se permite un mosaico por servicio en una organización.

Siga estos pasos para referirse a un servicio en su propio espacio de trabajo de {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  Cree un conjunto de credenciales para el mosaico de referencia para el servicio que desee utilizar.
1.  Cree un servicio personalizado proporcionado por el usuario que utilice esas credenciales:

    1.  Utilice la interfaz de {{site.data.keyword.cloud_notm}} para seleccionar el mosaico para el servicio que desee utilizar, y cree un conjunto de credenciales para el servicio. Las credenciales se crean en formato JSON y contienen campos independientes para el URL en el que se comunican con el servicio y para el nombre de usuario y la contraseña a utilizar con ese servicio.
    1.  Utilice el mandato `cf cups` (crear servicio proporcionado por el usuario) para crear un servicio personalizado que esté vinculado con esas credenciales. Por ejemplo, dadas estas credenciales,

      ```json
      {
        "url": "https://gateway.watsonplatform.net/personality-insights/api",
        "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
        "password": "RuytRliRvoFN"
      }
      ```
      {: codeblock}

      cree una instancia de servicio personalizada proporcionada por el usuario denominada `Personality-Insights-Std`:

      ```bash
      cf cups Personality-Insights-Std -p "\"583b2e88-c80d-4ec0-93c1-98239f805146\":\"RuytRliRvoFN\""
      ```
      {: pre}

      Asegúrese de especificar el nombre de usuario y la contraseña como una serie individual entre comillas dobles y separados por dos puntos. Además, adjunte las series de nombre de usuario y contraseña con comillas dobles, y escápelas con una barra inclinada invertida.
      {: tip}

### Uso de aplicaciones con {{site.data.keyword.Bluemix_dedicated_notm}}

Algunas aplicaciones de muestra y kits de iniciación de {{site.data.keyword.watson}} en GitHub incluyen un botón **Desplegar en {{site.data.keyword.cloud_notm}}**. Estos botones no funcionarán en instalaciones de {{site.data.keyword.Bluemix_dedicated_notm}} porque se refieren al entorno público de {{site.data.keyword.cloud_notm}}.

Para utilizar aplicaciones de muestra y kits de iniciación en entornos de {{site.data.keyword.Bluemix_dedicated_notm}}:

1.  En las instrucciones para la aplicación, asegúrese de que el archivo `manifest.yml` que cree utilice el nombre del servicio que haya creado con el mandato `cf cups`.
1.  En estas mismas instrucciones, en lugar de ejecutar el mandato `cf create-service`, utilice un mandato `cf cups`. Sin embargo, si ha creado una instancia personalizada proporcionada por el usuario del servicio, no necesita crearla de nuevo para utilizarla con la aplicación de muestra o con el kit de iniciación.
