---
title: 'Configuración de un servicio QnA Maker: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Antes de crear alguna base de conocimiento de QnA Maker, primero debe configurar un servicio QnA Maker en Azure. Cualquiera que tenga autorización para crear recursos en una suscripción puede configurar un servicio QnA Maker.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 01/14/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 8c2c50d8d717ec501b6ef847b41e2633487c8877
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266729"
---
# <a name="create-a-qna-maker-service"></a>Creación de un servicio QnA Maker

Antes de crear alguna base de conocimiento de QnA Maker, primero debe configurar un servicio QnA Maker en Azure. Cualquiera que tenga autorización para crear recursos en una suscripción puede configurar un servicio QnA Maker.

Esta configuración implementa algunos recursos de Azure. Juntos, estos recursos administran el contenido de la base de conocimiento y proporcionan funcionalidades de pregunta-respuesta mediante un punto de conexión.

1. Inicie sesión en [Azure Portal](<https://portal.azure.com>).

2.  Haga clic en **Agregar nuevo recurso**, escriba "qna maker" en el cuadro de búsqueda y seleccione el recurso QnA Maker.

    ![Creación de un nuevo servicio QnA Maker: adición de un nuevo recurso](../media/qnamaker-how-to-setup-service/create-new-resource.png)

3.  Haga clic en **Crear** después de leer los términos y condiciones.

    ![Creación de un servicio QnA Maker](../media/qnamaker-how-to-setup-service/create-new-resource-button.png)

4. En **QnA Maker**, seleccione las regiones y los niveles apropiados.

    ![Creación de un nuevo servicio QnA Maker: plan de tarifa y regiones](../media/qnamaker-how-to-setup-service/enter-qnamaker-info.png)

    * Rellene el **Nombre** con un nombre exclusivo para identificar este servicio QnA Maker. Este nombre también identifica el punto de conexión de QnA Maker al que se asociarán las bases de conocimiento.
    * Elija la **Suscripción** en que se implementará el recurso QnA Maker.
    * Seleccione el **Plan de tarifa de administración** para los servicios de administración de QnA Maker (API de portal y administración). Haga clic [aquí](https://aka.ms/qnamaker-pricing) para obtener más información sobre los precios de las SKU.
    * Cree un **Grupo de recursos** (recomendado) o use uno existente en el que implementar este recurso QnA Maker.
    * Elija el **Plan de tarifa de búsqueda** del servicio Azure Search. Si ve la opción de nivel Gratis atenuada, significa que ya dispone del un nivel Gratis de Azure Search implementado en la suscripción. En ese caso, necesitará comenzar con el nivel Básico de Azure Search. Vea los detalles de los precios de Azure Search [aquí](https://azure.microsoft.com/pricing/details/search/).
    * Elija la **Ubicación de la búsqueda** donde desea que se implementen los datos de Azure Search. Las restricciones sobre dónde deben almacenarse los datos del cliente comunicarán la ubicación elegida para Azure Search.
    * Asigne un nombre al servicio de aplicación en **Nombre de aplicación**.
    * De forma predeterminada, el nivel predeterminado de App Service es Estándar (S1). Puede cambiar el plan después de la creación. Consulte más información sobre los precios de App Service [aquí](https://azure.microsoft.com/pricing/details/app-service/).
    * Elija la **Ubicación de sitio web** donde se implementará App Service.

        > [!NOTE]
        > La ubicación de la búsqueda puede ser diferente de la ubicación de sitio web.

    * Elija si desea habilitar **Application Insights** o no. Si **Application Insights** está habilitado, QnA Maker recopila telemetría de tráfico, registros de chat y errores.
    * Elija la **Ubicación de App Insights** donde se implementarán los recursos de Application Insights.

5. Una vez validados todos los campos, puede hacer clic en **Crear** para iniciar la implementación de estos servicios en la suscripción. Esta operación tardará algunos minutos en completarse.

6.  Una vez realizada la implementación, verá los siguientes recursos creados en la suscripción.

    ![Recurso creado de un nuevo servicio QnA Maker](../media/qnamaker-how-to-setup-service/resources-created.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear y publicar una base de conocimiento](../Quickstarts/create-publish-knowledge-base.md)