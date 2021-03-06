---
title: 'Implementación de módulos desde Azure Portal: Azure IoT Edge | Microsoft Docs'
description: Uso de Azure Portal para implementar módulos en un dispositivo de IoT Edge
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/03/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 8b7327796cf29c8c234c0a750c90e0689f508f7e
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53969410"
---
# <a name="deploy-azure-iot-edge-modules-from-the-azure-portal"></a>Implementación de módulos de Azure IoT Edge desde Azure Portal

Una vez que ha creado módulos de IoT Edge con su lógica empresarial, querrá implementarlos en sus dispositivos para usarlos en el perímetro. Si tiene varios módulos que funcionan conjuntamente para recopilar y procesar datos, puede implementarlos todos a la vez y declarar las reglas de enrutamiento que los conectan.

Este artículo muestra cómo le puede ayudar Azure Portal en la creación de un manifiesto de implementación y en la inserción de la implementación en un dispositivo de IoT Edge. Para información sobre la creación de una implementación dirigida a varios dispositivos en función de sus etiquetas compartidas, consulte [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).

## <a name="prerequisites"></a>Requisitos previos

* Una instancia de [IoT Hub](../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure.
* Un [dispositivo de IoT Edge](how-to-register-device-portal.md) que tenga instalado el entorno de ejecución de Azure IoT Edge.

## <a name="select-your-device"></a>Selección del dispositivo

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a IoT Hub.
1. Seleccione **IoT Edge** en el menú.
1. Haga clic en el identificador del dispositivo de destino en la lista de dispositivos.
1. Seleccione **Set modules** (Establecer módulos).

## <a name="configure-a-deployment-manifest"></a>Configuración de un manifiesto de implementación

Un manifiesto de implementación es un documento JSON que describe qué módulos se van a implementar, cómo fluyen los datos entre los módulos y las propiedades deseadas de los módulos gemelos. Para más información sobre los manifiestos de implementación y cómo crearlos, consulte [Descripción de cómo se pueden utilizar, configurar y reutilizar los módulos de IoT Edge](module-composition.md).

Azure Portal tiene un asistente que le guía en la creación del manifiesto de implementación, en lugar de crear el documento JSON de forma manual. Consta de tres pasos: **Adición de módulos**, **Especificación de rutas** y **Revisión de la implementación**.

### <a name="add-modules"></a>Adición de módulos

1. En la sección **Configuración del registro** de la página, proporcione las credenciales para acceder a cualquier registro del contenedor privado que contiene las imágenes del módulo.

1. En la sección **Módulos de implementación** de la página, seleccione **Agregar**.

1. Fíjese en los tipos de módulos en la lista desplegable:

   * **Módulo de IoT Edge**: la opción predeterminada.
   * **Módulo de Azure Stream Analytics**: solo módulos generados a partir de una carga de trabajo de Azure Stream Analytics.

1. Seleccione **Módulo IoT Edge**.

1. Proporcione un nombre para el módulo y, a continuación, especifique la imagen de contenedor. Por ejemplo: 

   * **Nombre**: tempSensor
   * **Identificador URI de la imagen**: mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0

1. Rellene los campos opcionales si es necesario. Para más información sobre las opciones de creación del contenedor, la directiva de reinicio y el estado deseado, consulte [Propiedades deseadas de EdgeAgent](module-edgeagent-edgehub.md#edgeagent-desired-properties). Para más información sobre el módulo gemelo, consulte [Definición o actualización de las propiedades deseadas](module-composition.md#define-or-update-desired-properties).

1. Seleccione **Guardar**.

1. Repita los pasos 2 a 6 para agregar módulos adicionales a la implementación.

1. Seleccione **Siguiente** para ir a la sección de rutas.

### <a name="specify-routes"></a>Especificación de rutas

De forma predeterminada, el asistente proporciona una ruta llamada **route** y definida como **FROM /\* INTO $upstream**, que significa que cualquier mensaje de salida de cualquier módulo se envía al IoT Hub.  

Agregue o actualice las rutas con la información de [Declaración de rutas](module-composition.md#declare-routes) y, a continuación, seleccione **Siguiente** para continuar con la sección de revisión.

### <a name="review-deployment"></a>Revisión de la implementación

La sección de revisión le muestra el manifiesto de implementación en formato JSON que se ha creado en función de las selecciones de las dos secciones anteriores. Observe que hay dos módulos declarados que no ha agregado: **$edgeAgent** y **$edgeHub**. Estos dos módulos constituyen el [entorno de ejecución de Azure IoT Edge](iot-edge-runtime.md) y son valores predeterminados necesarios en todas las implementaciones.

Revise la información de implementación y seleccione **Enviar**.

## <a name="view-modules-on-your-device"></a>Visualización de módulos en el dispositivo

Una vez que los módulos se han implementado en el dispositivo, puede verlos todos en la página **Detalles del dispositivo** del portal. Esta página muestra el nombre de cada módulo implementado, así como información útil, como el estado de la implementación y el código de salida.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [implementar y supervisar módulos de IoT Edge a escala](how-to-deploy-monitor.md)
