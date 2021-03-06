---
title: 'Asociación de un recurso de Cognitive Services con un conjunto de aptitudes: Azure Search'
description: Instrucciones para asociar una suscripción todo en uno de Cognitive Services a una canalización de enriquecimiento cognitivo en Azure Search.
manager: cgronlun
author: LuisCabrer
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 01/07/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 509125e7c93f34b9ce28c58cb1ec96db1074d995
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54119652"
---
# <a name="associate-a-cognitive-services-resource-with-a-skillset-in-azure-search"></a>Asociación de un recurso de Cognitive Services con un conjunto de aptitudes en Azure Search 

Los algoritmos de IA que impulsan las directivas de [Cognitive Search](cognitive-search-concept-intro.md) para el procesamiento de datos no estructurados se basan en [**recursos de Cognitive Services**](https://azure.microsoft.com/services/cognitive-services/). Recursos como [**Computer Vision**](https://azure.microsoft.com/services/cognitive-services/computer-vision/) ofrecen análisis de imagen y reconocimiento óptico de caracteres (OCR) que extraen texto y estructuras de los archivos de imágenes, mientras que [**Text Analytics**](https://azure.microsoft.com/services/cognitive-services/text-analytics/) ofrece un procesamiento de lenguaje natural como el reconocimiento de entidades y la extracción de frases clave, por nombrar algunos.

Puede enriquecer un número limitado de documentos de manera gratuita, o bien puede asociar un recurso de Cognitive Services facturable para cargas de trabajo más grandes y más frecuentes. En este artículo, aprenderá a asociar un recurso de Cognitive Services con el conjunto de aptitudes cognitivas para enriquecer los datos durante el indexado.

Si la canalización consta exclusivamente de [aptitudes personalizadas](cognitive-search-create-custom-skill-example.md), no es necesario asociar un recurso de Cognitive Services.

> [!NOTE]
> A partir del 21 de diciembre de 2018, podrá asociar un recurso de Cognitive Services con un conjunto de aptitudes de Azure Search. Esto nos permite cobrar por la ejecución del conjunto de aptitudes. En esta fecha, también empezamos a cobrar por la extracción de imágenes como parte de la fase de descifrado de documentos. La extracción de texto de documentos continuará ofreciéndose sin costo adicional.
>
> La ejecución de [aptitudes cognitivas integradas](cognitive-search-predefined-skills.md) se carga en el [precio de pago por uso de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/) con la misma tarifa que tendría si hubiese realizado la tarea independientemente de Azure Search. La extracción de imágenes se cobra al precio de la versión preliminar, tal y como se describe en la [página de precios de Azure Search](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="use-free-resources"></a>Uso de recursos gratis

Puede usar una opción de procesamiento gratis y limitada que le da derecho a 20 enriquecimientos de documentos de manera diaria, lo que basta para completar el tutorial de Cognitive Search y los ejercicios de inicio rápido. 

> [!Important]
> A partir del 1 de febrero de 2019, la opción **Gratis (enriquecimientos limitados)** queda restringida a 20 documentos al día. 

1. Abra el Asistente para la **importación de datos**.

   ![Comando de importación de datos](media/search-get-started-portal/import-data-cmd2.png "Comando de importación de datos")

1. Elija un origen de datos y siga a **Agregar Cognitive Search (opcional)**. Para ver un tutorial paso a paso de este asistente, consulte el artículo sobre cómo [importar, indexar y consultar con las herramientas del portal](search-get-started-portal.md).

1. Expanda **Adjuntar Cognitive Services** y seleccione **Gratis (enriquecimientos limitados)**.

   ![Sección Adjuntar Cognitive Services expandida](./media/cognitive-search-attach-cognitive-services/attach1.png "Expanded Attach Cognitive Services")

Vaya al paso siguiente, **Agregar enriquecimientos**. Para una descripción de las aptitudes disponibles en el portal, consulte ["Paso 2: Agregar conocimientos cognitivos"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="use-billable-resources"></a>Uso de recursos facturables

Para cargas de trabajo que superen los 20 documentos diarios, va a necesitar un recurso facturable de Cognitive Services.

1. En el Asistente para la **importación de datos** de **Adjuntar Cognitive Services**, seleccione un recurso existente o haga clic en **Crear nuevo recurso de Cognitive Services**.

1. Para **Crear nuevo recurso de Cognitive Services**, se abre una pestaña nueva para que pueda crear el recurso. Asigne un nombre único al recurso.

1. Elija la misma ubicación de Azure Search. Actualmente, el indexado de las aptitudes cognitivas se admite en las regiones siguientes:

  * Centro occidental de EE.UU.
  * Centro-Sur de EE. UU
  * Este de EE. UU
  * Este de EE. UU. 2
  * Oeste de EE. UU. 2
  * Centro de Canadá
  * Europa occidental
  * Sur de Reino Unido 2
  * Europa del Norte
  * Sur de Brasil
  * Sudeste asiático
  * India Central
  * Este de Australia

1. Elija el plan de tarifa Todo en uno, **S0**. Este plan proporciona las características de visión y lenguaje que respaldan las aptitudes predefinidas en Cognitive Search.

    ![Crear un nuevo recurso de Cognitive Services](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Crear un nuevo recurso de Cognitive Services")

1. Haga clic en **Crear** para aprovisionar el nuevo recurso de Cognitive Services. 

1. Vuelva a la pestaña anterior donde se encuentra el Asistente para la **importación de datos**. Haga clic en **Actualizar** para mostrar el recurso de Cognitive Services y, luego, seleccione el recurso.

   ![Recurso de Cognitive Services seleccionado](./media/cognitive-search-attach-cognitive-services/attach2.png "Selected Cognitive Service Resource")

1. Expanda la sección **Agregar enriquecimiento** para seleccionar las aptitudes cognitivas específicas que desea ejecutar en los datos y continuar con el resto del flujo. Para una descripción de las aptitudes disponibles en el portal, consulte ["Paso 2: Agregar conocimientos cognitivos"](cognitive-search-quickstart-blob.md#create-the-enrichment-pipeline) en el inicio rápido de Cognitive Search.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Asociación de un conjunto de aptitudes existentes con un recurso de Cognitive Services

Si ya tiene un conjunto de aptitudes, puede asociarlo a un recurso de Cognitive Services nuevo o distinto.

1. En la página **Service overview** (Introducción al servicio), haga clic en **Conjuntos de aptitudes**.

   ![Pestaña Conjuntos de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Skillsets tab")

1. Haga clic en el nombre del conjunto de aptitudes y seleccione un recurso existente o cree uno nuevo. Haga clic en **Aceptar** para confirmar los cambios. 

   ![Lista de recursos del conjunto de aptitudes](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lista de recursos del conjunto de aptitudes")

Recuerde que **Gratis (enriquecimientos limitados)** se limita a 20 documentos diarios y que **Crear nuevo recurso de Cognitive Services** se usa para aprovisionar un recurso facturable nuevo. Si crea un nuevo recurso, seleccione **Actualizar** para actualizar la lista de recursos de Cognitive Services y, a continuación, seleccione el recurso.

## <a name="attach-cognitive-services-programmatically"></a>Asociación de Cognitive Services mediante programación

Cuando defina un conjunto de aptitudes mediante programación, agregue una sección `cognitiveServices` al conjunto de aptitudes. En la sección, incluya la clave del recurso de Cognitive Services que desea asociar con el conjunto de aptitudes. Recuerde que el recurso debe estar en la misma región que Azure Search. Además incluya `@odata.type` y establézcalo en `#Microsoft.Azure.Search.CognitiveServicesByKey`. 

En el siguiente ejemplo se muestra este patrón. Observe la sección cognitiveServices que está en la parte inferior de la definición.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2017-11-11-Preview
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Ejemplo: cálculo de los costos

Para calcular los costos asociados con el indexado de Cognitive Search, empiece con una idea de cómo debe verse un documento promedio para que pueda ejecutar algunos números. Por ejemplo, para realizar el cálculo, podría tomar como un cálculo aproximado:

+ 1000 archivos PDF
+ Seis páginas cada uno
+ Una imagen por página (6000 imágenes)
+ 3000 caracteres por página

Supongamos que hay una canalización que consta del descifrado de cada documento PDF con extracción de imágenes y texto, reconocimiento óptico de caracteres (OCR) de las imágenes y reconocimiento de entidad con nombre de las organizaciones. 

En este ejercicio, usamos el mayor precio por transacción. Los costos reales podrían ser menores debido a los precios escalonados. Consulte los [precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services).

1. Para el descifrado de documentos con contenido de texto e imagen, la extracción de texto actualmente es gratis. Para 6000 imágenes, suponga 1 USD por cada 1000 imágenes extraídas, con un costo de 6 USD por este paso.

2. Para realizar el reconocimiento óptico de caracteres (OCR) de 6000 imágenes en inglés, la aptitud de reconocimiento de OCR usa el mejor algoritmo (DescribeText). Suponiendo un precio de 2,50 USD por cada 1000 imágenes que se analizan, tendríamos que pagar 15,00 USD en este paso.

3. Para la extracción de entidades, tendríamos un total de 3 registros de texto por página. Cada registro tiene 1000 caracteres. Tres registros de texto por página * 6000 páginas = 18 000 registros de texto. Suponiendo un costo de 2,00 USD por cada 1,000 registros de texto, en este paso tendríamos un costo de 36,00 USD.

En resumen, pagaría unos 57 USD por la ingesta de 1000 documentos PDF de esta naturaleza con el conjunto de aptitudes descrito. 

## <a name="next-steps"></a>Pasos siguientes
+ [Página de precios de Azure Search](https://azure.microsoft.com/pricing/details/search/)
+ [Definición de un conjunto de aptitudes](cognitive-search-defining-skillset.md)
+ [Crear un conjunto de aptitudes (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Cómo asignar campos enriquecidos](cognitive-search-output-field-mapping.md)
