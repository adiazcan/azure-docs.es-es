---
title: 'Implementación de App Services: Azure Stack | Microsoft Docs'
description: Guía detallada para implementar App Service en Azure Stack
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: anwestg
ms.openlocfilehash: 504cd5a0bfbae1ff91f7914abe12aa35b699ee80
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104767"
---
# <a name="add-an-app-service-resource-provider-to-azure-stack"></a>Incorporación de un proveedor de recursos de App Service a Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Use las instrucciones de este artículo para implementar App Service en Azure Stack.

> [!IMPORTANT]  
> Aplique la actualización 1809 al sistema integrado de Azure Stack o implemente el kit de desarrollo de Azure Stack (ASDK) más reciente antes de implementar Azure App Service 1.4.

Puede ofrecer a los usuarios la capacidad de crear aplicaciones web y API. Para permitir que los usuarios creen estas aplicaciones, deberá:

 - Agregue el [proveedor de recursos de App Service](azure-stack-app-service-overview.md) a la implementación de Azure Stack mediante los pasos descritos en este artículo.
 - Después de instalar el proveedor de recursos de App Service, puede incluirlos en sus planes y ofertas. Los usuarios pueden suscribirse entonces para obtener el servicio y empezar a crear aplicaciones.

> [!IMPORTANT]  
> Antes de ejecutar el instalador del proveedor de recursos, asegúrese de que ha seguido las instrucciones descritas en [Antes de comenzar](azure-stack-app-service-before-you-get-started.md).

## <a name="run-the-app-service-resource-provider-installer"></a>Ejecución del instalador del proveedor de recursos de App Service

La instalación del proveedor de recursos de App Service tarda al menos una hora. El período de tiempo necesario depende del número de instancias de rol que vaya a implementar. Durante la implementación, el instalador ejecuta las siguientes tareas:

 - Creerá un contenedor de blobs en la cuenta de almacenamiento de Azure Stack especificada.
 - Creará una zona DNS y las entradas para App Service.
 - Registrará el proveedor de recursos de App Service.
 - Registrará los elementos de la galería de App Service.

Para implementar el proveedor de recursos de App Service, siga estos pasos:

1. Ejecute appservice.exe como administrador desde un equipo que pueda acceder al punto de conexión de administración de recursos de Azure del administrador de Azure Stack.

2. Seleccione **Implementar App Service o actualizar a la última versión**.

    ![Instalador de App Service][1]

3. Revise y acepte los términos de licencia del software de Microsoft y seleccione **Siguiente**.

4. Revise y acepte los términos de licencia de terceros y seleccione **Siguiente**.

5. Asegúrese de que la información de configuración de nube de App Service es correcta. Si usó la configuración predeterminada durante la implementación del Kit de desarrollo de Azure Stack (ASDK), puede aceptar los valores predeterminados. Pero, si personalizó las opciones cuando implementó el ASDK o va a realizar la implementación en un sistema integrado de Azure Stack, debe editar los valores de esta ventana para reflejar las diferencias.

   Por ejemplo, si se usa el sufijo de dominio mycloud.com, el punto de conexión de Azure Resource Manager del inquilino de Azure Stack debe cambiarse a management.&lt;región&gt;.mycloud.com. Revise estos valores y, a continuación, seleccione **Siguiente** para guardar la configuración.

   ![Instalador de App Service][2]

6. En la página siguiente del instalador de App Service, siga estos pasos:

     a. Seleccione **Conectar** situado junto al cuadro de **suscripciones de Azure Stack**.

     - Si se usa Azure Active Directory (Azure AD), escriba la cuenta de administrador y la contraseña de Azure AD que proporcionó al implementar Azure Stack. Seleccione **Iniciar sesión**.
     - Si se usan los Servicios de federación de Active Directory (AD FS), debe proporcionar la cuenta de administrador. Por ejemplo, cloudadmin@azurestack.local. Escriba la contraseña y seleccione **Iniciar sesión**.

   b. En el cuadro de **suscripciones de Azure Stack**, seleccione **Default Provider Subscription** (Suscripción de proveedor predeterminada).

     > [!IMPORTANT]  
     > App Service **debe** implementarse en la **suscripción de proveedor predeterminada**.

   c. En **Azure Stack Locations** (Ubicaciones de Azure Stack), seleccione la ubicación que corresponda a la región en la que se va a implementar. Por ejemplo, seleccione **local** si va a implementar con el Kit de desarrollo de Azure Stack.

    ![Instalador de App Service][3]

7. Ahora puede realizar la implementación en una red virtual existente que haya configurado [siguiendo estos pasos](azure-stack-app-service-before-you-get-started.md#virtual-network), o dejar que el instalador de App Service cree una nueva red virtual y subredes. Para crear una red virtual, siga estos pasos:

    a. Seleccione **Crear una red virtual con la configuración predeterminada**, acepte los valores predeterminados y haga clic en **Siguiente**.

   b. O, seleccione **Usar la red virtual y las subredes existentes**. Complete las siguientes acciones:

     - Seleccione el **grupo de recursos** que contiene la instancia de Virtual Network.
     - Elija el nombre de la instancia de **Virtual Network** en la que desea realizar la implementación.
     - Seleccione los valores correctos de **subred** para cada una de las subredes de rol necesarias.
     - Seleccione **Next** (Siguiente).

   ![Instalador de App Service][4]

8. Escriba la información para el recurso compartido de archivos y, a continuación, seleccione **Siguiente**. La dirección del recurso compartido de archivos debe usar el nombre de dominio completo (FQDN) o la dirección IP del servidor de archivos. Por ejemplo, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, o \\\10.0.0.1\websites.

   >[!NOTE]
   >El instalador intenta comprobar la conectividad con el recurso compartido de archivos antes de continuar. Sin embargo, si implementa en una red virtual existente, puede que se produzca un error en esta prueba de conectividad. Se le envía una advertencia y un aviso para continuar. Si la información del recurso compartido de archivos es correcta, continúe con la implementación.

   ![Instalador de App Service][7]

9. En la página siguiente del instalador de App Service, siga estos pasos:

    a. En el cuadro **Id. de la aplicación de identidad**, escriba el GUID de la aplicación que va a usar para la identidad (de Azure AD).

   b. En el cuadro **Identity Application certificate file** (Archivo del certificado de la aplicación de identidad) escriba (o vaya a) la ubicación del archivo del certificado.

   c. En el cuadro **Identity Application certificate password** (Contraseña del certificado de la aplicación de identidad), escriba la contraseña para el certificado. Esta contraseña es la que anotó cuando usó el script para crear los certificados.

   d. En el cuadro **Azure Resource Manager root certificate file** (Archivo del certificado raíz de Azure Resource Manager) escriba (vaya a) la ubicación del archivo del certificado.

   e. Seleccione **Next** (Siguiente).

   ![Instalador de App Service][9]

10. Para cada uno de los tres cuadros de archivo de certificado, seleccione **Examinar** y navegue hasta el archivo de certificado adecuado. Debe proporcionar la contraseña de cada certificado. Estos certificados son los que creó en el [paso para crear los certificados necesarios](azure-stack-app-service-before-you-get-started.md#get-certificates). Seleccione **Siguiente** después de escribir toda la información.

    | Box | Ejemplo de nombre de archivo de certificado |
    | --- | --- |
    | **Archivo de certificado SSL predeterminado de App Service**  | \_.appservice.local.AzureStack.external.pfx |
    | **Archivo de certificado SSL de API de App Service** | api.appservice.local.AzureStack.external.pfx |
    | **Archivo de certificado SSL del publicador de App Service** | ftp.appservice.local.AzureStack.external.pfx |

    Si usó un sufijo de dominio diferente cuando creó los certificados, los nombres de archivo de certificado no usan *local. AzureStack.external*. En su lugar, use la información del dominio personalizado.

    ![Instalador de App Service][10]

11. Escriba los detalles de SQL Server para la instancia de servidor que se usa para hospedar las bases de datos del proveedor de recursos de App Service y, después, seleccione **Siguiente**. El instalador valida las propiedades de conexión de SQL.

    > [!NOTE]
    > El instalador intenta comprobar la conectividad con la instancia de SQL Server antes de continuar. Sin embargo, si implementa en una red virtual existente, puede que se produzca un error en esta prueba de conectividad. Se le envía una advertencia y un aviso para continuar. Si la información de SQL Server es correcta, continúe con la implementación.
    >
    > Desde Azure App Service en Azure Stack 1.3 y versiones posteriores, el instalador comprobará que el servidor SQL Server tiene la contención de base de datos habilitada en el nivel de SQL Server.  Si no es así, se le indicará la siguiente excepción:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > Remítase a las [notas de la versión de Azure App Service en Azure Stack 1.3](azure-stack-app-service-release-notes-update-three.md) para más detalles.

    ![Instalador de App Service][11]

12. Revise las opciones de SKU y la instancia de rol. Los valores predeterminados se rellenan con el número mínimo de instancias y la SKU mínima de cada rol en una implementación de ASDK. Se proporciona un resumen de los requisitos de memoria y vCPU para ayudar a planear la implementación. Después de realizar las selecciones, elija **Siguiente**.

    >[!NOTE]
    >Para las implementaciones de producción, siga las instrucciones que encontrará en [Planeamiento de la capacidad de los roles de servidor de Azure App Service en Azure Stack](azure-stack-app-service-capacity-planning.md).

    | Rol | Número mínimo de instancias | SKU mínima | Notas |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A2: (2 vCPU, 3584 MB) | Administra y mantiene el estado de la nube de App Service. |
    | Administración | 1 | Standard_A2: (2 vCPU, 3584 MB) | Administra los puntos de conexión de API y Azure Resource Manager de App Service, las extensiones de portales (portal de Functions, administración e inquilino) y el servicio de datos. Para admitir la conmutación por error, se incrementan las instancias recomendadas a 2. |
    | Publicador | 1 | Standard_A1: (1 vCPU, 1792 MB) | Publica contenido a través de la implementación de web y FTP. |
    | FrontEnd | 1 | Standard_A1: (1 vCPU, 1792 MB) | Enruta las solicitudes a las aplicaciones de App Service. |
    | Trabajo compartido | 1 | Standard_A1: (1 vCPU, 1792 MB) | Hospeda aplicaciones de API o web y aplicaciones de Azure Functions. Puede ser conveniente agregar más instancias. Como operador, puede definir su oferta y elegir cualquier nivel de SKU. Los niveles deben tener una vCPU como mínimo. |

    ![Instalador de App Service][13]

    >[!NOTE]
    >**Windows Server 2016 Core no es una imagen de plataforma compatible para su uso con Azure App Service en Azure Stack.  No use imágenes de evaluación para las implementaciones de producción.**

13. En el cuadro **Seleccionar imagen de plataforma**, elija su imagen de máquina virtual Windows Server 2016 de implementación entre las que están disponibles en el proveedor de recursos de procesos para la nube de App Service. Seleccione **Next** (Siguiente).

14. En la página siguiente del instalador de App Service, siga estos pasos:

      a. Escriba el nombre de usuario y la contraseña del administrador de la máquina virtual con el rol de trabajo.

     b. Escriba el nombre de usuario y la contraseña del administrador de la máquina virtual con otros roles.

     c. Seleccione **Next** (Siguiente).

    ![Instalador de App Service][15]

15. En la página de resumen del instalador de App Service, siga estos pasos:

     a. Compruebe las selecciones realizadas. Para realizar cambios, use los botones **Anterior** para visitar las páginas anteriores.

    b. Si las configuraciones son correctas, active la casilla.

    c. Para iniciar la implementación, seleccione **Siguiente**.

    ![Instalador de App Service][16]

16. En la página siguiente del instalador de App Service, siga estos pasos:

     a. Realice un seguimiento del progreso de la instalación. App Service en Azure Stack tarda aproximadamente 60 minutos en realizar la implementación según las selecciones predeterminadas.

    b. Después de que el instalador finalice correctamente, seleccione **Salir**.

    ![Instalador de App Service][17]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Validación de la instalación de App Service en Azure Stack

1. En el Portal de administración de Azure Stack, vaya a **Administración - App Service**.

2. En la información general, debajo del estado, compruebe que en **Estado** se muestra el mensaje **Todos los roles están listos**.

    ![Administración de App Service](media/azure-stack-app-service-deploy/image12.png)

    Si va a realizar la implementación en una red virtual existente y a usar una dirección IP interna para conectarse al servidor de archivos, debe agregar una regla de seguridad de salida. Esta regla habilita el tráfico SMB entre la subred de trabajo y el servidor de archivos.  Para ello, vaya a WorkersNsg en el Portal de administración y agregue una regla de seguridad de salida con las siguientes propiedades:

    - Origen: Cualquiera
    - Intervalo de puertos de origen: *
    - Destino: Direcciones IP
    - Intervalo de direcciones IP de destino: Intervalo de direcciones IP del servidor de archivos
    - Intervalo de puertos de destino: 445
    - Protocolo: TCP
    - Acción: PERMITIR
    - Prioridad: 700
    - Nombre: Outbound_Allow_SMB445

## <a name="test-drive-app-service-on-azure-stack"></a>Prueba de App Service en Azure Stack

Después de implementar y registrar el proveedor de recursos de App Service, pruébelo para asegurarse de que los usuarios pueden implementar aplicaciones web y de API.

>[!NOTE]
>Debe crear una oferta que tenga el espacio de nombres Microsoft.Web en el plan. También necesita una suscripción de inquilino que se suscriba a la oferta. Para más información, consulte [Create offer](azure-stack-create-offer.md) (Creación de una oferta) y [Create plan](azure-stack-create-plan.md) (Creación de un plan).
>
>Se *debe* tener una suscripción de inquilino para crear aplicaciones que usen App Service en Azure Stack. Las únicas tareas que un administrador del servicio puede completar en el portal de administración están relacionadas con la administración del proveedor de recursos de App Service. Entre estas tareas se incluyen la adición de capacidad, la configuración de orígenes de implementación y la adición de niveles de trabajo y SKU.
>
>Para crear aplicaciones web, de API y Azure Functions, se debe usar el portal de inquilino y tener una suscripción de inquilino.
>

Para crear una aplicación web de prueba, siga estos pasos:

1. En el portal de usuario de Azure Stack, seleccione **+ Crear un recurso** > **Web y móvil** > **Aplicación web**.

2. En **Aplicación web**, escriba un nombre en **Aplicación web**.

3. En **Grupo de recursos**, seleccione **Nuevo**. Escriba un nombre para el **grupo de recursos**.

4. Seleccione **Plan de App Service/Ubicación** > **Crear nuevo**.

5. En **plan de App Service**, escriba un nombre para el **plan de App Service**.

6. Seleccione **Plan de tarifa** > **Free-Shared** (Gratis - Compartido) o **Shared-Shared** (Compartido - Compartido) > **Seleccionar** > **Aceptar** > **Crear**.

7. Aparecerá un icono para la nueva aplicación web en el panel. Seleccione el icono.

8. En la hoja **Aplicación web**, seleccione **Examinar** para ver el sitio web predeterminado para esta aplicación.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Implementación de un sitio web de WordPress, DNN o Django (opcional)

1. En el portal del inquilino de Azure Stack, seleccione **+**, vaya a Azure Marketplace, implemente un sitio web de Django y, a continuación, espere a que finalice la implementación. La plataforma web de Django usa una base de datos basada en el sistema de archivos. No requiere ningún proveedor de recursos adicional, como SQL o MySQL.

2. Si también implementó un proveedor de recursos de MySQL, puede implementar un sitio web de WordPress en Marketplace. Cuando se le pidan los parámetros de la base de datos, escriba el nombre de usuario como *User1@Server1*, con el nombre de usuario y el nombre del servidor de su elección.

3. Si también implementó un proveedor de recursos de SQL Server, puede implementar un sitio web de DNN en Marketplace. Cuando se le pidan los parámetros de la base de datos, elija una base de datos del equipo que ejecute la instancia de SQL Server que está conectado a su proveedor de recursos.

## <a name="next-steps"></a>Pasos siguientes

También puede probar otros [servicios de Plataforma como servicio (PaaS)](azure-stack-tools-paas-services.md).

 - [Proveedor de recursos de SQL Server](azure-stack-sql-resource-provider-deploy.md)
 - [Proveedor de recursos de MySQL](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: https://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: https://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: https://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy/app-service-installer.png
[2]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-arm-endpoints.png
[3]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-subscription-information.png
[4]: ./media/azure-stack-app-service-deploy/app-service-default-VNET-config.png
[5]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy/app-service-custom-VNET-config-with-values.png
[7]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration.png
[8]: ./media/azure-stack-app-service-deploy/app-service-fileshare-configuration-error.png
[9]: ./media/azure-stack-app-service-deploy/app-service-identity-app.png
[10]: ./media/azure-stack-app-service-deploy/app-service-certificates.png
[11]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration.png
[12]: ./media/azure-stack-app-service-deploy/app-service-sql-configuration-error.png
[13]: ./media/azure-stack-app-service-deploy/app-service-cloud-quantities.png
[14]: ./media/azure-stack-app-service-deploy/app-service-windows-image-selection.png
[15]: ./media/azure-stack-app-service-deploy/app-service-role-credentials.png
[16]: ./media/azure-stack-app-service-deploy/app-service-azure-stack-deployment-summary.png
[17]: ./media/azure-stack-app-service-deploy/app-service-deployment-progress.png
