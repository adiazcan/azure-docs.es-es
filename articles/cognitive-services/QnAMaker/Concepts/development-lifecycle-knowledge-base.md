---
title: 'Ciclo de vida de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 3fa6709292c0e099285014f11872008756f2c5f2
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263261"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Ciclo de vida de una base de conocimiento de QnA Maker
QnA Maker aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión. 

![Ciclo de creación](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Creación de una base de conocimiento de QnA Maker
El punto de conexión de la base de conocimiento de QnA Maker proporciona una respuesta mejor para las consultas de usuario basadas en el contenido de la base de conocimiento. Crear una base de conocimiento es una acción que se realiza una vez para configurar un repositorio de contenido de preguntas, respuestas y los metadatos asociados. Para crear una base de datos de conocimiento, se puede rastrear el contenido existente, como páginas de preguntas más frecuentes, manuales de productos o pares de pregunta-respuesta estructurados. Aprenda cómo [crear una base de conocimiento](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Probar y actualizar la base de conocimiento
La base de conocimiento está lista para probarla una vez que se llena con el contenido, bien editorialmente o mediante extracción automática. La prueba se puede realizar en el panel **Test** (Prueba). Para ello, se especifican consultas comunes de usuario y se comprueba que las respuestas devueltas son las previstas y la puntuación de la confianza es suficiente. Puede agregar preguntas alternativas para corregir las puntuaciones de confianza bajas. También puede agregar nuevas respuestas cuando una consulta devuelve la respuesta predeterminada "no se encontraron coincidencias en la base de conocimiento". Este bucle ajustado de prueba-actualización continúa hasta que esté satisfecho con los resultados. Vea cómo [probar la base de conocimiento](../How-To/test-knowledge-base.md).

Para bases de conocimiento grandes, las pruebas se pueden automatizar con generateAnswer API. 

## <a name="publish-the-knowledge-base"></a>Publicación de una base de conocimiento
Cuando haya terminado de probar la base de conocimiento, puede publicarla. Al publicar, se inserta la versión más reciente de la base de conocimiento probada en un índice de Azure Search dedicado que representa la base de conocimiento **publicada**. También se crea un punto de conexión al que puede llamar en su aplicación o bot de chat.

De esta manera, los cambios realizados en la versión de prueba de la base de conocimiento no afectan a la versión publicada que podría estar activa en una aplicación de producción.

Cada una de estas bases de conocimiento se pueden probar por separado. Con las API, puede dirigirse a la versión de prueba de la base de conocimiento con la marca `isTest=true` en la llamada de generateAnswer.

Vea cómo [publicar la base de conocimiento](../How-To/publish-knowledge-base.md).

## <a name="monitor-usage"></a>Supervisión de uso
Para poder registrar los registros de chat de su servicio, deberá habilitar Application Insights cuando [cree su servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

Puede obtener diversos datos de análisis de su uso del servicio. Obtenga más información sobre cómo utilizar Application Insights para obtener [datos de análisis del servicio QnA Maker](../How-To/get-analytics-knowledge-base.md).

Según la información que obtenga de los análisis, realice [las actualizaciones correspondientes en su base de conocimiento](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Puntuación de confianza](./confidence-score.md)

## <a name="see-also"></a>Otras referencias 

[Base de conocimiento](./knowledge-base.md)
[Información general de QnA Maker](../Overview/overview.md)
