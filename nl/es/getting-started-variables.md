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

# Variable de entorno de VCAP\_SERVICES
{: #vcapServices}

La variable de entorno `VCAP_SERVICES` contiene información que puede utilizar para interactuar con una instancia de servicio en {{site.data.keyword.Bluemix}}. Los campos de esta variable de entorno se establecen al enlazar un servicio a una aplicación.
{: shortdesc}

La aplicación necesita el URL y las credenciales de servicio de autenticación básica HTTP para autenticarse con un servicio de {{site.data.keyword.watson}}.

Una aplicación en ejecución normalmente lee estas variables de entorno, después de enlazar un servicio a la misma, para extraer los pares nombre-valor necesarios. Para obtener información sobre las variables, consulte [Despliegue de apps](/docs/manageapps/depapps.html#app_env) y busque "Variables de entorno".

## Ejemplo
Este ejemplo muestra la variable de entorno `VCAP_SERVICES` para el servicio de {{site.data.keyword.personalityinsightsshort}} enlazado a una aplicación:

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
| label    | El nombre asociado con el servicio en {{site.data.keyword.cloud_notm}}                                                              |
| name     | El nombre de la instancia de servicio                                                           |
| plan     | El plan disponible para el servicio en {{site.data.keyword.cloud_notm}}                                              |
| tags     | Información adicional sobre el servicio                                                   |

## Visualización de variables de entorno
Puede recuperar información sobre la variable desde la herramienta Cloud Foundry o desde la interfaz de {{site.data.keyword.cloud_notm}}.

- Con la herramienta de línea de mandatos de Cloud Foundry: Utilice el mandato `cf env` cuando haya creado un servicio y lo enlace a su aplicación:

    ```bash
    cf env {application-name}
    ```
    {: pre}

- En la interfaz de {{site.data.keyword.cloud_notm}}: La información sobre las variables de entorno está disponible en la página de resumen para una aplicación.
