---
title: Informe de validación para Azure Stack | Microsoft Docs
description: Use el informe de Azure Stack Readiness Checker para revisar los resultados de validación.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/23/2018
ms.author: sethm
ms.reviewer: unknown
ms.openlocfilehash: 0eed7943d782bda134c2df2de6cf9eeeb58bfa3b
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2019
ms.locfileid: "54243872"
---
# <a name="azure-stack-validation-report"></a>Informe de validación de Azure Stack
Use la herramienta Azure Stack Readiness Checker para ejecutar validaciones que admitan la implementación y el mantenimiento de un entorno de Azure Stack. La herramienta escribe los resultados en un archivo de informe .json. El informe muestra los datos resumidos y detallados sobre el estado de los requisitos previos para la implementación de Azure Stack. El informe también muestra información acerca de la rotación de secretos para las implementaciones de Azure Stack existentes.  

 ## <a name="where-to-find-the-report"></a>Lugar donde se puede encontrar el informe
Cuando se ejecuta la herramienta, los resultados se registran en **AzsReadinessCheckerReport.json**. La herramienta crea también un registro llamado **AzsReadinessChecker.log**. La ubicación de estos archivos se muestra con los resultados de validación de PowerShell.

![ejecución de la validación](./media/azure-stack-validation-report/validation.png)

Ambos archivos conservan los resultados de comprobaciones de validación posterior que se ejecutan en el mismo equipo.  Por ejemplo, se puede ejecutar la herramienta para validar los certificados, volver a ejecutarla para validar la identidad de Azure y validarla y luego, una tercera vez para validar el registro. Los resultados de las tres validaciones están disponibles en el informe .json resultante.  

De forma predeterminada, los dos archivos se escriben en *C:\Users\<nombre de usuario>\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
- Use el parámetro **-OutputPath** ***&lt;ruta de acceso&gt;***  al final de la línea de comandos de ejecución para especificar otra ubicación para el informe.   
- Use el parámetro **-CleanReport** al final del comando de ejecución para borrar la información de *AzsReadinessCheckerReport.json*. sobre ejecuciones anteriores de la herramienta.

## <a name="view-the-report"></a>Visualización del informe
Para ver el informe en PowerShell, indique la ruta de acceso al informe como valor de **-ReportPath**. Este comando muestra el contenido del informe e identifica las validaciones que aún no tienen resultados.

Por ejemplo, para ver el informe desde un símbolo del sistema de PowerShell que está abierto en la ubicación donde se encuentra el informe, ejecute: 
   > `Read-AzsReadinessReport -ReportPath .\AzsReadinessReport.json` 

La salida debe ser similar a la siguiente:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

Certificate Validation results not available.

############### Registration Validation Summary ###############

Azure Registration Validation results not available.

############### Azure Identity Results ###############

Test                          : ServiceAdministrator
Result                        : OK
AAD Service Admin             : admin@contoso.onmicrosoft.com
Azure Environment             : AzureCloud
Azure Active Directory Tenant : contoso.onmicrosoft.com
Error Details                 : 

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.

############### AzsReadiness Job Summary ###############

Index             : 0
Operations        : 
StartTime         : 2018/10/22 14:24:16
EndTime           : 2018/10/22 14:24:19
Duration          : 3
PSBoundParameters : 
````

## <a name="view-the-report-summary"></a>Visualización del resumen del informe
Para ver un resumen del informe, puede agregar el modificador **-Summary** al final de la línea de comandos de PowerShell. Por ejemplo:  
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -summary`  

El resumen muestra las validaciones que no tienen resultados e indica si las validaciones que están completas son correctas o no. La salida debe ser similar a la siguiente:

````PowerShell
Reading All Validation(s) from Report C:\Contoso-AzsReadinessCheckerReport.json

############### Certificate Validation Summary ###############

    Certificate Validation found no errors or warnings.
    
############### Registration Validation Summary ###############

    Registration Validation found no errors or warnings.

############### Azure Identity Validation Summary ###############

    Azure Identity Validation found no errors or warnings.

############### Azure Stack Graph Validation Summary ###############

Azure Stack Graph Validation results not available.

############### Azure Stack ADFS Validation Summary ###############

Azure Stack ADFS Validation results not available.
````


## <a name="view-a-filtered-report"></a>Vista de un informe filtrado
Para ver un informe que se filtra con un único tipo de validación, use el parámetro **-ReportSections** con uno de los valores siguientes:
- Certificate
- AzureRegistration
- AzureIdentity
- Grafo
- ADFS
- Trabajos   
- Todo  

Por ejemplo, para ver solo el informe resumen de certificados, utilice la siguiente línea de comandos de PowerShell: 
 > `Read-AzsReadinessReport -ReportPath .\Contoso-AzsReadinessReport.json -ReportSections Certificate – Summary`


## <a name="see-also"></a>Otras referencias
