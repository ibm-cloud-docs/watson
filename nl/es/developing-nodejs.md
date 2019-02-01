---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-02"

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

# Desarrollo de una aplicación de {{site.data.keyword.watson}} en Node.js

Para demostrar el uso de su API REST, cada servicio de {{site.data.keyword.ibmwatson}} proporciona aplicaciones basadas en web de muestra en Node.js en el proyecto GitHub de {{site.data.keyword.watson}}.
{: shortdesc}

## Antes de empezar

- Cree una cuenta en {{site.data.keyword.cloud_notm}}, o utilice una cuenta existente. [Regístrese de forma gratuita ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}. La cuenta debe tener espacio para al menos 1 app y 1 servicio.
- El [tiempo de ejecución de Node.js ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nodejs.org/#download){: new_window}, incluido el gestor de paquetes de [npm](https://www.npmjs.com/){: new_window}. Asegúrese de incluir el mandato en la variable de entorno `PATH` después de la instalación.
- El cliente de línea de mandatos de [Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Si lo ha instalado anteriormente, asegúrese de que la versión esté actualizada.

## Obtención del código fuente

Visite el proyecto de [{{site.data.keyword.watson}} Github ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud){: new_window} para ver las apps Node.js de ejemplo que están disponibles para {{site.data.keyword.watson}}. A continuación, utilice GitHub para clonar un repositorio localmente.

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
    cf create-service personality_insights standard my-personality-insights-service
    ```
    {: pre}

### Configuración del entorno de la app

1.  Copie el archivo `.env.example` en un archivo `.env` nuevo.
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

1.  Pegue los valores `password` y `username` (sin comillas) desde JSON a las variables relevantes del archivo `.env`. Por ejemplo:

    ```json
    PERSONALITY_INSIGHTS_USERNAME=583b2e88-c80d-4ec0-93c1-98239f805146
    PERSONALITY_INSIGHTS_PASSWORD=RuytRliRvoFN
    ```
    {: codeblock}

### Instalación e inicio de la app

1.  Instale el paquete de apps de demostración en el entorno de ejecución Node.js local:

    ```bash
    npm install
    ```
    {: pre}

1.  Inicie la app:

    ```bash
    npm start
    ```
    {: pre}

1.  Apunte el navegador a http://localhost:3000 para probar la app. El puerto se especifica en el archivo `app.js`.

## Despliegue en {{site.data.keyword.cloud_notm}}

Puede utilizar Cloud Foundry para desplegar la versión local de la app en {{site.data.keyword.cloud_notm}}.

1.  En el directorio raíz de proyecto, abra el archivo manifest.yml:
    1.  En la sección `applications` del archivo `manifest.yml`, cambie el valor name a un nombre exclusivo para la versión de la app.
    1.  En la sección `services`, especifique el nombre de la instancia de servicio de {{site.data.keyword.watson}} que ha creado para la app. Si no recuerda el nombre de servicio, utilice el mandato `cf services` para listar los servicios.

        El ejemplo siguiente muestra un archivo `manifest.yml` modificado:

        ```yml
        ---
        declared-services:
          my-personality-insights-service:
            label: personality_insights
            plan: standard
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
