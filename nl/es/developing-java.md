---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-09"

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

# Desarrollo de una aplicación {{site.data.keyword.watson}} en Java

Para demostrar el uso de su API REST, cada servicio de {{site.data.keyword.ibmwatson}} proporciona aplicaciones basadas en web de muestra en Java en el proyecto GitHub de {{site.data.keyword.watson}}.
{: shortdesc}

## Antes de empezar

- Cree una cuenta en {{site.data.keyword.cloud_notm}}, o utilice una cuenta existente. [Regístrese de forma gratuita ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
- Instale el cliente de línea de mandatos de [Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Si ya lo tiene instalado, asegúrese de que su versión esté actualizada.
- Descargue e instale [Apache Maven ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://maven.apache.org/download.cgi){: new_window}.
- Instale el [perfil de IBM WebSphere Liberty ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/wasdev/downloads/){: new_window}. Puede descargar solo la versión de tiempo de ejecución del perfil de WebSphere Liberty o descargar una versión como un plugin para el IDE de Eclipse. Instale y configure también el *JDK de Java* o el *IDE de Eclipse*.

## Obtención del código fuente
Visite el proyecto de [GitHub de {{site.data.keyword.watson}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud){: new_window} para ver las apps Java de muestra disponibles para {{site.data.keyword.watson}}. A continuación, utilice GitHub para clonar un repositorio localmente.

## Instalación local
Si desea modificar la app o utilizarla como base para crear su propia app, instálela localmente. A continuación, puede desplegar la versión modificada de la app en {{site.data.keyword.cloud_notm}}.

### Configuración de un servicio de {{site.data.keyword.watson}}

1.  En la línea de mandatos, vaya al directorio de proyectos local de la app que ha clonado.
1.  Inicie sesión en {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Busque el nombre del servicio y del plan. Si el archivo `manifest.yml` de la app de muestra incluye una sección `declared-services`, tenga en cuenta `label` y `plan`. De lo contrario, utilice el mandato `cf marketplace` para obtener el nombre del servicio y de los planes:

    ```bash
    cf marketplace
    ```
    {: pre}

1.  Cree una instancia del servicio en {{site.data.keyword.cloud_notm}}: `cf create-service {service-name} {service-plan} {service-instance-name}`. Para service-name y service-plan, utilice la información del archivo manifest o el mandato `cf marketplace`.

    Por ejemplo, emita el mandato siguiente para crear la instancia de servicio para el servicio de {{site.data.keyword.personalityinsightsshort}}:

    ```bash
    cf create-service personality_insights tiered my-personality-insights-service
    ```
    {: pre}

### Configuración del entorno de la app

1.  Cree una clave de servicio en el formato `cf create-service-key {service_instance} {service_key}`. Por ejemplo:

    ```bash
    cf create-service-key my-personality-insights-service myKey
    ```
    {: pre}

1.  Recupere las credenciales de la clave de servicio utilizando el mandato `cf service-key {service_instance} {service_key}`. Por ejemplo:

    ```bash
    cf service-key my-conversation-service myKey
    ```
    {: pre}

    El resultado de este mandato es un objeto JSON, como en este ejemplo:

    ```json
    {
      "url": "https://gateway.watsonplatform.net/personality-insights/api",
      "username": "583b2e88-c80d-4ec0-93c1-98239f805146",
      "password": "RuytRliRvoFN"
    }

    ```
    {: codeblock}
1.  Copie los valores de la clave de servicio en el archivo de origen para la app, ubicado en el directorio de trabajo de la app:

    ```java
    private String baseURL - "https://gateway.watsonplatform.net/personality-insights/api";
    private String username - "583b2e88-c80d-4ec0-93c1-98239f805146";
    private String password - "RuytRliRvoFN";
    ```
    {: codeblock}

### Creación y ejecución de la app

Estas instrucciones utilizan la versión de tiempo de ejecución del perfil de WebSphere Liberty. Deberá modificar los pasos si utiliza el plugin de Eclipse.
{: tip}

1.  Cree la app ejecutando el siguiente mandato de Maven desde el directorio de trabajo de la app:

    ```java
    mvn clean package
    ```
    {: pre}

1.  Emita el mandato `server create` del perfil de WebSphere Liberty para crear el servidor para su aplicación. El nombre del servidor especificado como el argumento para el mandato es arbitrario, pero debe ser exclusivo en su entorno de perfil local de WebSphere Liberty.

    ```bash
    server create server-name
    ```
    {: pre}

1.  Copie el archivo `.war` de la app al directorio `wlp-installation/usr/servers/server-name/dropins`, donde *wlp-installation* es el directorio base para su instalación local del perfil de WebSphere Liberty, y *server-name* es el nombre especificado para el servidor en el paso anterior.
1.  Inicie el servidor en el perfil de WebSphere Liberty. Especifique el nombre del servidor para la app como el argumento para el mandato.

    ```bash
    server start server-name
    ```
    {: pre}

1.  Apunte el navegador a http://localhost:9080/webApp para probar la app. El puerto se especifica en el archivo `app.js`.

    El puerto se especifica en el archivo `wlp-installation/usr/servers/server-name/server.xml`. Para cambiar el puerto, cambie el valor de `server.xml`, y luego detenga y reinicie el servidor.

### Resolución de problemas

Si el servidor no se inicia correctamente o no responde, examine los archivos de registro del directorio `wlp-installation/usr/servers/server-name/logs` para determinar la causa.

## Despliegue en {{site.data.keyword.cloud_notm}}

Puede utilizar Cloud Foundry para desplegar la versión local de la app en {{site.data.keyword.cloud_notm}}.

1.  En el directorio raíz de proyecto, abra el archivo manifest.yml:
    1. En la sección `applications` del archivo `manifest.yml`, cambie el valor name a un nombre exclusivo para la versión de la app.
    1. En la sección `services`, especifique el nombre de la instancia de servicio de {{site.data.keyword.watson}} que ha creado para la app. Si no recuerda el nombre de servicio, utilice el mandato `cf services` para listar los servicios.

        El ejemplo siguiente muestra un archivo `manifest.yml` modificado:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: tiered
        applications:
        - name: personality-insights-app-test1
          command: npm start
          path: .
          memory: 256M
          instances: 1
          services:
          - my-personality-insights-service
          env:
           NPM_CONFIG_PRODUCTION: false
        ```
        {: codeblock}

1.  Inicie sesión en {{site.data.keyword.cloud_notm}}:

    ```bash
    cf login -a api https://api.ng.bluemix.net
    ```
    {: pre}

1.  Envíe por push la app a {{site.data.keyword.cloud_notm}} y especifique el nombre de la app desde el archivo `manifest.yml`. Por ejemplo:

    ```bash
    cf push personality-insights-app-test1
    ```
    {: pre}

    Apunte el navegador al URL especificado en la salida del mandato.
