---
title: 'Sistemas operativos y motores de contenedor compatibles: Azure IoT Edge | Microsoft Docs'
description: Obtenga información sobre qué sistemas operativos pueden ejecutar el demonio y el entorno de ejecución de Azure IoT Edge y los motores de contenedor admitidos para los dispositivos de producción
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 12/17/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 6443260de0a8bd8531edb303fa581d281034fef3
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53555615"
---
# <a name="azure-iot-edge-supported-systems"></a>Sistemas compatibles con Azure IoT Edge

Hay varias maneras de buscar compatibilidad para el producto de Azure IoT Edge.

**Notificación de errores**: la mayor parte del desarrollo que lleva el producto de Azure IoT Edge se produce en el proyecto de código abierto de IoT Edge. Los errores se pueden notificar en la [página de problemas](https://github.com/azure/iotedge/issues) del proyecto. Las correcciones pasan pronto del proyecto a las actualizaciones de producto.

**Equipo de soporte técnico al cliente de Microsoft**: los usuarios que dispongan de un [plan de soporte técnico](https://azure.microsoft.com/support/plans/) pueden ponerse en contacto con este equipo creando una incidencia de soporte técnico directamente en [Azure Portal](https://ms.portal.azure.com/signin/index/?feature.settingsportalinstance=mpac).

**Solicitudes de características**: el producto Azure IoT Edge realiza un seguimiento de las solicitudes de características a través de la [página de voz del usuario](https://feedback.azure.com/forums/907045-azure-iot-edge).

## <a name="operating-systems"></a>Sistemas operativos
Azure IoT Edge se ejecuta en la mayoría de los sistemas operativos que pueden ejecutar contenedores. No obstante, la compatibilidad no es igual en todos. Los sistemas operativos se agrupan en niveles que representan el nivel de compatibilidad que los usuarios pueden esperar.

### <a name="tier-1"></a>Nivel 1
Los sistemas de nivel 1 se pueden considerar como oficialmente compatibles. Esto significa que Microsoft:
* tiene estos sistemas operativos en pruebas automatizadas
* proporciona paquetes de instalación para ellos

Disponibilidad general
| Sistema operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Raspbian-stretch | Sin  | SÍ|
| Ubuntu Server 16.04 | SÍ | Sin  |
| Ubuntu Server 18.04 | SÍ | Sin  |

Versión preliminar pública
| Sistema operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| Compilación 17763 de Windows 10 IoT Core | SÍ | Sin  |
| Compilación 17763 de Windows 10 para contenedores de Windows<br><br>Compilación 14393 o más reciente de Windows 10 para contenedores de Linux\* | SÍ | Sin  |
| Windows Server 2019 para contenedores de Windows<br><br>Windows Server 2016 o más reciente para contenedores de Linux\* | SÍ | Sin  |

\* Microsoft proporciona paquetes de instalación para los contenedores de Linux en los dispositivos Windows solo para desarrollo y pruebas. Esto no es una configuración admitida para su uso en producción. 

### <a name="tier-2"></a>Nivel 2
Los sistemas de nivel 2 se pueden considerar como compatible con Azure IoT Edge y pueden usarse con relativa facilidad. Esto significa que:
* Microsoft ha realizado pruebas ad hoc en las plataformas o conoce a un asociado que ha ejecutado correctamente Azure IoT Edge en la plataforma
* Los paquetes de información de otras plataformas pueden funcionar en estas plataformas

| Sistema operativo | AMD64 | ARM32 |
| ---------------- | ----- | ----- |
| CentOS 7.5 | SÍ | SÍ |
| Debian 8 | SÍ | SÍ |
| Debian 9 | SÍ | SÍ |
| RHEL 7.5 | SÍ | SÍ |
| Ubuntu 18.04 | SÍ | SÍ |
| Ubuntu 16.04 | SÍ | SÍ |
| Wind River 8 | SÍ | Sin  |
| Yocto | SÍ | Sin  |

## <a name="container-engines"></a>Motores de contenedor
Azure IoT Edge necesita un motor de contenedor para iniciar módulos, independientemente del sistema operativo en el que se esté ejecutando. Microsoft ofrece un motor de contenedor, moby-engine, para satisfacer este requisito. Se basa en el proyecto de código abierto de Moby. Docker CE y Docker EE son otros motores de contenedores conocidos. También están basados en el proyecto de código abierto de Moby y son compatibles con Azure IoT Edge. Microsoft proporciona el mejor soporte técnico posible para los sistemas que usan esos motores de contenedores pero, sin embargo, no puede proporcionar soluciones para los problemas de ellos. Por esta razón, Microsoft recomienda el uso de moby-engine en sistemas de producción.

