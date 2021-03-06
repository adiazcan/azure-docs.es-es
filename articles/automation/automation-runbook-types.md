---
title: Tipos de runbooks de Azure Automation
description: 'Describe los distintos tipos de Runbooks que puede usar en Azure Automation y las consideraciones que debe tener en cuenta al determinar qué tipo usar. '
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: ad24f53c7ca58756aa4028c8af2e4a83cfcfe76a
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45984329"
---
# <a name="azure-automation-runbook-types"></a>Tipos de runbooks de Azure Automation

Azure Automation admite numerosos tipos de runbooks que se describen brevemente en la tabla siguiente.  Las secciones siguientes proporcionan información adicional sobre cada tipo, incluidas consideraciones sobre cuándo usar cada uno.

| Escriba | DESCRIPCIÓN |
|:--- |:--- |
| [Gráfico](#graphical-runbooks) |Basado en Windows PowerShell y creado y editado completamente en el editor gráfico del Portal de Azure. |
| [Flujo de trabajo gráfico de PowerShell](#graphical-runbooks) |Basado en el flujo de trabajo de Windows PowerShell y creado y editado completamente en el editor gráfico del Portal de Azure. |
| [PowerShell](#powershell-runbooks) |Runbook de texto basado en el script de Windows PowerShell. |
| [Flujo de trabajo de PowerShell](#powershell-workflow-runbooks) |Runbook de texto basado en el flujo de trabajo de Windows PowerShell. |
| [Python](#python-runbooks) |Runbook de texto basado en Python. |

## <a name="graphical-runbooks"></a>Runbooks gráficos

[Gráfico](automation-runbook-types.md#graphical-runbooks) and Gráfico PowerShell Workflow runbooks are created and edited with the graphical editor in the Azure portal.  Puede exportarlos a un archivo y después importarlos en otra cuenta de automatización, pero no puede crearlos ni editarlos con otra herramienta.  Los runbooks gráficos generan código de PowerShell, pero no se puede ver ni modificar el código directamente. Los runbooks gráficos no se pueden convertir a uno de los [formatos de texto](automation-runbook-types.md), ni puede convertirse un runbook de texto a un formato gráfico. Los runbooks gráficos se pueden convertir en runbooks gráficos de flujo de trabajo de PowerShell durante la importación, y viceversa.

### <a name="advantages"></a>Ventajas

* Modelo de creación visual para insertar, vincular y configurar.  
* Se centra en cómo fluyen los datos por el proceso.  
* Representa visualmente los procesos de administración.  
* Permite incluir otros runbooks como runbooks secundarios para crear flujos de trabajo de nivel alto.  
* Anima a usar la programación modular.  

### <a name="limitations"></a>Limitaciones

* No se pueden editar los runbooks fuera del Portal de Azure.
* Puede requerir una actividad de código que contenga código de PowerShell para llevar a cabo una lógica compleja.
* No se puede ver ni modificar directamente el código de PowerShell creado por el flujo de trabajo gráfico. Tenga en cuenta que puede ver el código que cree en las actividades de código.

## <a name="powershell-runbooks"></a>Runbooks de PowerShell

Los runbooks de PowerShell están basados en Windows PowerShell.  Puede modificar directamente el código del runbook con el editor de texto en el Portal de Azure.  También puede usar cualquier editor de texto sin conexión e [importar el runbook](automation-creating-importing-runbook.md) en Azure Automation.

### <a name="advantages"></a>Ventajas

* Implemente toda la lógica compleja con código de PowerShell sin las complejidades adicionales de flujo de trabajo de PowerShell.
* El runbook se inicia con más rapidez que los runbooks de flujo de trabajo de PowerShell, ya que no es necesario compilarlos antes de la ejecución.

### <a name="limitations"></a>Limitaciones

* Debe estar familiarizado con el scripting de PowerShell.
* No puede usar el [procesamiento paralelo](automation-powershell-workflow.md#parallel-processing) para realizar varias acciones en paralelo.
* No puede usar los [puntos de control](automation-powershell-workflow.md#checkpoints) para reanudar el runbook en caso de error.
* Los runbooks gráficos y de flujo de trabajo de PowerShell pueden incluirse únicamente como runbooks secundarios mediante el cmdlet Start-AzureAutomationRunbook, que crea un nuevo trabajo.

### <a name="known-issues"></a>Problemas conocidos

A continuación se describen los problemas conocidos actuales con runbooks de PowerShell.

* Los runbooks de PowerShell no pueden recuperar un [recurso de variables](automation-variables.md) sin cifrar con un valor null.
* Los runbooks de PowerShell no pueden recuperar un [activo de variables](automation-variables.md) con *~* en el nombre.
* Get-Process en un bucle de un runbook de PowerShell puede bloquearse después de más de 80 iteraciones.
* Un runbook de PowerShell puede producir un error si se intenta escribir una gran cantidad de datos en el flujo de salida a la vez.   Se puede evitar este problema si se genera únicamente la información que se necesita al trabajar con objetos grandes.  Por ejemplo, en lugar de generar un elemento *Get-Process*, puede generar simplemente los campos obligatorios con *Get-Process | Select ProcessName, CPU*.

## <a name="powershell-workflow-runbooks"></a>Runbooks del flujo de trabajo de PowerShell

Los runbooks del flujo de trabajo de PowerShell son runbooks de texto basados en el [flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md).  Puede modificar directamente el código del runbook con el editor de texto en el Portal de Azure.  También puede usar cualquier editor de texto sin conexión e [importar el runbook](automation-creating-importing-runbook.md) en Azure Automation.

### <a name="advantages"></a>Ventajas

* Implemente toda la lógica compleja con código del flujo de trabajo de PowerShell.
* Use los [puntos de control](automation-powershell-workflow.md#checkpoints) para reanudar el runbook en caso de error.
* Use el [procesamiento paralelo](automation-powershell-workflow.md#parallel-processing) para realizar varias acciones en paralelo.
* Se pueden incluir otros runbooks gráficos o del flujo de trabajo de PowerShell como runbooks secundarios para crear flujos de trabajo de alto nivel.

### <a name="limitations"></a>Limitaciones

* El autor debe estar familiarizado con el flujo de trabajo de PowerShell.
* El runbook debe tratar la complejidad adicional del flujo de trabajo de PowerShell como [objetos deserializados](automation-powershell-workflow.md#code-changes).
* El runbook tarda más en iniciarse que los runbooks de PowerShell ya que deben compilarse antes de su ejecución.
* Los runbooks de PowerShell pueden incluirse únicamente como runbooks secundarios mediante el cmdlet Start-AzureAutomationRunbook, que crea un nuevo trabajo.

## <a name="python-runbooks"></a>Runbooks de Python

Runbooks de Python compilados en Python 2.  Puede modificar directamente el código del runbook mediante el editor de texto de Azure Portal, o puede usar cualquier editor de texto sin conexión e [importar el runbook](automation-creating-importing-runbook.md) en Azure Automation.

### <a name="advantages"></a>Ventajas

* Usar las sólidas bibliotecas de Python.

### <a name="limitations"></a>Limitaciones

* Debe estar familiarizado con el scripting de Python.
* En este momento solo se admite Python 2, lo que significa que las funciones específicas de Python 3 producirán error.
* Para poder utilizar bibliotecas de terceros, debe [importar el paquete](python-packages.md) a la cuenta de Automation que usará.

## <a name="considerations"></a>Consideraciones

Se deben tener en cuenta las siguientes consideraciones adicionales al determinar qué tipo usar para un runbook determinado.

* No es posible convertir runbooks de tipo gráfico a texto o viceversa.
* Existen limitaciones al usar runbooks de diferentes tipos como runbooks secundarios.  Consulte [Runbooks secundarios en Azure Automation](automation-child-runbooks.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la creación de runbooks de gráficos, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md)
* Para comprender las diferencias entre PowerShell y los flujos de trabajo de PowerShell para runbooks, consulte [Aprendizaje del flujo de trabajo de Windows PowerShell](automation-powershell-workflow.md)
* Para más información sobre cómo crear o importar un Runbook, vea [Crear o importar un Runbook](automation-creating-importing-runbook.md)
