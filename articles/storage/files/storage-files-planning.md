---
title: Planeamiento de una implementación de Azure Files | Microsoft Docs
description: Conozca los puntos que debe tener en cuenta al planear una implementación de Azure Files.
services: storage
author: wmgries
ms.service: storage
ms.topic: article
ms.date: 06/12/2018
ms.author: wgries
ms.component: files
ms.openlocfilehash: 0701049eb1aa86398e90484dbf21ef3781270fba
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48831388"
---
# <a name="planning-for-an-azure-files-deployment"></a>Planeamiento de una implementación de Azure Files
[Azure Files](storage-files-introduction.md) ofrece recursos compartidos de archivos en la nube totalmente administrados a los que se puede acceder mediante el protocolo SMB estándar. Dado que Azure Files está totalmente administrado, su implementación en escenarios de producción resulta mucho más sencilla que la implementación y administración de un servidor de archivos o un dispositivo NAS. En este artículo se tratan las cuestiones que deben tenerse en cuenta al implementar un recurso compartido de archivos de Azure para su uso en producción dentro de la organización.

## <a name="management-concepts"></a>Conceptos de administración
 El siguiente diagrama muestra las construcciones de administración de Azure Files:

![Estructura de archivos](./media/storage-files-introduction/files-concepts.png)

* **Cuenta de almacenamiento**: todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Consulte el artículo sobre los [objetivos de escalado y rendimiento](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) para información sobre la capacidad de la cuenta de almacenamiento.

* **Recurso compartido:** un recurso compartido de File Storage es un recurso compartido de archivos de SMB en Azure. Todos los directorios y archivos se deben crear en un recurso compartido principal. Una cuenta puede contener un número ilimitado de recursos compartidos, y un recurso compartido puede almacenar un número ilimitado de archivos, hasta la capacidad total de 5 TiB del recurso compartido de archivos.

* **Directorio:** una jerarquía de directorios opcional.

* **Archivo:** un archivo del recurso compartido. Un archivo puede tener un tamaño de hasta 1 TiB.

* **Formato de dirección URL**: en las solicitudes a un recurso compartido de archivos de Azure realizadas con el protocolo de REST de archivo, los archivos son direccionables mediante el formato de dirección URL siguiente:

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory>/directories>/<file>
    ```

## <a name="data-access-method"></a>Método de acceso a datos
Azure Files ofrece dos cómodos métodos de acceso a datos integrados que puede usar por separado o combinados entre sí para acceder a los datos:

1. **Acceso directo a la nube**: cualquier recurso compartido de archivos de Azure se puede montar mediante [Windows](storage-how-to-use-files-windows.md), [macOS](storage-how-to-use-files-mac.md) o [Linux](storage-how-to-use-files-linux.md) con el protocolo de bloque de mensaje de servidor (SMB) estándar del sector o a través de la API de REST de archivo. Con SMB, las operaciones de lectura y escritura en archivos del recurso compartido se realizan directamente en el recurso compartido de archivos en Azure. Para montar mediante una máquina virtual en Azure, el cliente SMB del sistema operativo debe ser compatible al menos con SMB 2.1. Para montar en local, como en la estación de trabajo de un usuario, el cliente SMB compatible con la estación de trabajo debe ser compatible al menos con SMB 3.0 (con cifrado). Además de SMB, hay nuevas aplicaciones o servicios que pueden acceder directamente al recurso compartido de archivos a través de REST de archivo, lo que proporciona una interfaz de programación de aplicaciones escalable y sencilla para el desarrollo de software.
2. **Azure File Sync**: con Azure File Sync, los recursos compartidos se pueden replicar en servidores de Windows Server locales o en Azure. Los usuarios accederían al recurso compartido de archivos mediante el servidor de Windows Server, por ejemplo, a través de un recurso compartido de SMB o NFS. Esto resulta útil en escenarios en los que es necesario acceder a los datos y modificarlos lejos de un centro de datos de Azure, como puede ser en una sucursal. Lo datos pueden replicarse entre varios puntos de conexión de Windows Server, por ejemplo, entre varias sucursales. Por último, los datos pueden colocarse en niveles en Azure Files, de modo que se pueda seguir accediendo a todos los datos a través del servidor, pero este no tenga una copia completa de ellos. Los datos se recuperan sin problemas cuando los abre el usuario.

En la tabla siguiente se muestra cómo pueden acceder los usuarios y las aplicaciones al recurso compartido de archivos de Azure:

| | Acceso directo a la nube | Azure File Sync |
|------------------------|------------|-----------------|
| ¿Qué protocolos necesita usar? | Azure Files admite SMB 2.1, SMB 3.0 y API de REST de archivo. | Acceda al recurso compartido de archivos de Azure a través de cualquier protocolo compatible de Windows Server (SMB, NFS, FTPS, etc.) |  
| ¿Dónde se ejecuta la carga de trabajo? | **En Azure**: Azure Files ofrece acceso directo a los datos. | **En local con red lenta**: los clientes de Windows, Linux y macOS pueden montar un recurso compartido de archivos de Windows local como una memoria caché rápida del recurso compartido de archivos de Azure. |
| ¿Qué nivel de ACL necesita? | Nivel de recurso compartido y archivo. | Nivel de recurso compartido, archivo y usuario. |

## <a name="data-security"></a>Seguridad de los datos
Azure Files tiene varias opciones integradas para garantizar la seguridad de los datos:

* Compatibilidad con el cifrado en ambos protocolos a través de cable: cifrado SMB 3.0 y REST de archivo a través de HTTPS. De forma predeterminada: 
    * Los clientes que admiten el cifrado SMB 3.0 envían y reciben datos a través de un canal cifrado.
    * Los clientes que no admiten SMB 3.0 con cifrado pueden comunicarse dentro de centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado. No se permite a los clientes SMB comunicarse entre centros de datos a través de SMB 2.1 o SMB 3.0 sin cifrado.
    * Los clientes pueden comunicarse a través de REST de archivo con HTTP o HTTPS.
* Cifrado en reposo ([Azure Storage Service Encryption](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)): la característica Storage Service Encryption (SSE) está habilitada para todas las cuentas de almacenamiento. Los datos en reposo se cifran con claves completamente administradas. En el cifrado en reposo no se aumentan los costos de almacenamiento ni se reduce el rendimiento. 
* Requisito opcional de datos cifrados en tránsito: cuando está seleccionado, Azure Files rechaza el acceso a los datos a través de canales sin cifrar. En concreto, solo se permiten HTTPS y SMB 3.0 con conexiones de cifrado. 

    > [!Important]  
    > La exigencia de transferencia segura de datos hace que los clientes SMB más antiguos que no son capaces de comunicarse con SMB 3.0 con cifrado experimenten un error. Para más información, consulte [Montaje en Windows](storage-how-to-use-files-windows.md), [Montaje en Linux](storage-how-to-use-files-linux.md) y [Montaje en macOS](storage-how-to-use-files-mac.md).

Para lograr la máxima seguridad, se recomienda encarecidamente habilitar siempre el cifrado en reposo y el cifrado de datos en tránsito cuando se usen clientes modernos para acceder a los datos. Por ejemplo, si tiene que montar un recurso compartido en una máquina virtual de Windows Server 2008 R2 que solo es compatible con SMB 2.1, debe permitir el tráfico sin cifrar a la cuenta de almacenamiento, dado que SMB 2.1 no admite el cifrado.

Si usa Azure File Sync para acceder al recurso compartido de archivos de Azure, use siempre HTTPS y SMB 3.0 con cifrado para sincronizar los datos en los servidores de Windows Server, independientemente de si se exige cifrado de datos en reposo.

## <a name="file-share-performance-tiers"></a>Niveles de rendimiento de un recurso compartido de archivos
Azure Files admite dos niveles de rendimiento: Estándar y Premium.

* Los **recursos compartidos de archivos estándar** tienen el respaldo de discos duros (HDD) que giran y ofrecen un rendimiento confiable para cargas de trabajo de E/S que no dan tanta importancia a la variabilidad del rendimiento, como recursos compartidos de archivos de uso general y entornos de desarrollo y pruebas. Los recursos compartidos de archivos estándar solo están disponibles en un modelo de facturación de pago por uso.
* Los **recursos compartidos de archivos Premium (versión preliminar)** están respaldados por discos en estado sólido (SSD) que proporcionan alto rendimiento y baja latencia de forma consistente en menos de 10 milisegundos en la mayoría de las operaciones de E/S para las cargas de trabajo intensivas con mayor uso de E/S, lo que hace que sean adecuados para una amplia variedad de cargas de trabajo como bases de datos, hospedaje de sitios web, entornos de desarrollo, etc. Los recursos compartidos de archivos Premium solo están disponibles en un modelo de facturación aprovisionada.

### <a name="provisioned-shares"></a>Recursos compartidos aprovisionados
Los recursos compartidos de archivos Premium se aprovisionan en función de una relación fija de GiB/IOPS/rendimiento. Por cada GiB aprovisionado, se generará un IOPS y un rendimiento de 0,1 MiB por segundo en el recurso compartido hasta los límites máximos por recurso compartido. El aprovisionamiento mínimo que se permite es 100 GiB con un IOPS/rendimiento mínimos. El tamaño del recurso compartido se puede aumentar en cualquier momento, pero se puede reducir una vez cada 24 horas desde el último aumento.

En su máximo esfuerzo, todos los recursos compartidos pueden aumentar hasta tres IOPS por GiB de almacenamiento aprovisionado durante 60 minutos, o más, según el tamaño del recurso compartido. Los nuevos recursos compartidos comienzan con todos los créditos de aumento según la capacidad aprovisionada.

| Capacidad aprovisionada | 100 GiB | 500 GiB | 1 TiB | 5 TiB | 
|----------------------|---------|---------|-------|-------|
| IOPS base | 100 | 500 | 1024 | 5120 | 
| Límite de aumento | 300 | 1500 | 3072 | 15 360 | 
| Rendimiento | 110 MiB/s | 150 MiB/s | 202 MiB/s | 612 MiB/s |

## <a name="file-share-redundancy"></a>Redundancia del recurso compartido de archivos
Azure Files admite tres opciones de redundancia de datos: almacenamiento con redundancia local (LRS), almacenamiento con redundancia de zona y almacenamiento con redundancia geográfica (GRS). En las siguientes secciones se describen las diferencias entre las diferentes opciones de redundancia:

### <a name="locally-redundant-storage"></a>Almacenamiento con redundancia local
[!INCLUDE [storage-common-redundancy-LRS](../../../includes/storage-common-redundancy-LRS.md)]

### <a name="zone-redundant-storage"></a>Almacenamiento con redundancia de zona
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-ZRS.md)]

### <a name="geo-redundant-storage"></a>Almacenamiento con redundancia geográfica
[!INCLUDE [storage-common-redundancy-GRS](../../../includes/storage-common-redundancy-GRS.md)]

## <a name="data-growth-pattern"></a>Patrón de crecimiento de datos
Actualmente, el tamaño máximo de un recurso compartido de archivos de Azure es de 5 TiB. Debido a esta limitación actual, debe tener en cuenta el crecimiento esperado de los datos al implementar un recurso compartido de archivos de Azure. 

Es posible sincronizar varios recursos compartidos de archivos de Azure en un único servidor de archivos de Windows con Azure File Sync. Esto permite garantizar que los recursos compartidos de archivos anteriores de gran tamaño que pueda tener en un entorno local se incluyen en Azure File Sync. Para más información, consulte [Planeamiento de una implementación de Azure File Sync](storage-files-planning.md).

## <a name="data-transfer-method"></a>Método de transferencia de datos
Existen muchas opciones sencillas para la transferencia masiva de datos desde un recurso de archivos existente, como un recurso compartido de archivos local, a Azure Files. Algunas populares incluyen (lista no exhaustiva):

* **Azure File Sync**: como parte de una primera sincronización entre un recurso compartido de archivos de Azure (un "punto de conexión de nube") y un espacio de nombres de directorio de Windows (un "punto de conexión de servidor"), Azure File Sync replica todos los datos del recurso compartido de archivos existente en Azure Files.
* **[Azure Import/Export](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)**: el servicio Azure Import/Export permite transferir de forma segura grandes cantidades de datos a un recurso compartido de archivos de Azure mediante el envío de unidades de disco duro a un centro de datos de Azure. 
* **[Robocopy](https://technet.microsoft.com/library/cc733145.aspx)**: Robocopy es una herramienta de copia conocida que se incluye con Windows y Windows Server. Robocopy puede usarse para transferir datos a Azure Files al montar el recurso compartido de archivos localmente y luego usar la ubicación montada como destino en el comando de Robocopy.
* **[AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#upload-files-to-an-azure-file-share)**: AzCopy es una utilidad de línea de comandos diseñada para copiar datos en y desde Azure Files, así como Azure Blob Storage, mediante sencillos comandos con un rendimiento óptimo. AzCopy está disponible para Windows y Linux.

## <a name="next-steps"></a>Pasos siguientes
* [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
* [Implementación de Azure Files](storage-files-deployment-guide.md)
* [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
