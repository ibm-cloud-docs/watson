---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-08"

keywords: IAM tokens,IAM authentication

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

# Autenticación con señales IAM
{: #iam}

Puede utilizar señales de Identity and Access Management (IAM) de {{site.data.keyword.cloud}} para llevar a cabo solicitudes autenticadas a servicios de {{site.data.keyword.ibmwatson}} sin incluir las credenciales de servicio en cada llamada. La autenticación de IAM utiliza señales de acceso para la autenticación, que se adquieren enviando una solicitud con una clave de API.
{: shortdesc}

Los servicios de {{site.data.keyword.watson}} que se crean en un grupo de recursos o que
se migran de Cloud Foundry a un grupo de recursos utilizan autenticación IAM. De forma predeterminada,
todos los servicios nuevos de {{site.data.keyword.watson}} utilizan autenticación IAM.
{: note}

## Obtención de credenciales de servicio de IAM
{: #iam-getting-credentials-manually}

Para acceder a los métodos de la API utilizando las credenciales de servicio de IAM, antes debe recopilar las credenciales. Puede acceder las credenciales de servicio desde la interfaz web de {{site.data.keyword.cloud_notm}}.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}.

1.  Acceda a su [lista de recursos ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard){: new_window}.

1.  Seleccione una instancia de servicio.
1.  Pulse **Mostrar credenciales** para ver la **Clave de API** y el **URL** para las credenciales.
    1.  Para ver las señales para las credenciales, seleccione **Credenciales de servicio** en el menú de navegación en la parte
izquierda de la página.
    1.  Seleccione **Ver credenciales** para ver la clave de API y
la información de señal completas para las credenciales.

        ```json
        {
          "apikey": "3df...   ...Y7Pc9",
          "iam_apikey_description": "Auto generated apikey during resource-key operation for...",
          "iam_apikey_name": "auto-generated-apikey-31b336bc-2d6a-41c3-a8b2-e05ec6db19b4",
          "iam_role_crn": "crn:v1:bluemix:public:iam::::serviceRole:Manager",
          "iam_serviceid_crn": "crn:v1:bluemix:public:iam-identity::a/57d48380...::serviceid:...",
          "url": "https://gateway.watsonplatform.net/{service_name}/api"
        }
        ```

## Actualización de las credenciales de servicio de IAM
{: #update-existing-svcs}

Puede actualizar credenciales de servicio para una instancia de servicio existente desde el panel de control del servicio.

1.  Inicie sesión en [{{site.data.keyword.cloud_notm}} ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}){: new_window}.

1.  Acceda a su [lista de recursos ![Icono de enlaceexterno](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/dashboard){: new_window}.

1.  Seleccione **Credenciales de servicio** en el menú de navegación en la parte
izquierda de la página.
1.  Utilice los menús e iconos para suprimir las credenciales existentes o para añadir nuevas credenciales.

Asegúrese de que actualiza las credenciales en sus aplicaciones para cualquier cambio realizado.

## Obtención de una señal de IAM utilizando una clave de API de servicio de Watson
{: #iamtoken}

Puede acceder a las API de servicio de {{site.data.keyword.ibmwatson_notm}} utilizando las claves de API que se han generado para la instancia de servicio. La clave de API se utiliza para generar una señal de acceso
de IAM. También puede utilizar este proceso si está desarrollando una aplicación que necesita trabajar con otros servicios de {{site.data.keyword.cloud_notm}}.

Para obtener más información sobre la utilización de claves de API de forma segura, consulte [Claves de API de servicio de IAM](/docs/services/watson?topic=watson-api-key-bp).
{: tip}

El mandato curl siguiente utiliza el método `POST identity/token` para generar una señal de acceso de IAM pasando una clave de API. La cabecera `Content-Type` indica que los datos están codificados en el URL. Los parámetros `data-urlencode` pasan los datos codificados por URL al método.

```bash
curl -k -X POST \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Utilice la autenticación para generar la señal de acceso. Utilice la misma autenticación cuando renueve la señal.

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--header "Content-Type: application/x-www-form-urlencoded" \
--header "Accept: application/json" \
--data-urlencode "grant_type=urn:ibm:params:oauth:grant-type:apikey" \
--data-urlencode "apikey={apikey}" \
"https://iam.bluemix.net/identity/token"

```
{: pre}

En el ejemplo siguiente se muestra la respuesta esperada:

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: pre}

## Uso de una señal para la autenticación
{: #use_token}

Genere una señal de acceso IAM utilizando el método `POST identity/token`. A continuación, puede utilizar la señal para realizar llamadas de API autenticadas. Una clave de API está asociada a una instancia de servicio específica. Una señal que se genera con una clave de API se puede utilizar sólo para llamadas a dicha instancia de servicio.

Una llamada de curl típica a un servicio de {{site.data.keyword.watson}} adopta la forma siguiente, donde `{token}` es la señal de acceso de IAM que ha generado:

```bash
curl -X GET \
--header "Authorization: Bearer {token}" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

Como alternativa, puede anteponer a la señal el texto `Authorization: Bearer ` (por ejemplo, `Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs`) y guardarlo en un archivo. A continuación, utilice el mandato siguiente:

```bash
curl -X GET \
--header "$(cat token.txt)" \
"https://gateway.watsonplatform.net/discovery/api/v1/environments?version=2017-11-07"
```
{: pre}

## Renovación de una señal
{: #refresh_token}

Puede utilizar la señal de renovación IAM que se genera mediante el método `POST identity/token` para ampliar la vida de la señal de acceso. El mandato siguiente renueva una señal de acceso existente. En el mandato, `{refresh-token}` es la señal de renovación de IAM que ha generado.

```bash
curl -k -X POST \
--header "Authorization: Basic Yng6Yng=" \
--data-urlencode "grant_type=refresh_token" \
--data-urlencode "refresh_token={refresh-token}" \
"https://iam.bluemix.net/identity/token"
```
{: pre}

Cuando renueve una señal, utilice la misma autenticación que ha utilizado para crear la señal original.
{: important}
