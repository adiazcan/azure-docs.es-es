---
title: Instalación de Azure IoT Edge en Windows | Microsoft Docs
description: Instrucciones de instalación de Azure IoT Edge en Windows 10, Windows Server y Windows IoT Core
author: kgremban
manager: philmea
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: kgremban
ms.custom: seodec18
ms.openlocfilehash: c280410816bfb48f21c68fe5d57b6ae18af0e855
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2019
ms.locfileid: "53970668"
---
# <a name="install-the-azure-iot-edge-runtime-on-windows"></a>Instalación del entorno de ejecución de Azure IoT Edge en Windows

El entorno de ejecución de Azure IoT Edge es lo que convierte a un dispositivo en un dispositivo IoT Edge. El entorno de ejecución se puede implementar en dispositivos tan pequeños como un Raspberry Pi o tan grandes como un servidor industrial. Una vez que un dispositivo está configurado con el entorno de ejecución de IoT Edge, puede empezar a implementar lógica de negocios en él desde la nube. 

Para obtener más información sobre el entorno de ejecución de IoT Edge, vea [Información del entorno de ejecución de Azure IoT Edge y su arquitectura](iot-edge-runtime.md).

En este artículo se enumeran los pasos para instalar el entorno de ejecución de Azure IoT Edge en el sistema Windows x64 (AMD e Intel). Esta compatibilidad con Windows se encuentra actualmente en versión preliminar.

>[!NOTE]
El uso de contenedores Linux en sistemas Windows no es una configuración de producción recomendada o compatible con Azure IoT Edge. Sin embargo, se puede usar con fines de desarrollo y pruebas.

## <a name="prerequisites"></a>Requisitos previos

Utilice esta sección para revisar si el dispositivo Windows puede admitir IoT Edge y para prepararlo para un motor de contenedor antes de la instalación. 

### <a name="supported-windows-versions"></a>Versiones de Windows admitidas

Azure IoT Edge es compatible con distintas versiones de Windows, en función de si ejecuta contenedores de Windows o de Linux. 

Puede ejecutar la versión más reciente de Azure IoT Edge con contenedores de Windows en las siguientes versiones de Windows:
* Windows 10 o IoT Core con la actualización de octubre de 2018 (compilación 17763)
* Windows Server 2019 (compilación 17763)

Puede ejecutar la versión más reciente de Azure IoT Edge con contenedores de Linux en las siguientes versiones de Windows: 
* Actualización de aniversario de Windows 10 (compilación 14393) o posterior
* Windows Server 2016 o posterior

Para obtener más información sobre qué sistemas operativos se admiten actualmente, vea [Sistemas compatibles con Azure IoT Edge](support.md#operating-systems). 

Para obtener más información sobre qué se incluye en la versión más reciente de IoT Edge, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases).

### <a name="prepare-for-a-container-engine"></a>Preparación de un motor de contenedor 

Azure IoT Edge se basa en un motor de contenedor [compatible con OCI](https://www.opencontainers.org/). Para escenarios de producción, use el motor de Moby incluido en el script de instalación para ejecutar contenedores de Windows en el dispositivo Windows. Para desarrollar y probar, puede ejecutar contenedores de Linux en el dispositivo Windows, pero debe instalar y configurar un motor de contenedor antes de instalar IoT Edge. Para estos escenarios, vea las siguientes secciones para obtener información sobre los requisitos previos para preparar el dispositivo. 

#### <a name="moby-engine-for-windows-containers"></a>Motor Moby para contenedores de Windows

Para dispositivos Windows con IoT Edge en escenarios de producción, Moby es el único motor de contenedor compatible oficialmente. El script de instalación instala automáticamente el motor Moby en el dispositivo antes de instalar IoT Edge. Prepare el dispositivo activando la característica de contenedores. 

1. En la barra de inicio, busque **Activar o desactivar las características de Windows** y abra el programa del panel de control.
2. Busque y seleccione **Contenedores**.
3. Seleccione **Aceptar**. 

#### <a name="docker-for-linux-containers"></a>Contenedores Docker para Linux

Si usa Windows para desarrollar y probar contenedores para los dispositivos de Linux, puede usar [Docker para Windows](https://www.docker.com/docker-windows) como motor de contenedor. Docker puede configurarse para usar [contenedores de Linux](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers). Deberá instalar Docker y configurarlo antes de instalar IoT Edge. Los contenedores de Linux no se admiten en los dispositivos Windows en producción. 

Si su dispositivo IoT Edge es un equipo Windows, compruebe que cumple los [requisitos del sistema](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/hyper-v-requirements) para Hyper-V. Si es una máquina virtual, habilite la [virtualización anidada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) y asigne al menos 2 GB de memoria.

## <a name="install-iot-edge-on-a-new-device"></a>Instalación de IoT Edge en un dispositivo nuevo

>[!NOTE]
>Los paquetes de software de Azure IoT Edge están sujetos a los términos de licencia ubicados en los paquetes (en el directorio LICENSE). Lea los términos de licencia antes de usar el paquete. La instalación y uso del paquete constituye la aceptación de estos términos. Si no acepta los términos de licencia, no utilice el paquete.

Un script de PowerShell descarga e instala el demonio de seguridad de Azure IoT Edge. Después, el demonio de seguridad inicia el primero de los dos módulos en tiempo de ejecución, el agente de IoT Edge, que permite implementaciones remotas de otros módulos. 

Cuando instale por primera vez el tiempo de ejecución de IoT Edge en un dispositivo, debe aprovisionar el dispositivo con una identidad de un IoT hub. Un único dispositivo de IoT Edge se puede aprovisionar manualmente mediante una cadena de conexiones de dispositivo proporcionada por IoT Hub. O bien, puede usar el servicio Device Provisioning para aprovisionar dispositivos automáticamente, lo que resulta útil cuando tiene muchos dispositivos para configurar. Según la elección de aprovisionamiento, elija el script de instalación adecuado. 

En las siguientes secciones se describen los casos de uso comunes y los parámetros para el script de instalación de IoT Edge en un dispositivo nuevo. 

### <a name="option-1-install-and-manually-provision"></a>Opción 1: Instalación y aprovisionamiento manual

En esta primera opción, proporciona una cadena de conexión de dispositivo generada por IoT Hub para aprovisionar el dispositivo. 

Siga los pasos de [Registro de un nuevo dispositivo Azure IoT Edge desde Azure Portal](how-to-register-device-portal.md) para registrar el dispositivo y recuperar la cadena de conexión de dicho dispositivo. 

En este ejemplo se muestra una instalación manual con contenedores de Windows:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -ContainerOs Windows -DeviceConnectionString '<connection-string>'
```

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:
* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado
* Omitir la instalación de la CLI de Moby

Para obtener más información sobre estas opciones de instalación, continúe leyendo este artículo o salte para obtener información sobre [Todos los parámetros de instalación](#all-installation-parameters).

### <a name="option-2-install-and-automatically-provision"></a>Opción 2: Instalación y aprovisionamiento automático

En esta segunda opción, aprovisionará el dispositivo mediante el servicio IoT Hub Device Provisioning. Proporcione el **Id. de ámbito** de una instancia del servicio Device Provisioning y el **Id. de registro** del dispositivo.

Siga los pasos de [Creación y aprovisionamiento de un dispositivo TPM Edge simulado en Windows](how-to-auto-provision-simulated-device-windows.md) para configurar el servicio Device Provisioning y recuperar su **identificador de ámbito**, simular un dispositivo TPM y recuperar su **identificador de registro**; luego, cree una inscripción individual. Una vez que el dispositivo esté registrado en IoT Hub, continúe con la instalación.  

   >[!TIP]
   >Mantenga abierta la ventana que se está ejecutando el simulador de TPM durante la instalación y prueba. 

En el siguiente ejemplo se muestra una instalación automática con contenedores de Windows:

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Dps -ContainerOs Windows -ScopeId <DPS scope ID> -RegistrationId <device registration ID>
```

Al instalar y aprovisionar un dispositivo manualmente, puede usar parámetros adicionales para modificar la instalación, incluyendo:
* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión
* Declarar una imagen de contenedor del agente específica y proporcionar las credenciales si se encuentra en un registro privado
* Omitir la instalación de la CLI de Moby

Para obtener más información sobre estas opciones de instalación, continúe leyendo este artículo o salte para obtener información sobre [Todos los parámetros de instalación](#all-installation-parameters).

## <a name="update-an-existing-installation"></a>Actualización de una instalación existente

Si ya ha instalado el entorno de ejecución de IoT Edge en un dispositivo antes y lo ha aprovisionado con una identidad de IoT Hub, puede usar un script de instalación simplificado. La marca `-ExistingConfig` declara que ya existe un archivo de configuración de IoT Edge en el dispositivo. El archivo de configuración contiene información sobre la identidad del dispositivo, así como sobre los certificados y la configuración de red. Puede usar esta opción de instalación independientemente de si el dispositivo se aprovisionó originalmente de forma manual o automática. 

Para obtener más información, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).

En este ejemplo se muestra una instalación que apunta a un archivo de configuración existente y usa contenedores de Windows: 

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -ExistingConfig -ContainerOs Windows
```

Al instalar IoT Edge desde un archivo de configuración existente, puede usar parámetros adicionales para modificar la instalación, incluyendo:
* Dirigir el tráfico para que pase por un servidor proxy
* Apuntar el instalador a un directorio sin conexión, o 
* Omitir la instalación de la CLI de Moby. 

No se puede declarar una imagen de contenedor del agente de IoT Edge con parámetros de script, porque esa información ya está establecida en el archivo de configuración de la instalación anterior. Si desea modificar la imagen del contenedor del agente, debe hacerlo en el archivo config.yaml. 

Para obtener más información sobre estas opciones de instalación, continúe leyendo este artículo o salte para obtener información sobre [Todos los parámetros de instalación](#all-installation-parameters).

## <a name="offline-installation"></a>Instalación sin conexión

Durante la instalación se descargan cuatro archivos: 
* Un archivo ZIP de demonio de seguridad de IoT Edge (iotedgd) 
* Un archivo ZIP del motor Moby
* Un archivo ZIP de la CLI de Moby
* Un archivo msi del paquete redistribuible de Visual C++ (tiempo de ejecución de VC)

Puede descargar uno o todos estos archivos por adelantado en el dispositivo y luego seleccionar el script de instalación en el directorio que contiene los archivos. El instalador comprueba en primer lugar el directorio y luego descarga solo los componentes que no se pueden encontrar. Si los cuatro archivos están disponibles sin conexión, puede instalar sin conexión a Internet. También puede usar esta característica para invalidar las versiones en línea de uno o más componentes.  

Para obtener los archivos de instalación más recientes, junto con las versiones anteriores, vea las [versiones de Azure IoT Edge](https://github.com/Azure/azure-iotedge/releases)

Para instalar con componentes sin conexión, use el parámetro `-OfflineInstallationPath` y proporcione la ruta de acceso absoluta al directorio de archivos. Por ejemplo,

```powershell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Install-SecurityDaemon -Manual -DeviceConnectionString '<connection-string>' -OfflineInstallationPath C:\Downloads\iotedgeoffline
```

## <a name="all-installation-parameters"></a>Todos los parámetros de instalación

En las secciones anteriores se presentaron escenarios de instalación comunes con ejemplos de cómo usar parámetros para modificar el script de instalación. En esta sección se proporciona una tabla de referencia de los parámetros válidos que puede usar para instalar IoT Edge. Para obtener más información, ejecute `get-help Install-SecurityDaemon -full` en una ventana de PowerShell. 

Para instalar IoT Edge con una configuración existente, el comando de instalación puede usar estos parámetros comunes: 

| Parámetro | Valores aceptados | Comentarios |
| --------- | --------------- | -------- |
| **Manual** | None | **Parámetro de modificador**. Todas las instalaciones deben declararse como manuales, DPS, o existingconfig.<br><br>Declara que usted proporcionará una cadena de conexión de dispositivo para aprovisionar el dispositivo manualmente |
| **Dps** | None | **Parámetro de modificador**. Todas las instalaciones deben declararse como manuales, DPS, o existingconfig.<br><br>Declara que usted proporcionará un Id. de ámbito de Device Provisioning Service (DPS) y el Id. de registro del dispositivo para aprovisionar a través de DPS.  |
| **ExistingConfig** | None | **Parámetro de modificador**. Todas las instalaciones deben declararse como manuales, DPS, o existingconfig.<br><br>Declara que un archivo config.yaml ya existe en el dispositivo con su información de aprovisionamiento. |
| **DeviceConnectionString** | Una cadena de conexión desde un dispositivo de IoT Edge registrado en IoT Hub, entre comillas simples | **Obligatorio** para la instalación manual. Si no proporciona una cadena de conexión en los parámetros del script, se le pedirá una durante la instalación. |
| **ScopeId** | Un Id. de ámbito de una instancia de Device Provisioning Service asociada con IoT Hub. | **Obligatorio** para la instalación de DPS. Si no proporciona un Id. de ámbito en los parámetros del script, se le pedirá uno durante la instalación. |
| **RegistrationId** | Un identificador de registro generado por el dispositivo | **Obligatorio** para la instalación de DPS. Si no proporciona un Id. de registro en los parámetros del script, se le pedirá uno durante la instalación. |
| **ContainerOs** | **Windows** o **Linux** | Si no se especifica el sistema operativo de contenedor, el valor predeterminado es Linux. Para los contenedores de Windows, se incluirá un motor de contenedor en la instalación. Para los contenedores de Linux, deberá instalar un motor de contenedor antes de iniciar la instalación. Ejecutar contenedores de Linux en Windows es un escenario de desarrollo útil, pero no se admite en producción. |
| **Proxy** | Dirección URL del proxy | Incluya este parámetro si el dispositivo debe pasar por un servidor proxy para conectarse a Internet. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **InvokeWebRequestParameters** | Tabla de hash de parámetros y valores | Durante la instalación se realizan varias solicitudes web. Utilice este campo para definir parámetros para esas solicitudes web. Este parámetro es útil para configurar las credenciales para los servidores proxy. Para más información, consulte [Configuración de un dispositivo de IoT Edge para que se comunique a través de un servidor proxy](how-to-configure-proxy-support.md). |
| **OfflineInstallationPath** | Ruta de acceso del directorio | Si se incluye este parámetro, el instalador buscará en el directorio los archivos ZIP de iotedged, del motor de Moby y de la CLI de Moby, y los archivos MSI en tiempo de ejecución de VC necesarios para la instalación. Si los cuatro archivos están en el directorio, puede instalar IoT Edge sin conexión a Internet. También puede usar este parámetro para invalidar la versión en línea de un componente específico. |
| **AgentImage** | URI de la imagen del agente de IoT Edge | De forma predeterminada, una nueva instalación de IoT Edge utiliza la etiqueta gradual más reciente de la imagen del agente de IoT Edge. Utilice este parámetro para establecer una etiqueta específica para la versión de la imagen o para proporcionar su propia imagen de agente. Para obtener más información, vea [Información sobre las etiquetas de IoT Edge](how-to-update-iot-edge.md#understand-iot-edge-tags). |
| **Nombre de usuario** | Nombre de usuario de registro de contenedor | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione un nombre de usuario con acceso al registro. |
| **Contraseña** | Cadena de contraseña segura | Utilice este parámetro solo si establece el parámetro -AgentImage en un contenedor en un registro privado. Proporcione la contraseña para acceder al registro. | 
| **SkipMobyCli** | None | Solo se aplica si -ContainerOS se establece en Windows. No instale la CLI de Moby (docker.exe) en $MobyInstallDirectory. |

## <a name="verify-successful-installation"></a>Comprobación de instalación correcta

Puede comprobar el estado del servicio IoT Edge mediante: 

```powershell
Get-Service iotedge
```

Examine los registros de servicio de los últimos 5 minutos con:

```powershell

# Displays logs from last 5 min, newest at the bottom.

Get-WinEvent -ea SilentlyContinue `
  -FilterHashtable @{ProviderName= "iotedged";
    LogName = "application"; StartTime = [datetime]::Now.AddMinutes(-5)} |
  select TimeCreated, Message |
  sort-object @{Expression="TimeCreated";Descending=$false} |
  format-table -autosize -wrap
```

Y, enumere los módulos en ejecución con:

```powershell
iotedge list
```

## <a name="uninstall-iot-edge"></a>Desinstalación de IoT Edge

Si quiere quitar la instalación de IoT Edge del dispositivo Windows, use el siguiente comando de una ventana de PowerShell administrativa. Este comando quita el entorno de ejecución de IoT Edge, junto con la configuración existente y los datos de motor de Moby. 

```PowerShell
. {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
Uninstall-SecurityDaemon -DeleteConfig -DeleteMobyDataRoot
```

Si piensa volver a instalar IoT Edge en el dispositivo, ignore los parámetros `-DeleteConfig` y `-DeleteMobyDataRoot` de forma que pueda volver a instalar el demonio de seguridad más adelante con la información de configuración existente. 

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene un dispositivo IoT Edge aprovisionado con el entorno de ejecución instalado, puede [implementar módulos de IoT Edge](how-to-deploy-modules-portal.md).

Si tiene problemas con la instalación correcta de IoT Edge, vea la página de [solución de problemas](troubleshoot.md).

Para actualizar una instalación existente a la versión más reciente de IoT Edge, vea [Actualice el archivo de configuración del demonio de seguridad y el entorno de ejecución de IoT Edge](how-to-update-iot-edge.md).
