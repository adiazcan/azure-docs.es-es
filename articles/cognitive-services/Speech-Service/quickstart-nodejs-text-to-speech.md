---
title: 'Inicio rápido: Conversión de texto a voz, Node.js: servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, obtendrá información sobre cómo convertir texto a voz con Node.js y la API REST Texto a voz. El texto de ejemplo incluido en esta guía se estructura como lenguaje de marcado de síntesis de voz (SSML). Esto le permite elegir la voz y el idioma de la respuesta de voz.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 8cc676355f432a25550b337e5de747c4956d3142
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358888"
---
# <a name="quickstart-convert-text-to-speech-using-nodejs"></a>Inicio rápido: Conversión de texto a voz con Node.js

En este tutorial, obtendrá información sobre cómo convertir texto a voz con Node.js y la API REST Texto a voz. El cuerpo de solicitud de esta guía se estructura como [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md), que permite elegir la voz y el idioma de la respuesta.

En esta guía de inicio rápido, se requiere una [cuenta de Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con un recurso del servicio Voz. Si no tiene una cuenta, puede usar la [evaluación gratuita](get-started.md) para obtener una clave de suscripción.

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Node 8.12.x o posterior](https://nodejs.org/en/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) o su editor favorito de código
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-project-and-require-dependencies"></a>Creación de un proyecto y requerimiento de dependencias

Cree un proyecto de Node.js mediante su IDE o editor favorito. A continuación, copie este fragmento de código en un archivo llamado `tts.js`.

```javascript
// Requires request for HTTP requests
const request = require('request');
// Requires fs to write synthesized speech to a file
const fs = require('fs');
// Requires readline-sync to read command line inputs
const readline = require('readline-sync');
// Requires xmlbuilder to build the SSML body
const xmlbuilder = require('xmlbuilder');
```

> [!NOTE]
> Si no ha usado estos módulos deberá instalarlos antes de ejecutar el programa. Para instalar estos paquetes, ejecute: `npm install request readline-sync`.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Establecimiento de la clave de suscripción y creación de un símbolo del sistema para TTS

En las siguientes secciones podrá crear funciones para controlar la autorización, llamar a la API Texto a voz y validar la respuesta. Comencemos agregando una clave de suscripción y creando un símbolo del sistema de entrada de texto.

```javascript
/*
 * These lines will attempt to read your subscription key from an environment
 * variable. If you prefer to hardcode the subscription key for ease of use,
 * replace process.env.SUBSCRIPTION_KEY with your subscription key as a string.  
 */
const subscriptionKey = process.env.SUBSCRIPTION_KEY;
if (!subscriptionKey) {
  throw new Error('Environment variable for your subscription key is not set.')
};

// Prompts the user to input text.
let text = readline.question('What would you like to convert to speech? ');
```

## <a name="get-an-access-token"></a>Obtención de un token de acceso

La API REST Texto a voz requiere un token de acceso para la autenticación. Para obtener un token de acceso, es necesario un intercambio. En este ejemplo se intercambia la clave de suscripción de Speech Service para obtener un token de acceso usando el punto de conexión `issueToken`.

Esta función toma dos argumentos, la clave de suscripción de servicios de voz y una función de devolución de llamada. Después de que la función haya obtenido un token de acceso, pasa el valor a la función de devolución de llamada. En la siguiente sección, vamos a crear la función para llamar a la API de texto a voz y guardar la respuesta de voz sintetizada.

En este ejemplo se da por supuesto que su suscripción de Speech Service está en la región Oeste de EE. UU. Si usa una región diferente, actualice el valor de `uri`. Para obtener una lista completa, consulte [Regiones](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Copie este código en el proyecto:

```javascript
function textToSpeech(subscriptionKey, saveAudio) {
    let options = {
        method: 'POST',
        uri: 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken',
        headers: {
            'Ocp-Apim-Subscription-Key': subscriptionKey
        }
    };
    // This function retrieve the access token and is passed as callback
    // to request below.
    function getToken(error, response, body) {
        console.log("Getting your token...\n")
        if (!error && response.statusCode == 200) {
            //This is the callback to our saveAudio function.
            // It takes a single argument, which is the returned accessToken.
            saveAudio(body)
        }
        else {
          throw new Error(error);
        }
    }
    request(options, getToken)
}
```

> [!NOTE]
> Para obtener más información, vea [Autenticación con un token de autenticación](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Realización de solicitud y guardado de la respuesta

Aquí va a generar la solicitud a la API de texto a voz y guardar la respuesta de voz. En este ejemplo se da por hecho que está utilizando el punto de conexión de Oeste de EE. UU. Si el recurso está registrado en una región diferente, asegúrese de actualizar `uri`. Para obtener más información, consulte [Regiones del servicio Voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

A continuación, deberá agregar los encabezados necesarios para la solicitud. Asegúrese de actualizar `User-Agent` con el nombre del recurso (que se encuentra en Azure Portal) y establezca `X-Microsoft-OutputFormat` en la salida de audio preferida. Para obtener una lista completa de los formatos de salida, vea [Salidas de Audio](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#audio-outputs).

A continuación, cree el cuerpo de solicitud mediante el lenguaje de marcado de síntesis de voz (SSML). Este ejemplo define la estructura y usa la entrada `text` que creó anteriormente.

>[!NOTE]
> Este ejemplo se utiliza la fuente de voz `JessaRUS`. Para obtener una lista completa de los idiomas y las voces que proporciona Microsoft, consulte [Compatibilidad con idiomas](language-support.md).
> Si está interesado en crear una voz única y reconocible para su marca, vea [Crear fuentes de voz personalizada](how-to-customize-voice-font.md).

Por último, deberá realizar una solicitud al servicio. Si la solicitud es correcta y se devuelve un código de estado 200, la respuesta de voz se escribe como `sample.wav`.

```javascript
// Make sure to update User-Agent with the name of your resource.
// You can also change the voice and output formats. See:
// https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech
function saveAudio(accessToken) {
    // Create the SSML request.
    let xml_body = xmlbuilder.create('speak')
      .att('version', '1.0')
      .att('xml:lang', 'en-us')
      .ele('voice')
      .att('xml:lang', 'en-us')
      .att('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
      .txt(text)
      .end();
    // Convert the XML into a string to send in the TTS request.
    let body = xml_body.toString();

    let options = {
        method: 'POST',
        baseUrl: 'https://westus.tts.speech.microsoft.com/',
        url: 'cognitiveservices/v1',
        headers: {
            'Authorization': 'Bearer ' + accessToken,
            'cache-control': 'no-cache',
            'User-Agent': 'YOUR_RESOURCE_NAME',
            'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
            'Content-Type': 'application/ssml+xml'
        },
        body: body
    };
    // This function makes the request to convert speech to text.
    // The speech is returned as the response.
    function convertText(error, response, body){
      if (!error && response.statusCode == 200) {
        console.log("Converting text-to-speech. Please hold...\n")
      }
      else {
        throw new Error(error);
      }
      console.log("Your file is ready.\n")
    }
    // Pipe the response to file.
    request(options, convertText).pipe(fs.createWriteStream('sample.wav'));
}
```

## <a name="put-it-all-together"></a>Colocación de todo junto

Ya casi ha terminado. El último paso consiste en llamar a la función `textToSpeech`.

```javascript
// Start the sample app.
textToSpeech(subscriptionKey, saveAudio);
```

## <a name="run-the-sample-app"></a>Ejecutar la aplicación de ejemplo

Eso es todo; ya está listo para ejecutar la aplicación de ejemplo de Texto a voz. Desde la línea de comandos (o sesión de terminal), vaya al directorio del proyecto y ejecute:

```console
node tts.js
```

Cuando se le solicite, escriba todo lo que gustaría convertir de texto a voz. Si se realiza correctamente, el archivo se ubicará en la carpeta del proyecto. Reprodúzcalo con el reproductor que prefiera.

## <a name="clean-up-resources"></a>Limpieza de recursos

Asegúrese de quitar cualquier información confidencial del código fuente de la aplicación de ejemplo como, por ejemplo, las claves de suscripción.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Explorar ejemplos de Node.js en GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/NodeJS)

## <a name="see-also"></a>Otras referencias

* [Referencia de Text-to-speech API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis#text-to-speech-api)
* [Crear fuentes de voz personalizada](how-to-customize-voice-font.md)
* [Grabación de muestras de voz para crear una voz personalizada](record-custom-voice-samples.md)
