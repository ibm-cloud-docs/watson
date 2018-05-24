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

# Interfaz de línea de mandatos de Cloud Foundry

Puede utilizar la herramienta CLI (interfaz de línea de mandatos) `cf` para trabajar con aplicaciones de {{site.data.keyword.cloud}} en el sistema local. La herramienta proporciona una interfaz enriquecida para una experiencia de desarrollo completa.
{: shortdesc}

Para obtener más información sobre cómo trabajar con las herramientas de Cloud Foundry para el desarrollo de aplicaciones, consulte [Cómo funciona Cloud Foundry con {{site.data.keyword.cloud_notm}}](/docs/overview/cf.html) y la [Documentación de Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.cloudfoundry.org/){: new_window}.

## Instalación de la interfaz de línea de mandatos cf

Puede encontrar la versión más reciente de la herramienta en la sección **Descargas** del [repositorio de GitHub de Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/cloudfoundry/cli#downloads){: new_window}. Verá dos tipos de paquetes:

- **Instaladores** se integran con la herramienta de gestión de paquetes de la plataforma o con el sistema de rastreo de software instalado para instalar la herramienta en su ubicación "oficial".
- **Binarios** proporcionan archivos de archivado que puede instalar en cualquier ubicación.

Asegúrese de incluir el mandato en la variable de entorno `PATH` después de instalar la herramienta.
{: tip}

## Mandatos para instancias de servicio

Mandatos `cf` útiles para gestionar instancias de servicio existentes de {{site.data.keyword.watson}} en {{site.data.keyword.cloud_notm}}:

- [**cf services**](/docs/cli/reference/cfcommands/index.html#cf_services): Lista las instancias de servicio del espacio actual.

  ```bash
  cf services
  ```
  {: pre}

  Si conoce el nombre del servicio, utilice el mandato `cf service` para ver información sobre el mismo. Por ejemplo:

  ```bash
  cf service conversation-tutorial
  ```
  {: pre}

- [**cf create-service**](/docs/cli/reference/cfcommands/index.html#cf_create-service): Crear una instancia del servicio que puede enlazar a su aplicación que se ejecuta en {{site.data.keyword.cloud_notm}}:

    ```bash
    cf create-service {service-name} {service-plan} {service-instance-name}
    ```
    {: pre}

    - Para los argumentos *{service-name}* y *{service-plan}*, utilice la salida del mandato `cf marketplace`.
    - Especifique el {service-instance-name} que desee crear. Para muchas aplicaciones, el nombre se especifica en el archivo `manifest.yml` u otro archivo de configuración para la aplicación.

- **cf delete-service**: Suprimir una instancia de servicio:

    ```bash
    cf delete-service {service-instance-name}
    ```
    {: pre}

    Al suprimir un servicio que ya no necesita, libera recursos en {{site.data.keyword.cloud_notm}} que no necesita.

## Mandatos para desplegar y ejecutar aplicaciones

Mandatos `cf` comunes para desplegar y ejecutar aplicaciones en {{site.data.keyword.cloud_notm}}:

- [**cf api**](/docs/cli/reference/cfcommands/index.html#cf_api): Especificar la ubicación de las API de {{site.data.keyword.watson}}:

  ```bash
  cf api https://api.ng.bluemix.net
  ```
  {: pre}

- [**cf login**](/docs/cli/reference/cfcommands/index.html#cf_login). Inicie sesión en {{site.data.keyword.cloud_notm}} con su {{site.data.keyword.ibmid}} y contraseña:

  ```bash
  cf login -u {username} -p {password}
  ```
  {: pre}

- [**cf marketplace**](/docs/cli/reference/cfcommands/index.html#cf_marketplace): Listar los servicios en el catálogo {{site.data.keyword.cloud_notm}} al que se puede conectar:

  ```bash
  cf marketplace
  ```
  {: pre}

  La salida incluye el nombre de cada servicio y la lista de planes bajo la cual está disponible el servicio. Necesita esta información al crear una instancia del servicio para la aplicación que se ejecuta en {{site.data.keyword.cloud_notm}}.

  Si sabe el nombre del servicio, utilice la opción `-s`. Por ejemplo:

  ```bash
  cf marketplace -s conversation
  ```
  {: pre}

- [**cf push**](/docs/cli/reference/cfcommands/index.html#cf_push): Desplegar o actualizar una aplicación en {{site.data.keyword.cloud_notm}}. La salida incluye el URL para acceder a su aplicación.

  ```bash
  cf push {application-name}
  ```
  {: pre}

  Los resultados del mandato utilizan a menudo el archivo `manifest.yml` u otro archivo de configuración para encontrar el archivo que se enviará por push (por ejemplo, un archivo `.war` para Java o un archivo `.zip` para Node.js). Sin embargo, puede especificar el nombre de vía de acceso del archivo de aplicación mediante la opción `-p`.

## Mandatos para aplicaciones existentes

Mandatos `cf` útiles para gestionar aplicaciones existentes en {{site.data.keyword.cloud_notm}}:

- [**cf apps**](/docs/cli/reference/cfcommands/index.html#cf_apps) Listar las aplicaciones que están desplegadas en el espacio actual.

  ```bash
  cf apps
  ```
  {: pre}

  Incluya el nombre de aplicación para ver información detallada sobre la salud y el estado de una aplicación:

  ```bash
  cf app {application-name}
  ```
  {: pre}

- **cf restage**: Recargar y reiniciar la aplicación en {{site.data.keyword.cloud_notm}}:

  ```bash
  cf restage {application-name}
  ```
  {: pre}

Este mandato reinicia la aplicación como existe en {{site.data.keyword.cloud_notm}}. Para cargar la versión más reciente de la aplicación en {{site.data.keyword.cloud_notm}} antes de reiniciarla, utilice el mandato `cf push` en su lugar.

- [**cf delete**](/docs/cli/reference/cfcommands/index.html#cf_delete): Suprimir una aplicación:

  ```bash
  cf delete application-name
  ```
  {: pre}

  Al suprimir una app, libera memoria y cuota de {{site.data.keyword.cloud_notm}} para utilizarlas para otras aplicaciones.

- [**cf bind-service**](/docs/cli/reference/cfcommands/index.html#cf_bind-service): Enlaza un servicio a una aplicación.

  Normalmente, puede utilizar el mandato `cf push` para enlazar una instancia de un servicio a una aplicación que la utilice. Puede enlazar una instancia de servicio existente a una aplicación existente con el mandato `cf bind-service`:

  ```bash
  cf bind-service {application-name} {service-instance-name}
  ```
  {: pre}

  A continuación, emita `cf restage` o `cf push` para reiniciar la aplicación.

- **cf unbind-service**: Elimina la asociación entre una aplicación y un servicio:

  ```bash
  cf unbind-service {application-name} {service-instance-name}
  ```
  {: pre}

  Puede que necesite emitir el mandato `cf restage` o `cf push` para reflejar el cambio.

  Este mandato elimina las credenciales de autenticación de la variable de entorno `VCAP_SERVICES`. Utilice `cf delete-service` o `cf delete` para eliminar la instancia de servicio o la aplicación.
  {: tip}
