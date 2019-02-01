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

# Ejecución de ejemplos del SDK de Node.js

El SDK (Software Development Kit) de Node.js de {{site.data.keyword.watson}} incluye código de ejemplo para cada servicio.
{: shortdesc}

## Antes de empezar

1.  Cree una cuenta de {{site.data.keyword.Bluemix}}, o utilice una cuenta existente. [Regístrese de forma gratuita ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/registration/?target=/catalog/%3fcategory=watson){: new_window}.
1.  Obtenga las credenciales de servicio para el servicio de {{site.data.keyword.watson}} que desee ejecutar. Para obtener detalles, consulte [Credenciales de servicio para los servicios de {{site.data.keyword.watson}}](/docs/services/watson/getting-started-credentials.html#getting-credentials-manually).
1.  Asegúrese de que tenga el [tiempo de ejecución de Node.js ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://nodejs.org/#download){: new_window} instalado, incluido el gestor de paquetes de [npm ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.npmjs.com/){: new_window}. Asegúrese de incluir el mandato en la variable de entorno `PATH` después de la instalación.

## Actualice y ejecute el ejemplo

1.  Guarde una copia del código de ejemplo para el servicio de {{site.data.keyword.watson}} que desee ejecutar.
    1.  Vaya al directorio `/examples` en el [SDK del nodo ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/node-sdk/tree/master/examples){: new_window} de watson-developer-cloud.
    1.  Guarde el código de un ejemplo localmente. Utilizaremos `personality_insights.v3.js`.
1.  Vaya al directorio donde ha guardado el código e instale el SDK de Node.js:

    ```bash
    npm install watson-developer-cloud --save
    ```
    {: pre}

1.  Actualice el código de ejemplo para utilizar las credenciales de servicio (api_key o username y password):

    ```javascript
    const personality_insights = new PersonalityInsightsV3({
      username: 'INSERT YOUR USERNAME FOR THE SERVICE HERE',
      password: 'INSERT YOUR PASSWORD FOR THE SERVICE HERE',
      version_date: '2016-10-19'
    });
    ```
    {: codeblock}

    Puede que tenga que realizar otros cambios. Por ejemplo, añada aquí texto suficiente para que lo analice {{site.data.keyword.personalityinsightsshort}}.

1.  Emita el mandato `node {example_application.vn}.js` para ejecutar el código. Por ejemplo:

    ```bash
    node personality_insights.v3.js
    ```
    {: pre}

    La respuesta incluye información del servicio. Por ejemplo, con el servicio de {{site.data.keyword.personalityinsightsshort}}, verá una salida similar a este ejemplo truncado:

    ```json
    {
      "word_count": 224,
      "word_count_message": "There were 224 words in the input. We need a minimum of 600, preferably 1,200 or more, to compute statistically significant estimates",
      "processed_lang": "en",
      "personality": [
      {
        "trait_id": "big5_openness",
        "name": "Openness",
        "category": "personality",
        "percentile": 0.9015311332932557,
        "children": [. . .]
      },
      . . .
      ],
      "needs": [. . .],
      "values": [. . .],
      "consumption_preferences": [. . .]
    }
    ```
    {: codeblock}
