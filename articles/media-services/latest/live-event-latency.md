---
title: Latencia de LiveEvent en Azure Media Services | Microsoft Docs
description: En este tema se proporciona información general sobre la latencia de LiveEvent y se muestra cómo establecer una latencia baja.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 12/19/2018
ms.author: juliako
ms.openlocfilehash: f4ded67ef964482a2acea0d731b1b154a95168d2
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53741358"
---
# <a name="liveevent-latency-in-media-services"></a>Latencia de LiveEvent en Media Services

En este artículo se muestra cómo establecer una latencia baja en un [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents). También se describen los resultados típicos que se ven cuando se usa la configuración de latencia baja en varios reproductores. Los resultados varían en función de la latencia de red y CDN.

Para usar la nueva característica **LowLatency**, puede establecer **StreamOptionsFlag** en **LowLatency** en **LiveEvent**. Al crear [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) para la reproducción de HLS, establezca [LiveOutput.Hls.fragmentsPerTsSegment](https://docs.microsoft.com/rest/api/media/liveoutputs/create#hls) en 1. Una vez que la transmisión esté en funcionamiento, puede usar la página de demostración de [Azure Media Player](http://ampdemo.azureedge.net/) (AMP) y establecer las opciones de reproducción para usar "Low Latency Heuristics Profile" (Perfil de heurística de baja latencia).

> [!NOTE]
> Actualmente, LowLatency HeuristicProfile en Azure Media Player está diseñado para volver a reproducir secuencias en protocolo DASH o HLS con CMAF. Si tiene como destino dispositivos iOS o MacOS a través de HLS con TS (por ejemplo, `format=m3u8-aapl` o `format=m3u8-aapl-v3`), no debe usar esta opción porque AMP usa directamente el reproductor nativo proporcionado por el sistema operativo en este caso.

El siguiente ejemplo de .NET muestra cómo establecer **LowLatency** en **LiveEvent**:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Standard to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second "Group Of Pictures" (GOP) length instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Observe el ejemplo completo: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="liveevents-latency"></a>Latencia de LiveEvents

En la tabla siguiente se muestran los resultados típicos de latencia (cuando está habilitada la marca LowLatency) en Media Services, medida desde el momento en que la fuente de contribución alcanza el servicio hasta cuando un usuario visualiza la reproducción en el reproductor. Para usar una latencia baja de forma óptima, debe ajustar la configuración del codificador a la longitud de grupo de imágenes (GOP) de 1 segundo. Cuando se usa una longitud GOP mayor, se minimiza el consumo de ancho de banda y se reduce la velocidad de bits en la misma velocidad de fotogramas. Esto resulta especialmente útil en vídeos con menos movimiento.

### <a name="pass-through"></a>Paso a través 

||Baja latencia de GOP de 2 s habilitada|Baja latencia de GOP de 1 s habilitada|
|---|---|---|
|DASH en AMP|10 s|8 s|
|HLS en el reproductor de iOS nativo|14 s|10 s|

### <a name="live-encoding"></a>Live Encoding

||Baja latencia de GOP de 2 s habilitada|Baja latencia de GOP de 1 s habilitada|
|---|---|---|
|DASH en AMP|14 s|10 s|
|HLS en el reproductor de iOS nativo|18 s|13 s|

> [!NOTE]
> Latencia de extremo a extremo puede variar según las condiciones de la red local o al introducir una capa de almacenamiento en caché de CDN. Debe probar las configuraciones exactas.

## <a name="next-steps"></a>Pasos siguientes

- [Introducción al streaming en vivo](live-streaming-overview.md)
- [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)

