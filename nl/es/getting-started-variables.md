---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: environment variable,service alias,VCAP services

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

# Variable de entorno de VCAP\_SERVICES
{: #vcapServices}

La variable de entorno `VCAP_SERVICES` contiene información que puede utilizar para interactuar con una instancia de servicio en {{site.data.keyword.cloud}}. Los campos de esta variable de entorno se establecen al enlazar una instancia de servicio a una aplicación.
{: shortdesc}

La aplicación necesita el URL y las credenciales de servicio para autenticarse con un servicio de {{site.data.keyword.watson}}. Una aplicación en ejecución normalmente lee estas variables de entorno después de enlazar un servicio a la misma, para extraer los pares nombre-valor necesarios. Puede enlazar los servicios de Cloud Foundry o el grupo de recursos a su app, pero los servicios de grupos de recursos necesitan un alias de servicio.

## Alias de servicio para los servicios IAM
{: #vcapIAMAlias}

Puede utilizar la variable de entorno `VCAP_SERVICES` con los servicios del grupo de recursos proporcionando al servicio un alias. Por ejemplo, utilice el mandato siguiente con la interfaz de línea de mandatos de {{site.data.keyword.cloud_notm}} para crear el alias `personality-insights-service` para un servicio llamado `my-personality-insights-service`.

```bash
ibmcloud resource service-alias-create personality-insights-service --instance-name my-personality-insights-service
```
{: pre}

A continuación, puede enlazar su app con el alias de servicio. Una aplicación en ejecución normalmente lee estas variables de entorno después de enlazar una instancia de servicio a la misma, para extraer los pares nombre-valor necesarios.

```bash
ibmcloud service bind {application-name} personality-insights-service.
```
{: pre}

## Ejemplo
{: #gs-variables-vcapIAMAliasExample}

Este ejemplo muestra la variable de entorno `VCAP_SERVICES` para el servicio de {{site.data.keyword.personalityinsightsshort}} enlazado a una aplicación. La instancia de servicio utiliza la autenticación de Cloud Foundry.

```json
{
  "VCAP_SERVICES": {
    "personality_insights": [
      {
        "credentials": {
          "password": "xxxxxxxxxxxx",
          "url": "https://gateway.watsonplatform.net/personality-insights/api",
          "username": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        },
        "label": "personality_insights",
        "name": "personality-insights-service",
        "plan": "standard",
        "tags": [
          "watson",
          "ibm_created",
          "ibm_dedicated_public",
          "lite"
        ]
      }
    ]
  }
}
```
{: codeblock}

La tabla siguiente describe el contenido de la variable de entorno `VCAP_SERVICES`.

| Elemento     | Descripción                                                                                |
|----------|--------------------------------------------------------------------------------------------|
| password | La contraseña de las credenciales de autenticación básica HTTP que se utilizan para conectarse al servicio |
| url      | El URL de conexión para el servicio                                                         |
| username | El nombre de usuario de las credenciales de autenticación básica HTTP utilizadas para conectarse al servicio |
| label    | El nombre asociado con el servicio en {{site.data.keyword.cloud_notm}}                                            |
| name     | El nombre de la instancia de servicio                                                           |
| plan     | El plan disponible para el servicio en {{site.data.keyword.cloud_notm}}                                              |
| tags     | Información adicional sobre el servicio                                                   |

## Visualización de variables de entorno
{: #gs-variables-vcapView}

Puede recuperar la información sobre la variable, bien desde la interfaz de línea de mandatos (CLI) de {{site.data.keyword.cloud_notm}} o desde la interfaz web.

- Con la CLI de {{site.data.keyword.cloud_notm}}: invoque el mandato `env` de Cloud Foundry tras crear un servicio y enlácelo a su aplicación:

    ```bash
    ibmcloud cf env {application-name}
    ```
    {: pre}

- En la interfaz de {{site.data.keyword.cloud_notm}}: La información sobre las variables de entorno está disponible en la página de resumen para una aplicación.
