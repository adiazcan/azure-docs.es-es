---
title: 'Inicio rápido: Obtención de información sobre imágenes con el SDK de Bing Visual Search para C#'
titleSuffix: Azure Cognitive Services
description: Aprenda a cargar una imagen con el SDK de Bing Visual Search y a obtener conclusiones sobre ella.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: quickstart
ms.date: 05/16/2018
ms.author: v-gedod
ms.openlocfilehash: c589cc14fd429385dd47aad489e827804916d406
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53741226"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-c"></a>Inicio rápido: Obtención de información sobre imágenes con el SDK de Bing Visual Search para C#

Use este artículo de inicio rápido para empezar a obtener información sobre las imágenes desde el servicio Bing Visual Search con el SDK de C#. Aunque Bing Visual Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK ofrece una forma fácil de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingVisualSearch).

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](http://www.mono-project.com/).
* El paquete NuGet para Visual Search. 
    - En el Explorador de soluciones en Visual Studio, haga clic con el botón derecho en el proyecto y seleccione `Manage NuGet Packages` en el menú. Instale el paquete `Microsoft.Azure.CognitiveServices.Search.CustomSearch`. La instalación de los paquetes NuGet también instala lo siguiente:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

<a name="client"></a>

## <a name="create-and-initialize-the-application"></a>Creación e inicialización de la aplicación

1. En Visual Studio, cree un nuevo proyecto. A continuación, agregue las siguientes directivas.
    
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
    using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
    ```

2. Cree instancias del cliente con la clave de suscripción.
    
    ```csharp
    var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));
    ```
    
## <a name="send-a-search-request"></a>Envío de una solicitud de búsqueda 

1. Cree un `FileStream` para las imágenes (en este caso `TestImages/image.jpg`). Después, use el cliente para enviar una solicitud de búsqueda con `client.Images.VisualSearchMethodAsync()`. 
    
    ```csharp
     System.IO.FileStream stream = new FileStream(Path.Combine("TestImages", "image.jpg"), FileMode.Open;
     // The knowledgeRequest parameter is not required if an image binary is passed in the request body
     var visualSearchResults = client.Images.VisualSearchMethodAsync(image: stream, knowledgeRequest: (string)null).Result;
    ```
    
2. Analice los resultados de la consulta anterior:

    ```csharp
    // Visual Search results
    if (visualSearchResults.Image?.ImageInsightsToken != null)
    {
        Console.WriteLine($"Uploaded image insights token: {visualSearchResults.Image.ImageInsightsToken}");
    }
    else
    {
        Console.WriteLine("Couldn't find image insights token!");
    }
    
    // List of tags
    if (visualSearchResults.Tags.Count > 0)
    {
        var firstTagResult = visualSearchResults.Tags.First();
        Console.WriteLine($"Visual search tag count: {visualSearchResults.Tags.Count}");
    
        // List of actions in first tag
        if (firstTagResult.Actions.Count > 0)
        {
            var firstActionResult = firstTagResult.Actions.First();
            Console.WriteLine($"First tag action count: {firstTagResult.Actions.Count}");
            Console.WriteLine($"First tag action type: {firstActionResult.ActionType}");
        }
        else
        {
            Console.WriteLine("Couldn't find tag actions!");
        }
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Compilación de una aplicación web de una sola página](tutorial-bing-visual-search-single-page-app.md)
