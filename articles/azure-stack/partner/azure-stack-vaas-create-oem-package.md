---
title: Procedimientos recomendados para la validación de Azure Stack | Microsoft Docs
description: En este artículo se describen las prácticas recomendadas para usar la validación como servicio.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ROBOTS: NOINDEX
ms.openlocfilehash: a90ec05fa3224033436830e7666c7eb81ff881f6
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247000"
---
# <a name="create-an-oem-package"></a>Creación de un paquete de OEM

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

El paquete de extensión de OEM de Azure Stack es el mecanismo por el cual se agrega contenido específico del OEM a la infraestructura de Azure Stack, para su uso en la implementación, así como en los procesos operativos, como actualización, expansión y reemplazo en el campo.

## <a name="creating-the-package"></a>Creación del paquete

Una vez creado y validado, el paquete de extensión de OEM puede usarse en VaaS.  Antes de continuar, asegúrese de que ha completado los pasos para [crear un paquete de OEM](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true). Después, el paquete se envía a Microsoft junto con los resultados de la prueba de VaaS para iniciar sesión en el flujo de trabajo de validación de la solución. En los siguientes pasos se explica cómo agrupar los archivos generados en un solo archivo ZIP que pueda consumir VaaS.

1. Identifique el siguiente contenido para el paquete:
    - Un ejecutable denominado `<Publisher>-<Model>-<Version>.exe`
    - Uno o varios archivos binarios denominados `<Publisher><Model>-<Version>-#.bin`, donde # es un número secuencial a partir de 1. El número de archivos binarios depende del tamaño total del contenido del paquete.
    - Un archivo de manifiesto denominado `oemMetadata.xml`, que debe tener un contenido idéntico al archivo metadata.xml en la raíz del contenido del paquete.

2. Seleccione los archivos de contenido y cree un archivo ZIP a partir del contenido:

    ![Contenido del archivo ZIP](media/vaas-create-oem-package-1.png) ![Comprimir el contenido del elemento](media/vaas-create-oem-package-2.png)

3. Cambie el nombre del archivo resultante de modo que sea lo suficientemente descriptivo para que pueda identificarlo.

## <a name="verifying-the-contents"></a>Comprobación del contenido

Para validar la estructura del archivo ZIP, inspecciónelo y compruebe que no haya ninguna subcarpeta. El TLD tiene el contenido comprimido. A continuación se muestra una estructura de paquete válida.
> [!IMPORTANT]
> Comprimir la carpeta principal en lugar del contenido provocará un error de firma del paquete.

![Contenido del paquete comprimido correctamente](media/vaas-create-oem-package-3.png)

El archivo ZIP ahora puede cargarse en VaaS, y Microsoft puede firmarlo en el flujo de trabajo de validación de la solución.

## <a name="next-steps"></a>Pasos siguientes

- [Validación de paquetes de OEM](azure-stack-vaas-validate-oem-package.md)
