---
title: Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona en Azure (versión preliminar pública) | Microsoft Docs
description: En este artículo se proporciona información acerca de las listas de exclusión y los límites de tamaño de solicitud del firewall de aplicaciones web en Application Gateway con Azure Portal.
documentationcenter: na
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: ''
ms.workload: infrastructure-services
ms.date: 09/26/2018
ms.author: victorh
ms.openlocfilehash: 8fb3dce108b59b8df0d330ec642365d2487eae35
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50085468"
---
# <a name="autoscaling-and-zone-redundant-application-gateway-public-preview"></a>Escalabilidad automática y puerta de enlace de aplicaciones con redundancia de zona (versión preliminar pública)

Application Gateway y Firewall de aplicaciones web (WAF) ahora están disponibles en versión preliminar pública en una nueva SKU v2 que ofrece mejoras de rendimiento y agrega compatibilidad para nuevas características esenciales, como escalabilidad automática, redundancia de zona y compatibilidad con VIP estáticas. La nueva SKU v2 sigue admitiendo las características existentes en la SKU de disponibilidad general, salvo algunas excepciones que se indican en la sección de limitaciones conocidas. La nueva SKU v2 incluye las siguientes mejoras:

- **Escalabilidad automática**: las implementaciones de Application Gateway o WAF en la SKU de escalabilidad automática pueden ampliarse o reducirse verticalmente en función de los cambios de tráfico. La escalabilidad automática también elimina el requisito de tener elegir un tamaño de implementación o un número de instancias durante el aprovisionamiento. Por lo tanto, la SKU ofrece verdadera elasticidad. En la nueva SKU, Application Gateway puede funcionar con una capacidad fija (escalabilidad automática deshabilitada) o en modo de escalabilidad automática habilitada. El modo de capacidad fija es útil para escenarios con cargas de trabajo coherentes y predecibles. El modo de escalabilidad automática es útil en aplicaciones que tendrán una gran variación en el tráfico de las aplicaciones.
   
   > [!NOTE]
   > La escalabilidad automática no está actualmente disponible para la SKU de WAF. Configure WAF con el modo de capacidad fija en lugar del modo de escalabilidad automática.
- **Redundancia de zona**: una implementación de Application Gateway o WAF ahora puede abarcar varias zonas de disponibilidad, lo que elimina la necesidad de aprovisionar y anclar distintas instancias de Application Gateway en cada zona con Traffic Manager. Puede elegir una o varias zonas donde se implementarán las instancias de Application Gateway, lo que garantiza la resistencia a errores de zona. El grupo de back-end de las aplicaciones se puede distribuir de manera similar entre las zonas de disponibilidad.
- **Mejoras de rendimiento**: la SKU de escalabilidad automática ofrece un rendimiento cinco veces superior en la descarga de SSL en comparación con la SKU de disponibilidad general.
- **Menores tiempos de implementación y actualización**: la SKU de escalabilidad automática proporciona menores tiempos de implementación y actualización en comparación con la SKU de disponibilidad general.
- **VIP estática**: la dirección IP virtual de la puerta de enlace de aplicaciones ahora admite exclusivamente el tipo VIP estática. Con esto se garantiza que la VIP asociada a la puerta de enlace de aplicaciones no cambiará tras un reinicio.

> [!IMPORTANT]
> La SKU de escalabilidad automática y de puerta de enlace de aplicaciones con redundancia de zona actualmente está en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

![](./media/application-gateway-autoscaling-zone-redundant/application-gateway-autoscaling-zone-redundant.png)

## <a name="supported-regions"></a>Regiones admitidas
La SKU de escalabilidad automática está disponible en las regiones Este de EE. UU. 2, Centro de EE. UU., Oeste de EE. UU. 2, Centro de Francia, Europa Occidental y Sudeste Asiático.

## <a name="pricing"></a>Precios
Durante la versión preliminar, no hay ningún cargo. Se le cobrarán los recursos que no sean la puerta de enlace de aplicaciones, como el almacén de claves, las máquinas virtuales, etcétera. 

## <a name="known-issues-and-limitations"></a>Problemas conocidos y limitaciones

|Problema|Detalles|
|--|--|
|Certificado de autenticación|No compatible.<br>Para más información, consulte [Introducción a SSL de un extremo a otro con Application Gateway](ssl-overview.md#end-to-end-ssl-with-the-v2-sku).|
|Combinación de Application Gateway Standard y Standard_v2 en la misma subred|No compatible.<br>Además, si está habilitado el escalado automático, una subred puede tener solo una puerta de enlace de aplicaciones.|
|Ruta definida por el usuario (UDR) en la subred de Application Gateway|No compatible|
|NSG para el intervalo de puertos de entrada| - 65200 a 65535 para la SKU Standard_v2<br>- 65503 a 65534 para la SKU Standard.<br>Para más información, consulte las [preguntas más frecuentes](application-gateway-faq.md#are-network-security-groups-supported-on-the-application-gateway-subnet).|
|Registros de rendimiento en Azure Diagnostics|No compatible.<br>Se deben usar las métricas de Azure.|
|Facturación|No hay facturación actualmente.|
|Modo FIPS, WebSocket|Actualmente no se admiten.|
|Solo modo ILB|Actualmente no se admite. Se admiten los modos público e ILB juntos.|
|Escalabilidad automática de firewall de aplicaciones web|WAF no admite el modo de escalabilidad automática. Se admite el modo de capacidad fija.|

## <a name="next-steps"></a>Pasos siguientes
- [Creación de una puerta de enlace de aplicaciones con redundancia de zona, escalabilidad automática y una dirección IP virtual reservada con Azure PowerShell](tutorial-autoscale-ps.md)
- Más información acerca de [Application Gateway](overview.md).
- Más información acerca de [Azure Firewall](../firewall/overview.md). 

