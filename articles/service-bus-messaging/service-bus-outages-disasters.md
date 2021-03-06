---
title: Aislamiento de las aplicaciones de Service Bus de Azure ante interrupciones y desastres | Microsoft Docs
description: Técnicas para proteger las aplicaciones ante una posible interrupción de Service Bus.
services: service-bus-messaging
author: spelluru
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/14/2018
ms.author: spelluru
ms.openlocfilehash: 85481deceeadaf4154659d35fccf777f489bd782
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393714"
---
# <a name="best-practices-for-insulating-applications-against-service-bus-outages-and-disasters"></a>Procedimientos recomendados para aislar aplicaciones ante desastres e interrupciones de Service Bus

Las aplicaciones esenciales deben funcionar de manera continua, incluso si se producen interrupciones imprevistas o desastres. En este artículo se describen técnicas que puede usar para proteger las aplicaciones de Service Bus ante un posible desastre o una interrupción del servicio.

Se define la interrupción como la falta temporal de disponibilidad de Azure Service Bus. La interrupción puede afectar a algunos componentes de Service Bus, como a un almacén de mensajería, o incluso a todo el centro de datos. Una vez corregido el problema, Service Bus vuelva a estar disponible. Normalmente, una interrupción no provoca la pérdida de mensajes ni otros datos. Un ejemplo de error de componente es la falta de disponibilidad de un determinado almacén de mensajería. Un ejemplo de interrupción de todo el centro de datos es un error de alimentación del centro de datos o un conmutador de red defectuoso en él. Una interrupción puede durar desde unos pocos minutos hasta varios días.

Un desastre se define como la pérdida permanente de una unidad de escalado de Service Bus o un centro de datos. El centro de datos no volverá necesariamente a estar disponible. Normalmente, un desastre provoca la pérdida de algunos o todos los mensajes u otros datos. Algunos ejemplos de desastres son los incendios, las inundaciones o los terremotos.

## <a name="current-architecture"></a>Arquitectura actual
Service Bus usa varios almacenes de mensajería para conservar los mensajes que se envían a colas o temas. Se asigna una cola o un tema sin particiones a un almacén de mensajería. Si este almacén de mensajería no está disponible, se producirá un error en todas las operaciones de esa cola o tema.

Todas las entidades de mensajería de Service Bus (colas, temas, retransmisiones) residen en un espacio de nombres de servicio, asociado con un centro de datos. Ahora, Service Bus admite tanto la [*recuperación ante desastres con localización geográfica* como la *replicación geográfica*](service-bus-geo-dr.md) en el nivel del espacio de nombres.

## <a name="protecting-queues-and-topics-against-messaging-store-failures"></a>Protección de colas y temas contra errores en el almacén de mensajería
Se asigna una cola o un tema sin particiones a un almacén de mensajería. Si este almacén de mensajería no está disponible, se producirá un error en todas las operaciones de esa cola o tema. Por otra parte, una cola particionada está formada por varios fragmentos. Cada fragmento se guarda en un almacén de mensajería diferente. Cuando se envía un mensaje a una cola o un tema con particiones, Service Bus asigna el mensaje a uno de los fragmentos. Si el almacén de mensajería correspondiente no está disponible, Service Bus escribe el mensaje en otro fragmento, si es posible. Ya no se admiten las entidades con particiones en la [SKU Premium](service-bus-premium-messaging.md). 

Para obtener más información sobre las entidades con particiones, consulte [Entidades de mensajería con particiones][Partitioned messaging entities].

## <a name="protecting-against-datacenter-outages-or-disasters"></a>Protección contra desastres o interrupciones del centro de datos
Para permitir una conmutación por error entre dos centros de datos, puede crear un espacio de nombres de servicio para Service Bus en cada centro de datos. Por ejemplo, el espacio de nombres de servicio de Service Bus **contosoPrimary.servicebus.windows.net** puede encontrarse en la región norte/central de Estados Unidos, y **contosoSecondary.servicebus.windows.net** puede encontrarse en la región sur/central de EE. UU. Si una entidad de mensajería de Service Bus debe permanecer accesible en el caso de una interrupción del centro de datos, puede crear esa entidad en ambos espacios de nombres.

Para obtener más información, consulte la sección "Error de Service Bus dentro de un centro de datos de Azure" en [Patrones de mensajería asincrónica y alta disponibilidad][Asynchronous messaging patterns and high availability].

## <a name="protecting-relay-endpoints-against-datacenter-outages-or-disasters"></a>Protección de los extremos de retransmisión contra desastres o interrupciones del centro de datos
La replicación geográfica de los extremos de retransmisión permite que un servicio que exponga un extremo de retransmisión sea accesible en caso de interrupciones de Service Bus. Para lograr la replicación geográfica, el servicio debe crear dos extremos de retransmisión en diferentes espacios de nombres. Los espacios de nombres deben residir en distintos centros de datos y ambos extremos deben tener nombres diferentes. Por ejemplo, se puede acceder a un punto de conexión principal en **contosoPrimary.servicebus.windows.net/myPrimaryService**, mientras que su equivalente secundario resulta accesible en **contosoSecondary.servicebus.windows.net/mySecondaryService**.

A continuación, el servicio escucha en ambos extremos y un cliente puede invocar el servicio a través de cualquiera de ellos. Una aplicación cliente selecciona de forma aleatoria una de las retransmisiones como extremo principal y envía su solicitud al extremo activo. Si la operación da como resultado un código de error, este error indica que el extremo de retransmisión no está disponible. La aplicación abre un canal al extremo de reserva y vuelve a emitir la solicitud. En ese momento, el extremo activo y el de reserva intercambian roles: la aplicación cliente considera el anterior extremo activo como el nuevo extremo de reserva y el anterior extremo de reserva pasa a ser el nuevo extremo activo. Si ambas operaciones de envío generan un error, no se modifican los roles de las dos entidades y se devuelve un error.

## <a name="protecting-queues-and-topics-against-datacenter-outages-or-disasters"></a>Protección de colas y temas contra desastres o interrupciones del centro de datos
Para lograr resistencia frente a interrupciones del centro de datos al usar mensajería asincrónica, Service Bus admite dos enfoques: replicación *activa* y *pasiva*. Para cada enfoque, si una cola o un tema determinados deben permanecer accesibles en el caso de una interrupción del centro de datos, puede crear la entidad en ambos espacios de nombres. Ambas entidades pueden tener el mismo nombre. Por ejemplo, se puede acceder a una cola principal en **contosoPrimary.servicebus.windows.net/myQueue**, mientras que su equivalente secundaria resulta accesible en **contosoSecondary.servicebus.windows.net/myQueue**.

Si la aplicación no requiere comunicación permanente entre remitente y receptor, esta puede implementar una cola del lado cliente duradera para evitar la pérdida de mensajes y proteger al remitente ante los errores transitorios de Service Bus.

## <a name="active-replication"></a>Replicación activa
La replicación activa usa entidades en ambos espacios de nombres para cada operación. Cualquier cliente que envíe un mensaje envía dos copias de él. La primera copia se envía a la entidad principal (por ejemplo, **contosoPrimary.servicebus.windows.net/sales**), y la segunda copia del mensaje se envía a la entidad secundaria (por ejemplo, **contosoSecondary.servicebus.windows.net/sales**).

Un cliente recibe mensajes de ambas colas. El receptor procesa la primera copia de un mensaje y se suprime la segunda copia. Para suprimir mensajes duplicados, el remitente debe etiquetar cada mensaje con un identificador único. Ambas copias del mensaje deben estar etiquetadas con el mismo identificador. Puede usar las propiedades [BrokeredMessage.MessageId][BrokeredMessage.MessageId] o [BrokeredMessage.Label][BrokeredMessage.Label], o bien una propiedad personalizada, para etiquetar el mensaje. El receptor debe mantener una lista de los mensajes que ya haya recibido.

El ejemplo de [replicación geográfica con mensajes asincrónicos de Service Bus][Geo-replication with Service Bus Brokered Messages] muestra la replicación activa de entidades de mensajería.

> [!NOTE]
> La replicación activa dobla el número de operaciones; por lo tanto, este enfoque puede suponer un costo mayor.
> 
> 

## <a name="passive-replication"></a>Replicación pasiva
En el caso de que no se hayan producido errores, la replicación pasiva solo usa una de las dos entidades de mensajería. Un cliente envía el mensaje a la entidad activa. Si la operación de la entidad activa genera un código de error que indica que es posible que el centro de datos que hospeda la entidad activa no esté disponible, el cliente envía una copia del mensaje a la entidad de reserva. En ese momento, la entidad activa y la de reserva intercambian roles: el cliente remitente considera la anterior entidad activa como la nueva entidad de reserva y la anterior entidad de reserva pasa a ser la nuevo entidad activa. Si ambas operaciones de envío generan un error, no se modifican los roles de las dos entidades y se devuelve un error.

Un cliente recibe mensajes de ambas colas. Dado que es probable que el receptor reciba dos copias del mismo mensaje, este debe suprimir los mensajes duplicados. Puede suprimir duplicados de la misma manera descrita para la replicación activa.

En general, la replicación pasiva es más económica que la activa porque en la mayoría de los casos se realiza una única operación. La latencia, el rendimiento y el costo económico son idénticos para el escenario sin replicación.

Cuando se usa la replicación pasiva, en los siguientes escenarios se pueden perder mensajes o recibirse dos veces:

* **Demora o pérdida de mensajes**: se supone que el remitente envió correctamente un mensaje m1 a la cola principal y después la cola deja de estar disponible antes de que el receptor reciba m1. El remitente envía otro mensaje m2 a la cola secundaria. Si la cola principal no está disponible temporalmente, el receptor recibe m1 una vez que la cola esté disponible de nuevo. En caso de desastre, puede que el receptor no reciba nunca m1.
* **Recepción duplicada**: supongamos que el remitente envía un mensaje m a la cola principal. Service Bus procesa m correctamente , pero no envía una respuesta. Una vez que la operación de envío agota el tiempo de espera, el remitente envía una copia idéntica de m a la cola secundaria. Si el receptor puede recibir la primera copia de m antes de que la cola principal deje de estar disponible, el receptor recibe ambas copias de m aproximadamente al mismo tiempo. Si el receptor no recibe la primera copia de m antes de que la cola principal deje de estar disponible, el receptor recibe inicialmente solo la segunda copia de m pero, a continuación, recibe una segunda copia de m cuando la cola principal vuelve a estar disponible.

El ejemplo de [replicación geográfica con mensajes asincrónicos de Service Bus][Geo-replication with Service Bus Brokered Messages] muestra la replicación pasiva de entidades de mensajería.

## <a name="geo-replication"></a>Replicación geográfica

Service Bus admite tanto la recuperación ante desastres con localización geográfica como la replicación geográfica en el nivel del espacio de nombres. Para obtener más información, consulte [Recuperación ante desastres con localización geográfica de Azure Service Bus](service-bus-geo-dr.md). La característica de recuperación ante desastres, disponible solo para la [SKU premium](service-bus-premium-messaging.md), implementa la recuperación ante desastres de metadatos y depende de espacios de nombres de recuperación ante desastres principales y secundarios.

## <a name="availability-zones-preview"></a>Availability Zones (versión preliminar)

La SKU de Service Bus Premium es compatible con [Availability Zones](../availability-zones/az-overview.md), lo que proporciona ubicaciones con aislamiento de errores dentro de una región de Azure. 

> [!NOTE]
> La versión preliminar de Availability Zones solo se admite en las regiones **Centro de EE. UU.**, **Este de EE. UU. 2** y **Centro de Francia**.

Solo puede habilitar Availability Zones en los espacios de nombres nuevos mediante Azure Portal. Service Bus no admite la migración de espacios de nombres existentes. No se puede deshabilitar la redundancia de zona después de habilitarla en el espacio de nombres.

![1][]

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información acerca de la recuperación ante desastres, consulte estos artículos:

* [Recuperación ante desastres con localización geográfica de Azure Service Bus](service-bus-geo-dr.md)
* [Continuidad de negocio de SQL Database de Azure][Azure SQL Database Business Continuity]
* [Diseño de aplicaciones resistentes de Azure][Azure resiliency technical guidance]

[Service Bus Authentication]: service-bus-authentication-and-authorization.md
[Partitioned messaging entities]: service-bus-partitioning.md
[Asynchronous messaging patterns and high availability]: service-bus-async-messaging.md#failure-of-service-bus-within-an-azure-datacenter
[BrokeredMessage.MessageId]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId
[BrokeredMessage.Label]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label
[Geo-replication with Service Bus Brokered Messages]: https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoReplication
[Azure SQL Database Business Continuity]: ../sql-database/sql-database-business-continuity.md
[Azure resiliency technical guidance]: /azure/architecture/resiliency

[1]: ./media/service-bus-outages-disasters/az.png