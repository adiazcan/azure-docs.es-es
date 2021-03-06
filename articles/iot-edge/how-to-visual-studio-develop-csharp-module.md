---
title: 'Desarrollo y depuración de módulos de C# en Visual Studio: Azure IoT Edge | Microsoft Docs'
description: Uso de Visual Studio 2017 para desarrollar y depurar módulos de C# para Azure IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 9a6c892a71c452a0c6c0dcd43509e345280a810e
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054938"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Uso de Visual Studio 2017 para desarrollar y depurar módulos de C# para Azure IoT Edge (versión preliminar)

Puede convertir la lógica de negocios en módulos de Azure IoT Edge. En este artículo se muestra cómo usar Visual Studio 2017 como herramienta principal para desarrollar y depurar módulos de C#.

Las herramientas de Azure IoT Edge para Visual Studio proporcionan las siguientes ventajas:

- Creación, edición, compilación y ejecución de soluciones y módulos de Azure IoT Edge en su equipo de desarrollo local.
- Implementación de la solución de Azure IoT Edge en un dispositivo de Azure IoT Edge gracias a Azure IoT Hub.
- Programación de los módulos de Azure IoT en C# disfrutando de las ventajas de desarrollo en Visual Studio.
- Administración de dispositivos de Azure IoT Edge y módulos sin interfaz de usuario.

En este artículo se muestra cómo usar las herramientas de Azure IoT Edge para Visual Studio 2017 para desarrollar módulos de IoT Edge en C#. También aprenderá a implementar el proyecto en el dispositivo de Azure IoT Edge.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se da por hecho que usa una máquina virtual que ejecuta Windows como máquina de desarrollo. El dispositivo IoT Edge puede ser otro dispositivo físico.

En este artículo se usa Visual Studio 2017 como herramienta de desarrollo principal, así que debe instalar Visual Studio. Y asegúrese de incluir la **carga de trabajo de desarrollo de Azure** en la instalación de Visual Studio 2017. También puede [modificar Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) y agregar la carga de trabajo de desarrollo de Azure.

Una vez que Visual Studio 2017 esté listo, también necesitará las siguientes herramientas y componentes:

- Descargue e instale la [extensión de Azure IoT Edge (versión preliminar)](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) desde el Visual Studio Marketplace para crear un proyecto de IoT Edge en Visual Studio 2017.

- Descargue e instale [Docker Community Edition](https://docs.docker.com/install/) en su equipo de desarrollo para compilar y ejecutar las imágenes del módulo. Deberá configurar Docker CE para que se ejecute ya sea en modo de contenedor de Linux o en modo de contenedor de Windows.

- Configure el entorno local de desarrollo para depurar, ejecutar y probar la solución de IoT Edge mediante la instalación de la [herramienta de desarrollo de Azure IoT EdgeHub](https://pypi.org/project/iotedgehubdev/). Instale [Python (2.7 o 3.6) y Pip](https://www.python.org/) y, a continuación, instale el paquete **iotedgehubdev** mediante la ejecución del siguiente comando en el terminal. Asegúrese de que la versión de la herramienta de desarrollo de Azure IoT EdgeHub es posterior a la 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > Puede usar un registro de Docker local con fines de prototipo y prueba en lugar de un registro en la nube.

- Para probar el módulo en un dispositivo, necesitará una instancia de IoT Hub con al menos un dispositivo IoT Edge. Para usar el equipo como un dispositivo IoT Edge, siga los pasos descritos en el inicio rápido para [Linux](quickstart-linux.md) o [Windows](quickstart.md). Si ejecuta el demonio de IoT Edge en su equipo de desarrollo, es posible que deba detener EdgeHub y EdgeAgent para empezar el desarrollo en Visual Studio.

### <a name="check-your-tools-version"></a>Comprobación de la versión de herramientas

1. En el menú **Herramientas**, seleccione **Extensiones y actualizaciones**. Expanda **Instalado > Herramientas** y encontrará **Herramientas de Azure IoT Edge** y **Cloud Explorer for Visual Studio**.

1. Observe la versión instalada. Puede comparar esta versión con la más reciente de Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge))

1. Si su versión es anterior a la que está disponible en Visual Studio Marketplace, actualice las herramientas en Visual Studio como se muestra en la siguiente sección.

### <a name="update-your-tools"></a>Actualización de las herramientas

1. En el cuadro de diálogo **Extensiones y actualizaciones**, expanda **Actualizaciones > Visual Studio Marketplace**, elija **Herramientas de Azure IoT Edge** o **Cloud Explorer for Visual Studio** y seleccione **Actualizar**.

1. Después de descargar la actualización de las herramientas, cierre Visual Studio para desencadenar la actualización de las herramientas mediante el programa de instalación de VSIX.

1. En el instalador, presione **Aceptar** para iniciar y **Modificar** para actualizar las herramientas.

1. Una vez completada la actualización, seleccione **Cerrar** y reinicie Visual Studio.

### <a name="create-an-azure-iot-edge-project"></a>Creación de un proyecto de Azure IoT Edge

La plantilla de proyecto de Azure IoT Edge en Visual Studio crea un proyecto que se puede implementar en dispositivos de Azure IoT Edge en Azure IoT Hub. En primer lugar, cree una solución de Azure IoT Edge y, luego, genere el primer módulo de C# en esa solución. Cada solución de IoT Edge puede contener más de un módulo.

1. En Visual Studio, seleccione **Nuevo** > **proyecto** en el menú **Archivo**.

1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Instalado**, expanda **Visual C# > Nube**, seleccione **Azure IoT Edge**, escriba un nombre de proyecto y especifique la ubicación; después, haga clic en **Aceptar**. El nombre de proyecto predeterminado es **AzureIoTEdgeApp1**.

   ![Nuevo proyecto](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

1. En la ventana **IoT Edge Module Configuration** (Configuración del módulo de IoT Edge), seleccione **C# Module** (Módulo de C#) y especifique el nombre del módulo y el repositorio de imágenes correspondiente. Visual Studio completa automáticamente el nombre del módulo como **localhost:5000 /<el nombre de su módulo\>**. Reemplácelo por su propia información de registro. Si usa un registro de Docker local para realizar pruebas, **localhost** puede valer. Si va a usar Azure Container Registry, utilice el servidor de inicio de sesión de la configuración del registro. El servidor de inicio de sesión se parece a ***\<nombre del registro\>*.azurecr.io**. Solo reemplace la sección **localhost:5000** de la cadena para que el resultado final se parezca a **\<* nombre del registro*\>.azurecr.io/*\<nombre del módulo\>***. El nombre de módulo predeterminado es **IoTEdgeModule1**

1. Seleccione **Aceptar** para crear el proyecto de Azure IoT Edge con un módulo de C#.

Ahora tiene un proyecto **AzureIoTEdgeApp1** y otro **IoTEdgeModule1** en su solución. El proyecto **AzureIoTEdgeApp1** tiene un archivo **deployment.template.json**. Este archivo define los módulos que desea compilar e implementar para la solución de IoT Edge y, además, define las rutas entre los módulos. La solución predeterminada tiene un módulo **tempSensor** y otro **IoTEdgeModule1**. El módulo **tempSensor** genera datos simulados para **IoTEdgeModule1**, mientras que el código predeterminado del módulo **IoTEdgeModule1** canaliza directamente los mensajes recibidos a Azure IoT Hub.

El proyecto **IoTEdgeModule1** es una aplicación de consola de .NET Core 2.1. Contiene los archivos de Docker que necesita para que el dispositivo de IoT Edge se ejecute con un contenedor de Windows o Linux. El archivo **module.json** describe los metadatos de un módulo. **program.cs** es el código del módulo real, que toma el SDK de dispositivo IoT de Azure como dependencia.

## <a name="develop-your-module"></a>Desarrollo de su módulo

El código del módulo de C# predeterminado que se incluye con la solución se encuentra en **IoTEdgeModule1** > **Program.cs**. El módulo y el archivo deployment.template.json se configuran para que pueda compilar la solución, insertarla en el registro de contenedor e implementarla en un dispositivo para iniciar la prueba sin tocar el código. El módulo se compila para recibir entradas de un origen (en este caso, el módulo **tempSensor** que simula datos) y canalizarlo a Azure IoT Hub.

Cuando esté listo para personalizar la plantilla de C# con su propio código, use los [SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md) para compilar módulos que aborden las necesidades principales de las soluciones de IoT, como seguridad, administración de dispositivos y confiabilidad.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>Inicialización de iotedgehubdev con una cadena de conexión de dispositivo IoT Edge

1. Copie la cadena de conexión de cualquier dispositivo de IoT Edge desde **cadena de conexión principal** en Visual Studio Cloud Explorer. Asegúrese de no copiar la cadena de conexión de un dispositivo que no sea Edge, ya que el icono de dispositivo IoT Edge es diferente al del dispositivo que no es Edge.

   ![Copia de la cadena de conexión del dispositivo Edge](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. Haga clic con el botón derecho en el proyecto **AzureIoTEdgeApp1** y, a continuación, haga clic en **Set Edge Device Connection String** (Establecer cadena de conexión del dispositivo Edge) para mostrar la ventana de configuración de Azure IoT Edge.

   ![Apertura de la ventana de la cadena de conexión del dispositivo Edge establecida](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. Escriba la cadena de conexión del primer paso y, a continuación, seleccione **Aceptar**.

> [!NOTE]
> Debe seguir estos pasos solo una vez en el equipo de desarrollo, ya que los resultados se aplican automáticamente a todas las soluciones de Azure IoT Edge posteriores. Puede realizar nuevamente este procedimiento si necesita cambiar a una cadena de conexión diferente.

## <a name="build-and-debug-single-c-module"></a>Compilación y depuración de un solo módulo de C#

Por lo general, querrá probar y depurar los módulos antes de que se ejecuten en una solución entera con varios de ellos.

1. Haga clic con el botón derecho en **IoTEdgeModule1** y seleccione **Set as StartUp Project** (Establecer como proyecto de inicio) en el menú contextual.

   ![Establecimiento del proyecto de inicio](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Presione **F5** o haga clic en el siguiente botón para ejecutar el módulo, puede tardar unos 10&ndash;20 segundos la primera vez que se ejecute.

   ![Ejecución del módulo](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Si el módulo se inicializa correctamente, verá que una aplicación de consola de .NET Core se inicia.

   ![Módulo en ejecución](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. Establezca un punto de interrupción en la función `PipeMessage()` del archivo **Program.cs** y, a continuación, envíe un mensaje para probarla mediante el comando siguiente en un shell de **Git Bash** o **WSL Bash**. (No puede ejecutar el comando `curl` en PowerShell ni desde el símbolo del sistema).

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Depuración de un solo módulo](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Debe desencadenarse el punto de interrupción. Puede inspeccionar las variables en la ventana **Variables locales** de Visual Studio.

   > [!TIP]
   > También puede usar [PostMan](https://www.getpostman.com/) u otras herramientas de API para enviar mensajes en lugar de `curl`.

1. Presione **Ctrl + F5** o haga clic en el botón Detener para detener la depuración.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Compilación y depuración de la solución IoT Edge con varios módulos

Cuando haya terminado de desarrollar el módulo, querrá ejecutar y depurar una solución completa de varios módulos.

1. Para agregar un segundo módulo C# a la solución, haga clic en **AzureIoTEdgeApp1** y seleccione **Agregar** > **New IoT Edge Module** (Nuevo módulo de IoT Edge). El nombre predeterminado del segundo módulo es **IoTEdgeModule2** y actuará como otro módulo de canalización.

1. Abra el archivo `deployment.template.json` y verá que **IoTEdgeModule2** se ha agregado a la sección **modules**. Reemplace la sección **routes** por lo siguiente. Si ha personalizado los nombres de módulo, asegúrese de actualizarlos para que coincidan.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

1. Haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Set as StartUp Project** (Establecer como proyecto de inicio) en el menú contextual.

1. Cree los puntos de interrupción y presione **F5** para poder ejecutar y depurar varios módulos a la vez. Verá varias ventanas de aplicación de consola de .NET Core; cada una de ellas representa un módulo de C# distinto.

   ![Depuración de varios módulos](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Presione **Ctrl + F5** o seleccione el botón Detener para detener la depuración.

## <a name="build-and-push-images"></a>Compilación e inserción de imágenes

1. Asegúrese de que **AzureIoTEdgeApp1** es el proyecto de inicio. Seleccione **Depurar** o **Versión** como configuración para compilar las imágenes del módulo.

    > [!NOTE]
    > Al elegir **Depurar**, Visual Studio usará `Dockerfile.debug` para compilar las imágenes de Docker. Esto incluye el depurador de la línea de comandos de .NET Core VSDBG en la imagen de contenedor al realizar la compilación. Para los módulos de IoT Edge listos para producción, se recomienda usar la configuración de **Versión** que usa `Dockerfile` sin VSDBG.

1. Si usa un registro privado como Azure Container Registry, use el siguiente comando de Docker para iniciar sesión en él. Si usa un registro local, puede [Run a local registry](https://docs.docker.com/registry/deploying/#run-a-local-registry) (Ejecutar un registro local).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Si usa un registro privado como Azure Container Registry, deberá agregar la información de inicio de sesión del registro a la configuración en tiempo de ejecución que se encuentra en el archivo `deployment.template.json`. Reemplace los marcadores de posición por su nombre de usuario administrador, contraseña y nombre de registro reales de ACR.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

1. Haga clic con el botón derecho en **AzureIoTEdgeApp1** y seleccione **Build and Push Edge Solution** (Compilar e insertar solución de Edge) para compilar e inserta las imágenes de Docker para cada módulo.

   ![Compilación e inserción de imágenes](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)

## <a name="deploy-the-solution"></a>Implementación de la solución

En el artículo de la guía de inicio rápido que siguió para configurar el dispositivo de IoT Edge, implementó un módulo mediante Azure Portal. También puede implementar módulos mediante Cloud Explorer para Visual Studio. Ya tiene un manifiesto de implementación listo para su escenario (el archivo `deployment.json`) y todo lo que necesita hacer es seleccionar el dispositivo que recibirá la implementación.

1. Abra **Cloud Explorer** al hacer clic en **Ver** > **Cloud Explorer**. Asegúrese de haber iniciado sesión en Visual Studio 2017.

1. En **Cloud Explorer**, expanda su suscripción, busque Azure IoT Hub y el dispositivo de Azure IoT Edge que desea implementar.

1. Haga clic con el botón derecho en el dispositivo de IoT Edge que crear e implementar; debe seleccionar el archivo de manifiesto de implementación de `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

   > [!NOTE]
   > No debe seleccionar `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`.

1. Haga clic en el botón Actualizar para ver los nuevos módulos en ejecución junto con el módulo **TempSensor**, **$edgeAgent** y **$edgeHub**.

## <a name="view-generated-data"></a>Visualización de datos generados

1. Para supervisar el mensaje de D2C de un determinado dispositivo, seleccione el dispositivo en la lista y, a continuación, haga clic en **Start Monitoring D2C Messages** (Iniciar la supervisión de mensajes de D2C) en la ventana **Acción**.

1. Para detener la supervisión de datos, seleccione el dispositivo en la lista y, después, **Stop Monitoring D2C Messages** (Detener la supervisión de mensajes de D2C) en la ventana **Acción**.

## <a name="next-steps"></a>Pasos siguientes

Para desarrollar módulos personalizados para los dispositivos IoT Edge, consulte [Información y uso de los SDK de Azure IoT Hub](../iot-hub/iot-hub-devguide-sdks.md).
