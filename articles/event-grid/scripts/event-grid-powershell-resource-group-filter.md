---
title: 'Ejemplo de script de Azure PowerShell: Suscripción a un grupo de recursos y filtrado por recurso | Microsoft Docs'
description: 'Ejemplo de script de Azure PowerShell: Suscripción a un grupo de recursos y filtrado por recurso'
services: event-grid
documentationcenter: na
author: tfitzmac
ms.service: event-grid
ms.devlang: powershell
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2018
ms.author: tomfitz
ms.openlocfilehash: 21719ab4e6b999f262ff53adf31d855d6e1833b4
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271409"
---
# <a name="subscribe-to-events-for-a-resource-group-and-filter-for-a-resource-with-powershell"></a>Suscripción a eventos de un grupo de recursos y filtrado de un recurso con PowerShell

Este script crea una suscripción de Event Grid a los eventos de un grupo de recursos. Utiliza un filtro para obtener solo los eventos de un recurso especificado en el grupo de recursos.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

El script de ejemplo de la versión preliminar requiere el módulo de Event Grid. Para instalarlo, ejecute `Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`.

## <a name="sample-script---stable"></a>Script de ejemplo: estable

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events/filter-events.ps1 "Filter events")]

## <a name="sample-script---preview-module"></a>Script de ejemplo: módulo de la versión preliminar

[!code-powershell[main](../../../powershell_scripts/event-grid/filter-events-preview/filter-events-preview.ps1 "Filter events")]


## <a name="script-explanation"></a>Explicación del script

Este script usa el siguiente comando para crear la suscripción de eventos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [New-AzureRmEventGridSubscription](https://docs.microsoft.com/powershell/module/azurerm.eventgrid/new-azurermeventgridsubscription) | Cree una suscripción de Event Grid. |

## <a name="next-steps"></a>Pasos siguientes

* Para una introducción a las aplicaciones administradas, consulte la [introducción a las aplicaciones administradas de Azure](../overview.md).
* Para más información sobre PowerShell, consulte la [documentación de Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
