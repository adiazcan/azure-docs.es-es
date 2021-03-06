---
title: Volver a escribir los encabezados HTTP en una instancia de Azure Application Gateway existente
description: En este artículo se proporciona información sobre cómo volver a escribir los encabezados HTTP en una instancia existente de Azure Application Gateway con Azure PowerShell
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 8429618a945ec70c52b925887790aa469e23ef89
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53730780"
---
# <a name="rewrite-http-headers-in-an-existing-application-gateway"></a>Volver a escribir los encabezados HTTP en una instancia de Application Gateway existente

Puede usar Azure PowerShell para configurar las [reglas para volver a escribir los encabezados de solicitud y respuesta HTTP](rewrite-http-headers.md) en una [SKU de puerta de enlace de aplicaciones de escalabilidad automática y con redundancia de zona](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) existente

> [!IMPORTANT] 
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

En este tutorial, aprenderá a:

> [!div class="checklist"]
>
> * Recuperar la configuración de una puerta de enlace de aplicaciones existente
> * Especificar la configuración de la regla para volver a escribir el encabezado
> * Actualizar la puerta de enlace de aplicaciones con la configuración anterior para volver a escribir los encabezados HTTP

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial es necesario ejecutar Azure PowerShell en local. Debe tener instalado el módulo Azure versión 1.0.0 o posterior. Ejecute `Import-Module Az` y después `Get-Module Az` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Después de verificar la versión de PowerShell, ejecute `Login-AzAccount` para crear una conexión con Azure.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

```azurepowershell
Connect-AzAccount
Select-AzSubscription -Subscription "<sub name>"
```

## <a name="specify-your-http-header-rewrite-rule-configuration"></a>**Especificación de la configuración de la regla para volver a escribir el encabezado**

Configure los nuevos objetos necesarios para volver a escribir los encabezados HTTP:

- **RequestHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de solicitud que va a escribir y el nuevo valor en el que deben volver a escribirse los encabezados originales.
- **ResponseHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de respuesta que va a escribir y el nuevo valor en el que deben volver a escribirse los encabezados originales.
- **ActionSet**: este objeto contiene las configuraciones de los encabezados de solicitud y respuesta especificados anteriormente. 
- **RewriteRule**: este objeto contiene todos los *actionSets* especificados anteriormente. 
- **RewriteRuleSet**: este objeto contiene todos los *rewriteRules* y deberá conectarse a una regla de enrutamiento de solicitud básica o basada en ruta de acceso.

```azurepowershell
$requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-isThroughProxy" -HeaderValue "True"
$responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -HeaderValue "max-age=31536000"
$actionSet = New-AzApplicationGatewayRewriteRuleActionSet -RequestHeaderConfiguration $requestHeaderConfiguration -ResponseHeaderConfiguration $responseHeaderConfiguration    
$rewriteRule = New-AzApplicationGatewayRewriteRule -Name rewriteRule1 -ActionSet $actionSet    
$rewriteRuleSet = New-AzApplicationGatewayRewriteRuleSet -Name rewriteRuleSet1 -RewriteRule $rewriteRule
```

## <a name="retrieve-configuration-of-your-existing-application-gateway"></a>Recuperación de configuración de la puerta de enlace de aplicaciones existente

```azurepowershell
$appgw1 = Get-AzApplicationGateway -Name "AutoscalingAppGw" -ResourceGroupName "<rg name>"
```

## <a name="retrieve-configuration-of-your-existing-request-routing-rule"></a>Recuperación de configuración de la regla de enrutamiento de solicitud existente

```azurepowershell
$reqRoutingRule = Get-AzApplicationGatewayRequestRoutingRule -Name Rule1 -ApplicationGateway $appgw
```

## <a name="update-the-application-gateway-with-the-configuration-for-rewriting-http-headers"></a>Actualización de la puerta de enlace de aplicaciones con la configuración para volver a escribir los encabezados HTTP

```azurepowershell
$appgw1 = Add-AzApplicationGatewayRewriteRuleSet -ApplicationGateway $appgw -Name rewriteRuleSet1 -RewriteRule $rewriteRuleSet.RewriteRules
$appgw2 = Set-AzApplicationGatewayRequestRoutingRule -ApplicationGateway $appgw -Name rule1 -RuleType $reqRoutingRule.RuleType -BackendHttpSettingsId $reqRoutingRule.BackendHttpSettings.Id -HttpListenerId $reqRoutingRule.HttpListener.Id -BackendAddressPoolId $reqRoutingRule.BackendAddressPool.Id -RewriteRuleSetId $rewriteRuleSet.Id
$appgw3 =  Set-AzApplicationGateway -ApplicationGateway $appgw2
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear una puerta de enlace de aplicaciones con reglas de enrutamiento basadas en rutas de direcciones URL](./tutorial-url-route-powershell.md)
