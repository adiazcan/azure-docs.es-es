---
title: " Compilación de una aplicación web de página única - Bing Visual Search"
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo integrar Bing Visual Search API en una aplicación web de página única.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 10/04/2017
ms.author: aahi
ms.openlocfilehash: 8ff5e36e6189c522e00c7cdd126c26b1cef92912
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53745149"
---
# <a name="create-a-visual-search-single-page-web-app"></a>Creación de una aplicación web de página única de Visual Search 

Bing Visual Search API proporciona una experiencia similar a los detalles de imagen que se muestran en Bing.com/images. Visual Search permite especificar una imagen y obtener conclusiones sobre la imagen, como otras imágenes visualmente similares, orígenes de compras, páginas web que incluyen la imagen, etc. 

En este artículo se explica cómo extender una aplicación web de página única para Bing Image Search API. Para ver ese tutorial u obtener el código fuente que se usa aquí, consulte [Tutorial: Create a single-page app for the Bing Image Search API](../Bing-Image-Search/tutorial-bing-image-search-single-page-app.md) (Crear una aplicación de página única para Bing Image Search API). 

El código fuente completo para esta aplicación (después de extenderla para usar Bing Visual Search API) está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchApp.html).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="call-the-bing-visual-search-api-and-handle-the-response"></a>Llamar Bing Visual Search API y gestionar la respuesta

Edite el tutorial de Bing Image Search y agregue el código siguiente al final del elemento `<script>` (y antes de la etiqueta `</script>` de cierre). El código siguiente gestiona una respuesta de Visual Search de la API, recorre en iteración los resultados y los muestra.

``` javascript
function handleVisualSearchResponse(){
    if(this.status !== 200){
        console.log(this.responseText);
        return;
    }
    let json = this.responseText;
    let response = JSON.parse(json);
    for (let i =0; i < response.tags.length; i++) {
        let tag = response.tags[i];
        if (tag.displayName === '') {
            for (let j = 0; j < tag.actions.length; j++) {
                let action = tag.actions[j];
                if (action.actionType === 'VisualSearch') {
                    let html = '';
                    for (let k = 0; k < action.data.value.length; k++) {
                        let item = action.data.value[k];
                        let height = 120;
                        let width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
                        html += "<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + "' height=" + height + " width=" + width + "'>";
                    }
                    showDiv("insights", html);
                    window.scrollTo({top: document.getElementById("insights").getBoundingClientRect().top, behavior: "smooth"});
                }
            }
        }
    }
}
```

El código siguiente envía una solicitud de búsqueda a la API mediante un escucha de eventos para llamar a `handleVisualSearchResponse()`.


```javascript
function bingVisualSearch(insightsToken){
    let visualSearchBaseURL = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch',
        boundary = 'boundary_ABC123DEF456',
        startBoundary = '--' + boundary,
        endBoundary = '--' + boundary + '--',
        newLine = "\r\n",
        bodyHeader = 'Content-Disposition: form-data; name="knowledgeRequest"' + newLine + newLine;

    let postBody = {
        imageInfo: {
            imageInsightsToken: insightsToken
        }
    }
    let requestBody = startBoundary + newLine;
    requestBody += bodyHeader;
    requestBody += JSON.stringify(postBody) + newLine + newLine;
    requestBody += endBoundary + newLine;       
    
    let request = new XMLHttpRequest();

    try {
        request.open("POST", visualSearchBaseURL);
    } 
    catch (e) {
        renderErrorMessage("Error performing visual search: " + e.message);
    }
    request.setRequestHeader("Ocp-Apim-Subscription-Key", getSubscriptionKey());
    request.setRequestHeader("Content-Type", "multipart/form-data; boundary=" + boundary);
    request.addEventListener("load", handleVisualSearchResponse);
    request.send(requestBody);
}
```

## <a name="capture-insights-token"></a>Capturar el token de conclusiones

Agregue el código siguiente en el objeto `searchItemsRenderer`. Este código agrega un vínculo **buscar similares** que llama a la función `bingVisualSearch` al hacer clic en él. La función recibe imageInsightsToken como un argumento.

``` javascript
html.push("<a href='javascript:bingVisualSearch(\"" + item.imageInsightsToken + "\");'>find similar</a><br>");
```

## <a name="display-similar-images"></a>Mostrar imágenes similares

Agregue el código HTML siguiente en la línea 601. Este código de marcado agrega un elemento que se usa para mostrar los resultados de la llamada a Bing Visual Search API.

``` html
<div id="insights">
    <h3><a href="#" onclick="return toggleDisplay('_insights')">Similar</a></h3>
    <div id="_insights" style="display: none"></div>
</div>
```

Con el nuevo código JavaScript y los elementos HTML implementados, los resultados de la búsqueda se muestran con un vínculo **buscar similares**. Haga clic en el vínculo para rellenar la sección **Similares** con imágenes similares a la que ha seleccionado. Puede que tenga que expandir la sección **Similares** para mostrar las imágenes.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Recorte y carga de una imagen](tutorial-visual-search-crop-area-results.md)