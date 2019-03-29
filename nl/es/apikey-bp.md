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

# Claves de API de servicio de IAM
{: #api-key-bp}

Las claves de API de servicio de {{site.data.keyword.ibmwatson}} se utilizan para adquirir señales con portadora que se utilizan para autenticar llamadas a servicios {{site.data.keyword.ibmwatson_notm}}.
{: shortdesc}

A diferencia de la implementación de Identity and Access Management (IAM) de {{site.data.keyword.cloud_notm}} de las claves de API, las claves IAM de {{site.data.keyword.ibmwatson_notm}} se establecen en base a cada servicio. Puede asignar permisos a cada clave de API en lugar de a cada servicio. Esta implementación difiere del método estándar {{site.data.keyword.cloud_notm}} de las claves de API basadas en el usuario. Con {{site.data.keyword.ibmwatson_notm}}, a los usuarios se les asigna un rol para cada servicio. Para obtener más información sobre la implementación general de IAM de {{site.data.keyword.cloud_notm}}, consulte [Métodos recomendados de clave de API](/docs/services/iam?topic=iam-iamoverview#iamoverview).

Puede enlazar una clave de API de Watson sólo a una instancia de servicio única.
{: tip}

Cada vez que se crea una instancia de servicio {{site.data.keyword.ibmwatson_notm}}, se genera automáticamente un conjunto de credenciales para el rol `Gestor`. Estas credenciales están disponibles para el usuario en la página **Panel de instrumentos** de la instancia de servicio. Puede utilizarlas para llamar a *todos* los métodos de la API del servicio. Puede limitar las acciones posibles que se llevan a cabo al utilizar el servicio creando una clave de API con un rol distinto.

| Rol de acceso al servicio | Descripción de acciones |
|:-----------------|:-----------------|
| `Lector` | Los lectores solo pueden realizar acciones de lectura dentro de un servicio como, por ejemplo, ver recursos específicos del servicio. |
| `Escritor` | Los escritores tienen permisos más allá del rol de lector, incluida la creación y la edición de recursos específicos del servicio. |
| `Gestor` | Los gestores tienen permisos más allá del rol de escritor para completar las acciones privilegiadas tal como define el servicio. Además, pueden crear y editar recursos específicos del servicio. |
{: caption="Tabla 1. Ejemplo de roles de acceso al servicio para usuarios" caption-side="top"}

Para obtener más información sobre la creación de credenciales de servicio de {{site.data.keyword.ibmwatson_notm}} adicionales, consulte [Actualización de credenciales de servicio de IAM](/docs/services/watson?topic=watson-iam#update-existing-svcs).

## Métodos recomendados de claves de API
{: #api-bp}

Como método de autenticación para el servicio, las claves de API se deben mantener seguras. Las siguientes prácticas recomendadas le ayudan a mantener una clave de API segura y a reducir la posibilidad de exponer públicamente las credenciales que comprometan la aplicación.

-   *Utilice una clave de API con el rol adecuado para la función que está utilizando.*

    Cuando cree una aplicación de usuario final, plantéese usar una clave de API con el rol `Lector` para todas las llamadas a métodos `GET` de la API. El rol `Lector` no puede realizar funciones de eliminado ni adición a la instancia de servicio.

-   *No incluya la clave de API directamente en el código.*

    Las claves de API incluidas en el código podrían ser visibles para los usuarios. En lugar de incluir las claves de API en el código de la aplicación, plantéese almacenarlas en variables de entorno o en archivos fuera del sistema de control del código fuente.

-   *No almacene una clave de API en archivos dentro del sistema de control del código fuente de la aplicación.*

    Si almacena las claves de API en archivos, conserve dichos archivos fuera del código fuente de la aplicación. Esta práctica es especialmente importante si se utiliza un sistema de gestión de código fuente público como GitHub.

-   *Vuelva a generar las claves de API periódicamente.*

    Puede crear nuevas credenciales en cualquier momento.
    1.  En la lista de recursos, seleccione una instancia de servicio.
    1.  Pulse **Credenciales de servicio** en el menú de navegación en la parte izquierda de la página.

     Cuando migre su aplicación a las nuevas credenciales, no olvide suprimir las antiguas, utilizando el icono de papelera para las credenciales que quiera suprimir.
     {: tip}
