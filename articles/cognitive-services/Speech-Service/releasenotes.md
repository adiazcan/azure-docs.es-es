---
title: 'Notas de la versión: servicio de voz'
titlesuffix: Azure Cognitive Services
description: Consulte un registro de características, mejoras, correcciones de errores y problemas conocidos para los servicios de voz de Azure.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: wolfma
ms.custom: seodec18
ms.openlocfilehash: 7485ca1e4b1143ed46c9b3bef9ca66af0638b4f8
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53599423"
---
# <a name="release-notes"></a>Notas de la versión

## <a name="speech-sdk-120-2018-december-release"></a>Speech SDK 1.2.0: Versión de diciembre de 2018

**Nuevas características**

* Python
  * La versión beta de la compatibilidad con Python (3.5 y versiones posteriores) está disponible con esta versión. Para más información, [consulte aquí](quickstart-python.md).
* JavaScript
  * Speech SDK para JavaScript ha sido de código abierto. El código fuente está disponible en [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js).
  * Ya se admite Node.js; puede encontrar más información [aquí](quickstart-js-node.md).
  * Se quitó la restricción de longitud para las sesiones de audio; la reconexión se realizará automáticamente en la portada.
* Objeto de conexión
  * Desde el reconocedor puede tener acceso a un objeto de conexión. Este objeto le permite iniciar la conexión al servicio y suscribirse para conectar y desconectar eventos explícitamente.
    (Esta opción no está disponible en JavaScript ni Python).
* Compatibilidad con Ubuntu 18.04.
* Android
  * Compatibilidad con ProGuard habilitada durante la generación del APK.

**Mejoras**

* Mejoras en el uso de subprocesos internos, lo que reduce el número de subprocesos, bloqueos y exclusiones mutuas.
* Se mejoraron los informes de errores y la información. En algunos casos, los mensajes de error no se propagan totalmente.
* Se actualizaron las dependencias de desarrollo en JavaScript para usar los módulos actualizados.

**Correcciones de errores**

* Fugas de memoria fijas debido a un error de coincidencia de tipos en RecognizeAsync.
* En algunos casos, se perdieron excepciones.
* Corrección de las fugas de memoria en los argumentos de eventos de traducción.
* Se ha corregido un problema de bloqueo al volver a conectar en sesiones de larga ejecución.
* Se ha corregido un problema que podría dar lugar a que faltase el resultado final para las traducciones con errores.
* C#: Si no se esperaba una operación asincrónica en el subproceso principal, es posible que se pudiese desechar el reconocedor antes de completarse la tarea asincrónica.
* Java: Se ha corregido un problema que provocaba un bloqueo de la VM de Java.
* Objective-C: Se ha corregido la asignación fija; se devolvió RecognizedIntent en lugar de RecognizingIntent.
* JavaScript: Se ha establecido el formato de salida predeterminado en "simple" en SpeechConfig.
* JavaScript: Se ha quitado una incoherencia entre las propiedades del objeto de configuración en JavaScript y otros lenguajes.

**Muestras**

* Se han actualizado y corregido varios ejemplos, como las voces de salida para la traducción, etc.
* Se han agregado ejemplos de Node.js en el [repositorio de ejemplo](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-110"></a>Speech SDK 1.1.0

**Nuevas características**

* Compatibilidad con Android x86/x64.
* Compatibilidad con proxy: en el objeto SpeechConfig ahora puede llamar a una función para establecer la información del proxy (nombre de host, puerto, nombre de usuario y contraseña). Esta característica no está disponible aún en iOS.
* Mensajes y códigos de error mejorados. Si un reconocimiento devolvió un error, esto ya ha establecido `Reason` (en el evento cancelado) o `CancellationDetails` (en el resultado del reconocimiento) en `Error`. El evento cancelado ahora contiene dos miembros adicionales, `ErrorCode` y `ErrorDetails`. Si el servidor devolvió información de error adicional con el error notificado, ahora estará disponible en los nuevos miembros.

**Mejoras**

* Verificación adicional agregada en la configuración del reconocedor y mensaje de error adicional agregado.
* Control mejorado del silencio prolongado en medio de un archivo de audio.
* Paquete NuGet: para proyectos de .NET Framework, evita la compilación con la configuración de AnyCPU.

**Correcciones de errores**

* En los reconocedores se han encontrado varias excepciones corregidas. Además, las excepciones se detectan y se convierten en un evento cancelado.
* Corrección de una fuga de memoria en la administración de propiedades.
* Se corrigió el error en el que un archivo de entrada de audio podría bloquear el reconocedor.
* Se corrigió un error donde se podrían recibir eventos después de un evento de detención de la sesión.
* Se corrigieron algunas condiciones de subprocesos.
* Se corrigió un problema de compatibilidad de iOS que podría dar lugar a un bloqueo.
* Mejoras de estabilidad para la compatibilidad del micrófono en Android.
* Se corrigió un error donde un reconocedor en JavaScript ignoraría el lenguaje de reconocimiento.
* Se corrigió un error que impide establecer el valor EndpointId (en algunos casos) en JavaScript.
* Se cambió el orden de los parámetros en AddIntent en JavaScript y se agregó la firma de AddIntent en JavaScript.

**Muestras**

* Se ha agregado un ejemplo de C++ y C# sobre el uso de transmisiones de inserción y extracción en el [repositorio de ejemplos](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Speech SDK 1.0.1

Mejoras en la confiabilidad y correcciones de errores:

* Corrección de un potencial error grave debido a una condición de carrera al desechar un reconocedor
* Corrección de un potencial error grave en el caso de propiedades sin establecer.
* Comprobación adicional de errores y parámetros.
* Objective-C: corrección de posibles errores graves causados por la invalidación de nombres en NSString.
* Objective-C: ajuste de visibilidad en la API
* JavaScript: corrección con respecto a los eventos y sus cargas.
* Mejoras en la documentación.

Se ha agregado un nuevo ejemplo de Javascript en nuestro [repositorio de ejemplos](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>SDK de Voz 1.0.0 de Cognitive Services: Versión de septiembre de 2018

**Nuevas características:**

* Compatibilidad con Objective-C en iOS. Consulte la [Guía de inicio rápido de Objective-C para iOS](quickstart-objectivec-ios.md).
* Se admite JavaScript en el explorador. Consulte la [Guía de inicio rápido de JavaScript](quickstart-js-browser.md).

**Cambios importantes**

* Con esta versión se presentan una serie de cambios importantes.
  Consulte [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para más información.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>SDK de Voz 0.6.0 de Cognitive Services: Versión de agosto de 2018

**Nuevas características:**

* Ahora, las aplicaciones de UWP creadas con SDK de Voz superan el Kit para la certificación de aplicaciones en Windows (WACK).
  Consulte la [Guía de inicio rápido de UWP](quickstart-csharp-uwp.md).
* Compatibilidad con .NET Standard 2.0 en Linux (Ubuntu 16.04 x64).
* Experimental: compatibilidad con Java 8 en Windows (64 bits) y Linux (Ubuntu 16.04 x 64).
  Consulte la [Guía de inicio rápido de Java Runtime Environment](quickstart-java-jre.md).

**Cambios funcionales**

* Se expone más información detallada sobre los errores de conexión.

**Cambios importantes**

* En Java (Android), la función `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` ya no requiere un parámetro de ruta de acceso. Ahora, la ruta de acceso se detecta automáticamente en todas las plataformas compatibles.
* En Java y C#, se ha quitado el descriptor de acceso get- de la propiedad `EndpointUrl`.

**Correcciones de errores**

* En Java, se implementa ahora el resultado de la síntesis de audio en el reconocedor de traducción.
* Se ha corregido un error que podía provocar subprocesos inactivos y un mayor número de sockets abiertos y sin usar.
* Se ha corregido un problema por el que un proceso de reconocimiento de larga ejecución podía terminar en mitad de la transmisión.
* Se ha corregido una condición de carrera en el proceso de apagado del reconocedor.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>SDK de Voz 0.5.0 de Cognitive Services: Versión de julio de 2018

**Nuevas características:**

* Compatibilidad con la plataforma Android (API 23: Android Marshmallow 6.0 o posterior). Consulte el [inicio rápido de Android](quickstart-java-android.md).
* Compatibilidad con .NET Standard 2.0 en Windows. Consulte el [inicio rápido de .NET Core](quickstart-csharp-dotnetcore-windows.md).
* Experimental: compatibilidad con UWP en Windows (versión 1709 o posterior).
  * Consulte la [Guía de inicio rápido de UWP](quickstart-csharp-uwp.md).
  * Nota: Las aplicaciones de UWP creadas con el SDK de Voz no pasan aún el Kit para la certificación de aplicaciones en Windows (WACK).
* Compatibilidad con el reconocimiento de ejecución prolongada con reconexión automática.

**Cambios funcionales**

* `StartContinuousRecognitionAsync()` admite reconocimiento de ejecución prolongada.
* El resultado del reconocimiento contiene más campos. Tienen un desplazamiento desde el principio del audio y la duración (ambos en tics) del texto reconocido y valores adicionales que representan el estado de reconocimiento, por ejemplo, `InitialSilenceTimeout` e `InitialBabbleTimeout`.
* Compatibilidad con AuthorizationToken para la creación de instancias de fábrica.

**Cambios importantes**

* Eventos de reconocimiento: el tipo de evento NoMatch se combina con el evento Error.
* SpeechOutputFormat en C# se llama ahora OutputFormat para concordar con C++.
* El tipo de valor devuelto de algunos métodos de la interfaz `AudioInputStream` se ha modificado ligeramente:
   * En Java, el método `read` ahora devuelve `long` en lugar de `int`.
   * En C#, el método `Read` ahora devuelve `uint` en lugar de `int`.
   * En C++, los métodos `Read` y `GetFormat` ahora devuelven `size_t` en lugar de `int`.
* C++: las instancias de secuencias de entrada de audio ahora solo se pueden pasar como un valor `shared_ptr`.

**Correcciones de errores**

* Se han corregido los valores devueltos incorrectos cuando se agota el tiempo de espera de `RecognizeAsync()`.
* Se ha eliminado la dependencia de las bibliotecas de Media Foundation en Windows. El SDK ahora usa las API de audio básicas.
* Corrección de la documentación: se ha agregado una página de [regiones](regions.md) para describir cuáles son las regiones admitidas.

**Problema conocido**

* SDK de Voz para Android no informa de los resultados de la síntesis de voz para la traducción. Este problema se solucionará en la próxima versión.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>SDK de Voz 0.4.0 de Cognitive Services: Versión de junio de 2018

**Cambios funcionales**

- AudioInputStream

  Un reconocedor ahora puede consumir una secuencia como origen de audio. Para más información, consulte la [guía de procedimientos](how-to-use-audio-input-streams.md) relacionada.

- Formato de salida detallado

  Al crear un elemento `SpeechRecognizer`, puede solicitar el formato de salida `Detailed` o `Simple`. `DetailedSpeechRecognitionResult` contiene una puntuación de confianza, texto reconocido, formato léxico sin formato, formato normalizado y formato normalizado con palabras soeces enmascaradas.

**Cambio importante**

- En C# se cambia de `SpeechRecognitionResult.RecognizedText` a `SpeechRecognitionResult.Text`.

**Correcciones de errores**

- Se ha corregido un posible problema de devolución de llamada en la capa USP durante el apagado.

- Si un reconocedor usaba un archivo de entrada de audio, mantenía el identificador de archivo más tiempo del necesario.

- Se han eliminado varios interbloqueos entre el suministro de mensajes y el reconocedor.

- Se desencadena un resultado `NoMatch` cuando se agota la respuesta del servicio.

- Las bibliotecas de Media Foundation en Windows son de carga retrasada. Esta biblioteca solo es necesaria para la entrada del micrófono.

- La velocidad de carga de los datos de audio se limita al doble de la velocidad de audio original.

- En Windows, los ensamblados .NET de C# ahora son de nombre seguro.

- Corrección de la documentación: `Region` necesita información para crear un reconocedor.

Se han agregado más ejemplos y se actualizan constantemente. Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos de SDK de Voz](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>SDK de Voz 0.2.12733 de Cognitive Services: Versión de mayo de 2018

Esta versión es la primera versión preliminar pública de SDK de Voz de Cognitive Services.
