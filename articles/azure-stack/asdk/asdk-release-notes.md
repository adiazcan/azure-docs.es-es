---
title: Notas de la versión del Kit de desarrollo de Microsoft Azure Stack | Microsoft Docs
description: Mejoras, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/21/2018
ms.author: sethm
ms.reviewer: misainat
ms.openlocfilehash: d1ff154c42709f0c672b30f7ec51a436fb44ce13
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724749"
---
# <a name="asdk-release-notes"></a>Notas de la versión del Kit de desarrollo de Azure Stack 
 
En este artículo se proporciona información sobre las mejoras, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack (ASDK). Si no está seguro de qué versión se está ejecutando, puede usar el [portal de administración](../azure-stack-updates.md#determine-the-current-version).

> Para estar al día de las novedades del ASDK, suscríbase a esta [![fuente](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-118110101"></a>Compilación 1.1811.0.101

### <a name="new-features"></a>Nuevas características

Esta compilación incluye las siguientes correcciones y mejoras para Azure Stack:  

- Hay un conjunto de requisitos mínimos y recomendados nuevos en hardware y software para el ASDK. Estas nuevas especificaciones recomendadas se documentan en las [Consideraciones de planeación de la implementación de Azure Stack](asdk-deploy-considerations.md). A medida que ha evolucionado la plataforma de Azure Stack, más servicios están ahora disponibles y pueden requerir más recursos. El aumento de especificaciones refleja estas recomendaciones revisadas.

### <a name="fixed-issues"></a>Problemas corregidos

<!-- TBD - IS ASDK --> 
- Se ha corregido un problema en el que la dirección IP pública mostraba el mismo valor **EventDateTime** para cada registro en lugar de la marca **TimeDate** que se muestra cuándo se creó el registro. Ya puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.

<!-- 3099544 – IS, ASDK --> 
- Se ha corregido un problema que se producía al crear una máquina virtual (VM) nueva mediante el portal de Azure Stack. Seleccionar el tamaño de la máquina virtual hacía que la columna USD/mes mostrara el mensaje **No disponible**. Esta columna ya no aparece, ya que en Azure Stack no se permite mostrar la columna de precios de la máquina virtual.

<!-- 2930718 - IS ASDK --> 
- Se ha corregido un problema en el que el portal de administrador, al acceder a los detalles de cualquier suscripción de usuario después de cerrar la hoja y hacer clic en **Recientes**, no mostraba el nombre de la suscripción de usuario. Ahora aparece el nombre de la suscripción de usuario.

<!-- 3060156 - IS ASDK --> 
- Se ha corregido un problema en los portales de administrador y usuario: hacer clic en la configuración del portal y seleccionar **Eliminar todas las opciones de configuración y los paneles privados** no funcionaba según lo previsto y se mostraba una notificación de error. 

<!-- 2930799 - IS ASDK --> 
- Se ha corregido un problema en los portales de administrador y de usuario: en **Todos los servicios**, el recurso **Plan de DDoS Protection** se mostraba de forma incorrecta aunque no está disponible en Azure Stack.
 
<!--2760466 – IS  ASDK --> 
- Se ha corregido un problema en el que, cuando se instala un nuevo entorno de Azure Stack, no se mostraba la alerta de **Activación necesaria**. Ahora se muestra correctamente.

<!--1236441 – IS  ASDK --> 
- Se ha corregido un problema que impedía aplicar las directivas RBAC a un grupo de usuarios cuando se usa ADFS.

<!--3463840 - IS, ASDK --> 
- Se ha corregido un problema en el que las copias de seguridad de infraestructura contenían errores debido a que no se podía acceder al servidor de archivos desde la red VIP pública. Esta corrección traslada el servicio de copia de seguridad de infraestructura de nuevo a la red de infraestructura pública. Si ha aplicado la [revisión más reciente de Azure Stack para 1809](#azure-stack-hotfixes) que soluciona este problema, la actualización 1811 no realizará ninguna modificación adicional. 

<!-- 2967387 – IS, ASDK --> 
- Se ha corregido un problema en el que la cuenta que se usa para iniciar sesión en el portal de administrador o usuario de Azure Stack se mostraba como **Usuario no identificado**. Este mensaje se mostraba cuando en la cuenta no se había especificado un *nombre* o un *apellido*.   

<!--  2873083 - IS ASDK --> 
- Se ha corregido un problema en el que, al usar el portal para crear un conjunto de escalado de máquinas virtuales (VMSS), la lista desplegable *Tamaño de instancia* no se cargaba correctamente con Internet Explorer. Este explorador ahora funciona correctamente.  

<!-- 3190553 - IS ASDK -->
- Se ha corregido un problema que generaba alertas falsas que indicaban que una instancia de rol de infraestructura no estaba disponible o que el nodo de la unidad de escala estaba sin conexión.

### <a name="known-issues"></a>Problemas conocidos

#### <a name="portal"></a>Portal  

<!-- 2930820 - IS ASDK --> 
- En los portales de administrador y de usuario, si se busca "Docker", el elemento se devuelve de forma incorrecta. No está disponible en Azure Stack. Si se intenta crearlo, se muestra una hoja con una indicación de error. 

<!-- 2931230 – IS  ASDK --> 
- No se pueden eliminar los planes que se agregan a una suscripción de usuario como planes complementarios, aunque se quite el plan de la suscripción de usuario. El plan permanecerá hasta que también se eliminen las suscripciones a las que haga referencia el plan complementario. 

<!-- TBD - IS ASDK --> 
- Los dos tipos de suscripción administrativa que se incluyeron con la versión 1804 no deberían usarse. Los tipos de suscripción son **suscripción de medición** y **suscripción de consumo**. Estos tipos de suscripción están visibles en los nuevos entornos de Azure Stack a partir de la versión 1804 pero aún no están listos para su uso. Tendrá que seguir usando el tipo de suscripción del **proveedor predeterminado**.

<!-- TBD - IS ASDK --> 
- La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

<!-- TBD - IS ASDK --> 
- No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.

#### <a name="compute"></a>Proceso 

<!-- 3235634 – IS, ASDK -->
- Para implementar máquinas virtuales con tamaños que contiene un sufijo **v2**; por ejemplo, **Standard_A2_v2**, especifique el sufijo como **Standard_A2_v2** (v minúscula). No use **Standard_A2_V2** (v mayúscula). Esto funciona en Azure global y es una incoherencia en Azure Stack.

<!-- 2869209 – IS, ASDK --> 
- Cuando se usa el [cmdlet **Add-AzsPlatformImage**](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), se debe usar el parámetro **-OsUri** como el URI de la cuenta de almacenamiento donde se ha cargado el disco. Si usa la ruta de acceso local del disco, se produce el siguiente error en el cmdlet: *Long running operation failed with status ‘Failed’* (Se produjo un problema en la operación de larga ejecución con el estado "Error"). 

<!--  2795678 – IS, ASDK --> 
- Cuando usa el portal para crear máquinas virtuales (VM) en un tamaño de máquina virtual prémium (DS, Ds_v2, FS, FSv2), la máquina virtual se crea en una cuenta de almacenamiento estándar. La creación en una cuenta de almacenamiento estándar no afecta al funcionamiento, E/S por segundo ni la facturación. Puede omitir la siguiente advertencia sin problemas: **Ha elegido usar un disco estándar en un tamaño que admite discos prémium. Esto no se recomienda ya que podría afectar al rendimiento del sistema operativo. Considere la opción de usar Premium Storage (SSD).**

<!-- 2967447 - IS, ASDK --> 
- La experiencia de creación de conjuntos de escalado de máquinas virtuales (VMSS) proporciona la versión 7.2 basada en CentOS como una opción para la implementación. Como esa imagen no está disponible en Azure Stack, seleccione otro sistema operativo para la implementación o use una plantilla de Azure Resource Manager en la que se especifique otra imagen de CentOS que el operador haya descargado de Marketplace antes de la implementación.  

<!-- TBD - IS ASDK --> 
- Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

<!-- 1662991 IS ASDK --> 
- No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico.  

<!-- 2724961- IS ASDK --> 
- Al registrar el proveedor de recursos **Microsoft.Insight** en la configuración de la suscripción y crear una máquina virtual Windows con el diagnóstico del sistema operativo invitado habilitado, el gráfico de porcentaje de CPU de la página de información general de la máquina virtual no muestra los datos de métricas.

   Para buscar datos de métricas, como el gráfico de porcentaje de CPU para la máquina virtual, vaya a la ventana Métrica y muestre todas las métricas de invitado de las máquinas virtuales Windows admitidas.

<!-- 3507629 - IS, ASDK --> 
- Managed Disks crea dos nuevos [tipos de cuota de proceso](../azure-stack-quota-types.md#compute-quota-types) para limitar la capacidad máxima de los discos administrados que se pueden aprovisionar. De forma predeterminada, se asignan 2 048 GiB para cada tipo de cuota de discos administrados. Sin embargo, es posible que encuentre los siguientes problemas:

   - Para las cuotas creadas antes de la actualización 1808, la cuota de Managed Disks mostrará valores 0 en el portal de administrador, aunque se han asignado 2 048 GiB. Puede aumentar o disminuir el valor según sus necesidades reales y el valor de la cuota recién establecido invalida el predeterminado de 2 048 GiB.
   - Si actualiza el valor de la cuota a 0, equivale al valor predeterminado de 2 048 GiB. Como alternativa, establezca el valor de la cuota en 1.

<!-- TBD - IS ASDK --> 
- Después de aplicar la actualización 1811, se pueden producir los problemas siguientes al implementar máquinas virtuales con Managed Disks:

   1. Si la suscripción se creó antes de la actualización 1808, se puede producir un error en la implementación de máquinas virtuales con Managed Disks con un mensaje de error interno. Para resolver el error, siga estos pasos en cada suscripción:
      1. En el portal del inquilino, vaya a **Suscripciones** y busque la suscripción. Haga clic en **Proveedores de recursos**, después en **Microsoft.Compute** y luego en **Volver a registrar**.
      2. En la misma suscripción, vaya a **Control de acceso (IAM)**, y compruebe que **Azure Stack – Managed Disk** (Azure Stack - Disco administrado) aparece en la lista.
   2. Si ha configurado un entorno de varios inquilinos, se puede producir un error con un mensaje de error interno en la implementación de máquinas virtuales en una suscripción asociada con un directorio de invitados. Para solucionar el error, siga los pasos de [este artículo](../azure-stack-enable-multitenancy.md#registering-azure-stack-with-the-guest-directory) para volver a configurar cada uno de los directorios de invitado.

#### <a name="networking"></a>Redes

<!-- 1766332 - IS ASDK --> 
- En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Basada en directivas** como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

<!-- 1902460 - IS ASDK --> 
- Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

<!-- 16309153 - IS ASDK --> 
- En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.

<!-- 2529607 - IS ASDK --> 
- Durante la *rotación secreta* de Azure Stack, hay un período en el que las direcciones IP públicas a nivel de instancia no son accesibles entre dos y cinco minutos.

<!-- 2664148 - IS ASDK --> 
-   En los escenarios en los que el inquilino tenga acceso a sus máquinas virtuales mediante un túnel VPN S2S, podría encontrarse con que los intentos de conexión producen un error si la subred local se ha agregado a la puerta de enlace de red local una vez que la puerta de enlace ya está creada. 

#### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

<!-- #### Usage -->  

<!-- #### Identity -->

## <a name="build-11809090"></a>Compilación 1.1809.0.90

### <a name="new-features"></a>Nuevas características
Esta compilación incluye las siguientes correcciones y mejoras para Azure Stack.  

<!--  2712869   | IS  ASDK -->  
- **Cliente de Syslog de Azure Stack (disponibilidad general)** Este cliente permite el reenvío de auditorías, alertas y registros de seguridad relacionados con la infraestructura de Azure Stack a un servidor de Syslog o a un software de información de seguridad y administración de eventos (SIEM) que es externo a Azure Stack. El cliente de Syslog ahora permite especificar el puerto en el que escucha el servidor de Syslog.

Con esta versión, el cliente de Syslog está disponible con carácter general y se puede usar en entornos de producción.

Para más información, consulte [Reenvío de syslog de Azure Stack](../azure-stack-integrate-security.md).

### <a name="fixed-issues"></a>Problemas corregidos

<!-- TBD - IS ASDK -->
- En el portal, el gráfico de memoria que notifica la capacidad libre y usada ahora es preciso. Ya puede predecir de manera más confiable cuántas máquinas virtuales puede crear.

<!-- TBD - IS ASDK --> 
- Se ha corregido un problema en el que se creaban máquinas virtuales en el portal de usuarios de Azure Stack y el portal mostraba un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie DS. Las máquinas virtuales de la serie DS pueden albergar tantos discos de datos como la configuración de Azure.

- <!-- 2702741 -  IS, ASDK --> Se ha corregido el problema por el cual no se garantizaba que las direcciones IP públicas que se implementaban mediante el método de asignación dinámica se conservasen después de emitirse una detención o desasignación. Ahora ya se conservan.

- <!-- 3078022 - IS, ASDK --> Si una máquina virtual se detenía o desasignaba con una actualización anterior a la 1808, no se podía reasignar después de la actualización 1808.  Este problema se ha corregido en la actualización 1809. Con esta corrección, las instancias que se encontraban en este estado y no se podían iniciar ya se pueden iniciar en la actualización 1809. La corrección también impide que este problema vuelva a ocurrir.

<!-- TBD -  IS ASDK --> 
- Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

- **Varias revisiones** de rendimiento, estabilidad, seguridad y el sistema operativo que se usa en Azure Stack.

### <a name="changes"></a>Cambios

### <a name="known-issues"></a>Problemas conocidos

#### <a name="portal"></a>Portal  

<!-- 2515955   | IS ,ASDK--> 
- *Todos los servicios* reemplaza a *Más servicios* en los portales de administración y del usuario de Azure Stack. Ahora se puede usar *Todos los servicios* como alternativa al desplazamiento en los portales de Azure Stack de la misma manera que en los portales de Azure.

<!--  TBD – IS, ASDK --> 
- Se han retirado los tamaños de máquina virtual *A básico* para la [creación de conjuntos de escalado de máquinas virtuales](../azure-stack-compute-add-scalesets.md) (VMSS) a través del portal. Para crear un VMSS con este tamaño, use PowerShell o una plantilla.

#### <a name="health-and-monitoring"></a>Estado y supervisión

<!-- 1264761 - IS ASDK -->  
- Es posible que vea alertas del componente *Controlador de mantenimiento* con los siguientes detalles:  

   Alerta 1:
   - NOMBRE:  rol de infraestructura incorrecto
   - GRAVEDAD: Advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Heartbeat Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.  

  Alerta 2:
   - NOMBRE:  rol de infraestructura incorrecto
   - GRAVEDAD: Advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Fault Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.

  Se pueden omitir ambas alertas con seguridad y se cerrarán automáticamente con el tiempo.  

<!-- 2368581 - IS. ASDK --> 
- Operador de Azure Stack: si recibe una alerta de memoria insuficiente y no se pueden implementar las máquinas virtuales del inquilino debido a un *error de creación de máquina virtual de Fabric*, es posible que la marca de Azure Stack supere la memoria disponible. Use la [herramienta de planeamiento de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para comprender mejor la capacidad disponible para las cargas de trabajo.


#### <a name="compute"></a>Proceso 

<!-- 3164607 – IS, ASDK -->
- Si se vuelve a conectar un disco desasociado a la misma máquina virtual (VM) con el mismo nombre y LUN, se produce un error parecido a **No se puede conectar el disco de datos "datadisk" a la VM 'vm1'**. El error se produce porque el disco se está desasociando en este momento o porque no se pudo realizar la última operación de desasociación. Espere hasta que el disco esté completamente desasociado para volver a intentarlo o elimine/desasocie de nuevo el disco de forma explícita. La solución alternativa consiste en volver a conectarlo con un nombre diferente o en otro LUN. 

<!-- 3235634 – IS, ASDK -->
- Para implementar máquinas virtuales con tamaños que contiene un sufijo **v2**; por ejemplo, **Standard_A2_v2**, especifique el sufijo como **Standard_A2_v2** (v minúscula). No use **Standard_A2_V2** (v mayúscula). Esto funciona en Azure global y es una incoherencia en Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Cuando cree una máquina virtual mediante el portal de Azure Stack y seleccione el tamaño de la máquina, se mostrará la columna USD/mes con el mensaje **No disponible**. Esta columna no debería aparecer, ya que en Azure Stack no se permite mostrar la columna de precios de la máquina virtual.

<!-- 2869209 – IS, ASDK --> 
- Cuando se usa el [cmdlet **Add-AzsPlatformImage**](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), se debe usar el parámetro **-OsUri** como el URI de la cuenta de almacenamiento donde se ha cargado el disco. Si usa la ruta de acceso local del disco, se produce el siguiente error en el cmdlet: *Long running operation failed with status ‘Failed’* (Se produjo un problema en la operación de larga ejecución con el estado "Error"). 

<!--  2966665 – IS, ASDK --> 
- Al conectar discos de datos SSD a máquinas virtuales de discos administrados de tamaño prémium (DS, DSv2, Fs y Fs_V2), se produce el siguiente error:  *Error al actualizar discos para la máquina virtual "vmName". Error: No se puede realizar la operación solicitada porque el tipo de cuenta de almacenamiento "Premium_LRS" no es compatible con el tamaño de VM "Standard_DS/Ds_V2/FS/Fs_v2"*

   Para solucionar este problema, use discos de datos *Standard_LRS* en lugar de *discos Premium_LRS*. El uso de discos de datos *Standard_LRS* no cambia el costo de E/S por segundo ni de facturación.  

<!--  2795678 – IS, ASDK --> 
- Cuando usa el portal para crear máquinas virtuales (VM) en un tamaño de máquina virtual prémium (DS, Ds_v2, FS, FSv2), la máquina virtual se crea en una cuenta de almacenamiento estándar. La creación en una cuenta de almacenamiento estándar no afecta al funcionamiento, E/S por segundo ni la facturación. 

   Puede omitir la siguiente advertencia sin problemas: *Ha elegido usar un disco estándar en un tamaño que admite discos prémium. Esto no se recomienda ya que podría afectar al rendimiento del sistema operativo. Considere la opción de usar Premium Storage (SSD).*

<!-- 2967447 - IS, ASDK --> 
- La experiencia de creación de conjuntos de escalado de máquinas virtuales (VMSS) proporciona la versión 7.2 basada en CentOS como una opción para la implementación. Como esa imagen no está disponible en Azure Stack, seleccione otro sistema operativo para la implementación o use una plantilla de Azure Resource Manager en la que se especifique otra imagen de CentOS que el operador haya descargado de Marketplace antes de la implementación.

<!-- TBD -  IS ASDK --> 
- Los valores de escalado para conjuntos de escalas de máquina virtual no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.



<!-- TBD -  IS ASDK --> 
- Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

<!-- 1662991 - IS ASDK --> 
- No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico.

<!-- 2724961- IS ASDK --> 
- Al registrar el proveedor de recursos **Microsoft.Insight** en la configuración de la suscripción y crear una máquina virtual Windows con el diagnóstico del SO invitado habilitado, el gráfico Porcentaje de CPU en la página de información general de la máquina virtual no podrá mostrar los datos de métricas.
 
  Para buscar el gráfico Porcentaje de CPU para la máquina virtual, vaya a la hoja **Métrica** y muestre todas las métricas de invitado de las máquinas virtuales de Windows admitidas.

 

#### <a name="networking"></a>Redes
<!-- 1766332 - IS, ASDK --> 
- En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Basada en directivas** como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

<!-- 1902460 -  IS ASDK --> 
- Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

<!-- 16309153 -  IS ASDK --> 
- En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.

<!-- 2702741 -  IS ASDK --> 
- No se garantiza que las direcciones IP públicas que se implementan mediante el método de asignación dinámica se conserven después de emitirse una detención o desasignación.

<!-- 2529607 - IS ASDK --> 
- Durante la *rotación secreta* de Azure Stack, hay un período en el que las direcciones IP públicas a nivel de instancia no son accesibles entre dos y cinco minutos.

<!-- 2664148 - IS ASDK --> 
- En los escenarios en los que el inquilino tenga acceso a sus máquinas virtuales mediante un túnel VPN S2S, podría encontrarse con que los intentos de conexión producen un error si la subred local se ha agregado a la puerta de enlace de red local una vez que la puerta de enlace ya está creada. 


<!--  #### SQL and MySQL  -->


#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

<!-- TBD - IS ASDK --> 
- Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de proceso.  


#### <a name="usage"></a>Uso  
<!-- TBD -  IS ASDK --> 
- El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.

<!-- #### Identity -->

## <a name="build-11808097"></a>Compilación 1.1808.0.97

### <a name="new-features"></a>Nuevas características
Esta compilación incluye las siguientes correcciones y mejoras para Azure Stack.  

<!-- 1658937 | ASDK, IS --> 
- **Iniciar copias de seguridad según una programación predefinida**: como un dispositivo, Azure Stack ahora puede desencadenar de forma automática las copias de seguridad de infraestructura periódicamente para eliminar la intervención humana. Azure Stack también limpiará automáticamente el recurso compartido externo para las copias de seguridad anteriores al período de retención definido. Para más información, consulte [Habilitación de la copia de seguridad de Azure Stack con PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 2496385 | ASDK, IS -->  
- **Tiempo de transferencia de datos agregado al tiempo total de copia de seguridad.** Para más información, consulte [Habilitación de la copia de seguridad de Azure Stack con PowerShell](../azure-stack-backup-enable-backup-powershell.md).

<!-- 1702130 | ASDK, IS --> 
- **La capacidad externa de copia de seguridad ahora muestra la capacidad correcta del recurso compartido externo.** (Anteriormente esto se codificada de forma rígida a 10 GB). Para más información, consulte [Habilitación de la copia de seguridad de Azure Stack con PowerShell](../azure-stack-backup-enable-backup-powershell.md).
 
<!-- 2753130 |  IS, ASDK   -->  
- **Las plantillas de Azure Resource Manager admiten ahora el elemento de condición**: ahora puede implementar un recurso en una plantilla de Azure Resource Manager con una condición. Puede diseñar la plantilla para implementar un recurso en función de una condición, por ejemplo, evaluando si un valor de un parámetro está presente. Para información acerca de cómo usar una plantilla como una condición, consulte [Implementación condicional de un recurso en una plantilla de Azure Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/conditional-deploy) y [sección Variables de plantillas de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-templates-variables) en la documentación de Azure. 

   También puede usar plantillas para [implementar recursos en varias suscripciones o grupos de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-cross-resource-group-deployment).  

<!--2753073 | IS, ASDK -->  
- **Se ha actualizado la compatibilidad con la versión de recursos de la API Microsoft.Network** para incluir compatibilidad con la versión de la API 2017-10-01 desde 2015-06-15 para los recursos de red de Azure Stack. La compatibilidad con las versiones de recursos entre 2017-10-01 y 2015-06-15 no se incluye en esta versión. Consulte en [Consideraciones para las redes de Azure Stack](../user/azure-stack-network-differences.md) las diferencias de funcionalidad.

<!-- 2272116 | IS, ASDK   -->  
- **Azure Stack ha agregado compatibilidad con las búsquedas inversas de DNS para los puntos de conexión de infraestructura de Azure Stack que se usan externamente** (es decir, para portal, adminportal, management y adminmanagement). Esto permite que los nombres de puntos de conexión externos de Azure Stack se puedan resolver desde una dirección IP.

<!-- 2780899 |  IS, ASDK   --> 
- **Azure Stack ahora admite agregar interfaces de red adicionales a una máquina virtual existente.**  Esta funcionalidad está disponible mediante el portal, PowerShell y la CLI. Para más información, consulte [Incorporación de interfaces de red a máquinas virtuales o su eliminación de ellas](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface-vm) en la documentación de Azure. 

<!-- 2222444 | IS, ASDK   -->  
- **Se han realizado mejoras en la precisión y la resistencia en medidores de uso de la red.** Los medidores de uso de la red ahora son más precisos y tienen en cuenta las suscripciones suspendidas, los períodos de interrupción y las condiciones de carrera.

<!-- 2297790 | IS, ASDK -->  
- **Mejoras en el cliente de Syslog (característica en vista previa) de Azure Stack**. Este cliente permite el reenvío de la auditoría y los registros relacionados con la infraestructura de Azure Stack a un servidor de syslog o a un software de administración de eventos e información de seguridad (SIEM) que es externo a Azure Stack. El cliente de syslog ahora es compatible con el protocolo TCP con texto sin formato o el cifrado de TLS 1.2; el último es la configuración predeterminada. Puede configurar la conexión TLS con autenticación solo de servidor o mutua.

  Para configurar el modo en que el cliente de syslog se comunica (como el protocolo, el cifrado y la autenticación) con el servidor de syslog, use el cmdlet Set-SyslogServer. Este cmdlet está disponible desde el punto de conexión con privilegios (PEP). 

- <!-- ASDK --> **Los elementos de la galería para Virtual Machine Scale Sets ahora están integrados**.  Los elementos de la galería Virtual Machine Scale Set ahora están disponible en los portales de usuario y administrador sin necesidad de descargarlo. 

- <!-- IS, ASDK --> **Escalado de conjuntos de escalado de máquinas virtuales**.  Puede usar el portal para [escalar un conjunto de escalado de máquinas virtuales](../azure-stack-compute-add-scalesets.md#scale-a-virtual-machine-scale-set) (VMSS).   

- <!-- 2489570 | IS ASDK--> **Compatibilidad con configuraciones de directiva IPSec/IKE personalizadas** para [puertas de enlace de VPN en Azure Stack](/azure/azure-stack/azure-stack-vpn-gateway-about-vpn-gateways).

- <!-- | IS ASDK--> **Elemento de Marketplace de Kubernetes**. Ahora puede implementar clústeres de Kubernetes con el [Elemento de Marketplace de Kubernetes](/azure/azure-stack/azure-stack-solution-template-kubernetes-cluster-add). Los usuarios pueden seleccionar el elemento de Kubernetes y rellenar varios parámetros para implementar un clúster de Kubernetes en Azure Stack. El propósito de las plantillas es facilitar a los usuarios la configuración de implementaciones de Kubernetes de desarrollo y pruebas en unos pocos pasos.

<!-- ####### | IS, ASDK -->  
- **Azure Resource Manager incluye el nombre de la región.** Con esta versión, los objetos recuperados desde Azure Resource Manager ahora incluirá el atributo de nombre de región. Si un script de PowerShell existente pasa directamente el objeto a otro cmdlet, el script puede producir un error. Este es el comportamiento compatible de Azure Resource Manager y requiere que el cliente que realiza la llamada quite el atributo de región. Para más información acerca de Azure Resource Manager, consulte la [documentación de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/).

<!-- TBD | IS, ASDK -->  
- **Paso de suscripciones entre proveedores delegados.** Ahora puede pasar suscripciones entre suscripciones de proveedor delegado nuevas o existentes que pertenezcan al mismo inquilino del directorio. Las suscripciones que pertenecen a la suscripción del proveedor predeterminado también se pueden pasar a las suscripciones de proveedor delegado en el mismo inquilino de directorio. Para más información, consulte [Delegación de ofertas en Azure Stack](../azure-stack-delegated-provider.md).
 
<!-- 2536808 IS ASDK --> 
- **Mejor tiempo de creación de VM** para las máquinas virtuales que se crean con imágenes que se descargan de Azure Marketplace.

### <a name="fixed-issues"></a>Problemas corregidos
- <!-- IS ASDK--> Se ha corregido el problema de la creación de un conjunto de disponibilidad en el portal que provocaba que el conjunto tuviera un dominio de error y un dominio de actualización de 1.

<!-- TBD | ASDK, IS --> 
- Se han realizado varias mejoras en el proceso de actualización para que sea más confiable. Además, las correcciones se llevaron a cabo en la infraestructura subyacente, lo que mejora la purga de nodos y, por tanto, reduce al mínimo el posible tiempo de inactividad para las cargas de trabajo durante la actualización.

<!--2292271 | ASDK, IS --> 
- Se ha corregido un problema por el que un límite de cuota modificado no se aplicaba a las suscripciones existentes.  Ahora, si genera un límite de cuota para un recurso de red que forma parte de una oferta o plan asociado a una suscripción de usuario, el nuevo límite se aplica a las suscripciones previamente existentes además de a las nuevas.

<!-- 2448955 | IS ASDK --> 
- Ahora puede consultar correctamente los registros de actividad para los sistemas que se implementan en una zona horaria UTC+N.    

<!-- 2319627 |  ASDK, IS --> 
- La comprobación previa de los parámetros de configuración de copia de seguridad (ruta de acceso, nombre de usuario, contraseña y clave de cifrado) ya no establece una configuración incorrecta en la configuración de copia de seguridad. (Anteriormente, se establecía una configuración incorrecta en la copia de seguridad y esta produciría después un error al desencadenarse).

<!-- 2215948 |  ASDK, IS --> 
- La lista de copias de seguridad ahora se actualiza cuando elimina manualmente la copia de seguridad desde el recurso compartido externo.

<!-- 2360715 |  ASDK, IS -->  
- Al configurar la integración del centro de datos, ya no se tiene acceso al archivo de metadatos de AD FS desde un recurso compartido. Para más información, consulte [Configuración de la integración de AD FS mediante el archivo de metadatos de federación](../azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

<!-- 2388980 | ASDK, IS --> 
- Se ha corregido un problema que impedía a los usuarios asignar una dirección IP pública existente, que se había asignado previamente a una interfaz de red o a un equilibrador de carga, a una nueva interfaz de red o equilibrador de carga.  

<!-- 2551834 - IS, ASDK --> 
- Cuando se selecciona *Información general* para una cuenta de almacenamiento en los portales de administrador o de usuario, ahora el panel *Información esencial* muestra correctamente toda la información esperada. 

<!-- 2551834 - IS, ASDK --> 
- Cuando se selecciona *Etiquetas* para una cuenta de almacenamiento en los portales de administrador o de usuario, la información ahora se muestra correctamente.

<!-- TBD - IS ASDK --> 
- Esta versión de Azure Stack corrige el problema que impedía que la aplicación del controlador se actualizara desde los paquetes de extensiones de OEM.

<!-- 2055809- IS ASDK --> 
- Se ha corregido un problema que impedía la eliminación de las máquinas virtuales de la hoja de proceso cuando la máquina virtual no se podía crear.  

<!--  2643962 IS ASDK -->  
- La alerta por *baja capacidad de memoria* ya no aparece de forma incorrecta.

<!--  TBD ASDK --> 
- La máquina virtual que hospeda el punto de conexión con privilegios (PEP) ha aumentado a 4 GB. En el ASDK, esta máquina virtual se denomina AzS ERCS01.

- <!--  TBD – IS, ASDK --> Se han retirado los tamaños de máquina virtual *A básico* para la [creación de conjuntos de escalado de máquinas virtuales](../azure-stack-compute-add-scalesets.md) (VMSS) a través del portal. Para crear un VMSS con este tamaño, use PowerShell o una plantilla. 

### <a name="known-issues"></a>Problemas conocidos

#### <a name="portal"></a>Portal  
- <!-- 2967387 – IS, ASDK --> La cuenta que se usa para iniciar sesión en el portal de administrador o usuario de Azure Stack se muestra como **Usuario no identificado**. Esto se produce cuando en la cuenta no se ha especificado un *Nombre* o un *Apellido*. Para solucionar este problema, modifique la cuenta de usuario para proporcionar el nombre o el apellido. Después, tendrá que cerrar sesión y volver a iniciar sesión en el portal. 

-  <!--  2873083 - IS ASDK --> Cuando se usa el portal para crear un conjunto de escalado de máquinas virtuales (VMSS), la lista desplegable *Tamaño de instancia* no se carga correctamente cuando se usa Internet Explorer. Para solucionar este problema, use otro explorador cuando utilice el portal para crear un VMSS.  

#### <a name="portal"></a>Portal  
<!-- 2931230 – IS  ASDK --> 
- No se pueden eliminar los planes que se agregan a una suscripción de usuario como planes complementarios, aunque se quite el plan de la suscripción de usuario. El plan permanecerá hasta que también se eliminen las suscripciones a las que haga referencia el plan complementario. 

<!--2760466 – IS  ASDK --> 
- Cuando se instala un nuevo entorno de Azure Stack que ejecuta esta versión, la alerta que indica *Activación necesaria* podría no mostrarse. La [activación](../azure-stack-registration.md) se requiere para poder usar la redifusión de marketplace. 

<!-- TBD - IS ASDK --> 
- Los dos tipos de suscripción administrativa que se incluyeron con la versión 1804 no deberían usarse. Los tipos de suscripción son **suscripción de medición** y **suscripción de consumo**. Los tipos de suscripción son **suscripción de medición** y **suscripción de consumo**. Estos tipos de suscripción están visibles en los nuevos entornos de Azure Stack a partir de la versión 1804 pero aún no están listos para su uso. Tendrá que seguir usando el tipo de suscripción **Proveedor predeterminado**.

<!-- 2403291 - IS ASDK --> 
- No puede usar la barra de desplazamiento horizontal a lo largo de la parte inferior de los portales de administrador y de usuario. Si no puede acceder a la barra de desplazamiento horizontal, use las rutas de navegación para ir a una hoja anterior del portal seleccionando la hoja que desea ver en la lista de rutas que se encuentra en la parte superior izquierda del portal.
  ![Ruta de navegación](media/asdk-release-notes/breadcrumb.png)

<!-- TBD -  IS ASDK --> 
- La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

<!-- TBD -  IS ASDK --> 
- No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.

<!--  TBD | ASDK -->  
- La zona horaria predeterminada para la implementación de Azure Stack se establecerá ahora en UTC. Puede seleccionar una zona horaria al instalar Azure Stack, sin embargo, se revertirá automáticamente a la hora UTC como valor predeterminado durante la instalación.

#### <a name="health-and-monitoring"></a>Estado y supervisión
<!-- 1264761 - IS ASDK -->  
- Es posible que vea alertas del componente *Controlador de mantenimiento* con los siguientes detalles:  

   Alerta 1:
   - NOMBRE:  rol de infraestructura incorrecto
   - GRAVEDAD: Advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Heartbeat Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.  

  Alerta 2:
   - NOMBRE:  rol de infraestructura incorrecto
   - GRAVEDAD: Advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Fault Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.

  Se pueden omitir ambas alertas con seguridad y se cerrarán automáticamente con el tiempo.  

<!-- 2368581 - IS. ASDK --> 
- Operador de Azure Stack: si recibe una alerta de memoria insuficiente y no se pueden implementar las máquinas virtuales del inquilino debido a un *error de creación de máquina virtual de Fabric*, es posible que la marca de Azure Stack supere la memoria disponible. Use la [herramienta de planeamiento de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para comprender mejor la capacidad disponible para las cargas de trabajo.

<!-- TBD - IS. ASDK --> 
- Cuando se ejecuta el cmdlet **Test-AzureStack** en el punto de conexión con privilegios (PEP), la prueba de **rendimiento de la instancia de rol de la infraestructura de Azure Stack** generará un mensaje de advertencia para la máquina virtual de ERCS. Puede omitir sin problema el mensaje de advertencia y seguir usando el ASDK.

#### <a name="compute"></a>Proceso
<!-- 2494144 - IS, ASDK --> 
- Al seleccionar un tamaño de máquina virtual para una implementación, algunos tamaños de la serie F no aparecen en el selector de tamaño al crear una máquina virtual. Los siguientes tamaños de máquina virtual no aparecen en el selector: *F8s_v2*, *F16s_v2*, *F32s_v2* y *F64s_v2*.  
  Como alternativa, utilice uno de los métodos siguientes para implementar una máquina virtual. En cada método, debe especificar el tamaño de máquina virtual que desea utilizar.

- <!-- 3099544 – IS, ASDK --> Cuando cree una máquina virtual (VM) mediante el portal de Azure Stack y seleccione el tamaño de la máquina, se mostrará la columna USD/mes con el mensaje **No disponible**. Esta columna no debería aparecer, ya que en Azure Stack no se permite mostrar la columna de precios de la máquina virtual.

- <!-- 2869209 – IS, ASDK --> Cuando se usa el [cmdlet **Add-AzsPlatformImage**](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), se debe usar el parámetro **-OsUri** como el URI de la cuenta de almacenamiento donde se ha cargado el disco. Si usa la ruta de acceso local del disco, se produce el siguiente error en el cmdlet: *Long running operation failed with status ‘Failed’* (Se produjo un problema en la operación de larga ejecución con el estado "Error"). 

- <!--  2966665 – IS, ASDK -->Al conectar discos de datos SSD a máquinas virtuales de discos administrados de tamaño prémium (DS, DSv2, Fs y Fs_V2), se produce el siguiente error:  *Error al actualizar discos para la máquina virtual "vmName". Error: No se puede realizar la operación solicitada porque el tipo de cuenta de almacenamiento "Premium_LRS" no es compatible con el tamaño de VM "Standard_DS/Ds_V2/FS/Fs_v2)".*

   Para solucionar este problema, use discos de datos *Standard_LRS* en lugar de *discos Premium_LRS*. El uso de discos de datos *Standard_LRS* no cambia el costo de E/S por segundo ni de facturación.  

- <!--  2795678 – IS, ASDK --> Cuando usa el portal para crear máquinas virtuales (VM) en un tamaño de máquina virtual premium (DS, Ds_v2, FS, FSv2), la máquina virtual se crea en una cuenta de almacenamiento estándar. La creación en una cuenta de almacenamiento estándar no afecta al funcionamiento, E/S por segundo ni la facturación. 

   Puede omitir la siguiente advertencia sin problemas: *Ha elegido usar un disco estándar en un tamaño que admite discos prémium. Esto no se recomienda ya que podría afectar al rendimiento del sistema operativo. Considere la opción de usar Premium Storage (SSD).*

- <!-- 2967447 - IS, ASDK --> La experiencia de creación de conjuntos de escalado de máquinas virtuales (VMSS) proporciona 7.2 basada en CentOS como una opción para la implementación. Como esa imagen no está disponible en Azure Stack, seleccione otro sistema operativo para la implementación o use una plantilla de ARM en la que se especifique otra imagen de CentOS que el operador haya descargado de Marketplace antes de la implementación.

<!-- TBD -  IS ASDK --> 
- Los valores de escalado para conjuntos de escalas de máquina virtual no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.

<!-- TBD -  IS ASDK --> 
- Al crear máquinas virtuales en el portal de usuario de Azure Stack, este muestra un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie D. Todas las máquinas virtuales de la serie D pueden albergar tantos discos de datos como la configuración de Azure.

<!-- TBD -  IS ASDK --> 
- Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

<!-- TBD -  IS ASDK --> 
- Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

<!-- 1662991 - IS ASDK --> 
- No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico.

<!-- 2724961- IS ASDK --> 
- Al registrar el proveedor de recursos **Microsoft.Insight** en la configuración de la suscripción y crear una máquina virtual Windows con el diagnóstico del sistema operativo invitado habilitado, la página de información general de la máquina virtual no muestra los datos de métricas. 

 

#### <a name="networking"></a>Redes
<!-- 1766332 - IS, ASDK --> 
- En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Basada en directivas** como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

<!-- 1902460 -  IS ASDK --> 
- Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

<!-- 16309153 -  IS ASDK --> 
- En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.

<!-- 2702741 -  IS ASDK --> 
- No se garantiza que las direcciones IP públicas que se implementan mediante el método de asignación dinámica se conserven después de emitirse una detención o desasignación.

<!-- 2529607 - IS ASDK --> 
- Durante la *rotación secreta* de Azure Stack, hay un período en el que las direcciones IP públicas a nivel de instancia no son accesibles entre dos y cinco minutos.

<!-- 2664148 - IS ASDK --> 
- En los escenarios en los que el inquilino tenga acceso a sus máquinas virtuales mediante un túnel VPN S2S, podría encontrarse con que los intentos de conexión producen un error si la subred local se ha agregado a la puerta de enlace de red local una vez que la puerta de enlace ya está creada. 


#### <a name="sql-and-mysql"></a>SQL y MySQL
<!-- TBD - ASDK --> 
- La base de datos que hospeda los servidores debe estar dedicada al uso por el proveedor de recursos y las cargas de trabajo de usuario. No puede usar una instancia de SQL que esté siendo utilizada por otro consumidor, incluido App Services.

<!-- IS, ASDK --> 
- Los caracteres especiales, entre los que se incluyen los espacios y los puntos, no se admiten en el nombre de **familia** al crear una SKU para los proveedores de recursos de SQL y MySQL.

#### <a name="app-service"></a>App Service
<!-- 2352906 - IS ASDK --> 
- Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

<!-- TBD - IS ASDK --> 
- Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de proceso.  

<!-- TBD - IS ASDK --> 
- App Service solo puede implementarse en la *suscripción del proveedor predeterminado* en este momento.

#### <a name="usage"></a>Uso  
<!-- TBD -  IS ASDK --> 
- El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.

<!-- #### Identity -->

