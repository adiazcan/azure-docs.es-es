---
title: Requisitos de certificados de infraestructura de clave pública de Azure Stack para sistemas integrados de Azure Stack | Microsoft Docs
description: Describe los requisitos de implementación de certificados PKI de Azure Stack para sus sistemas integrados.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2019
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 6cf32ba50e83b95d51493244ef8e8646433b0b02
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54024950"
---
# <a name="azure-stack-public-key-infrastructure-certificate-requirements"></a>Requisitos de certificados de infraestructura de clave pública de Azure Stack

Azure Stack tiene una red de infraestructura pública que usa direcciones IP públicas accesibles externamente y asignadas a un pequeño conjunto de servicios de Azure Stack y, posiblemente, a las máquinas virtuales del inquilino. Se requieren certificados PKI con los nombres DNS apropiados para estos puntos de conexión de la infraestructura pública de Azure Stack durante la implementación de este. En este artículo se proporciona información acerca de lo siguiente:

- Los certificados son necesarios para implementar Azure Stack
- El proceso de obtención de certificados que concuerda con esas especificaciones
- Cómo preparar, validar y utilizar esos certificados durante la implementación

> [!Note]  
> Durante la implementación, los certificados se deben copiar en la carpeta de implementación que coincida con el proveedor de identidades con el que va a realizar esta operación (Azure AD o AD FS). Si usa un único certificado para todos los puntos de conexión, debe copiar ese archivo de certificado en cada carpeta de implementación, tal y como se describe en las tablas siguientes. La estructura de carpetas anterior se compila en la máquina virtual de implementación y puede encontrarse en C:\CloudDeployment\Setup\Certificates. 

## <a name="certificate-requirements"></a>Requisitos de certificados
En la lista siguiente se describen los requisitos de certificados que son necesarios para implementar Azure Stack: 
- Los certificados deben ser emitidos desde una entidad de certificación interna o pública. Si se usa una entidad de certificación pública, se debe incluir en la imagen del sistema operativo base como parte del programa de entidades de certificación raíz de confianza de Microsoft (Microsoft Trusted Root Certificate Program). Podrá encontrar la lista completa aquí: https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca 
- La infraestructura de Azure Stack debe tener acceso de red a la ubicación de la lista de revocación de certificados (CRL) de la entidad de certificación publicada en el certificado. Esta lista de revocación de certificados debe ser un punto de conexión http
- Al cambiar los certificados, deben estar emitidos por la misma entidad certificación interna utilizada para firmar los certificados proporcionados en la implementación o por cualquier entidad de certificación pública anterior.
- No se admite el uso de certificados autofirmados.
- Para la implementación y la rotación, se puede usar un certificado único que abarque todos los espacios de nombres en los campos Nombre del firmante y Nombre alternativo del firmante (SAN) del certificado, O BIEN se pueden usar certificados individuales para cada uno de los espacios de nombres siguientes requeridos por los servicios de Azure Stack que se van a usar. Para ambos enfoques hay que usar caracteres comodín para los puntos de conexión donde sean necesarios, como **KeyVault** y **KeyVaultInternal**. 
- El cifrado PFX del certificado debe ser 3DES. 
- El algoritmo de firma de certificado no debe ser SHA1. 
- El formato del certificado debe ser PFX, porque las claves públicas y privadas son necesarias para la instalación de Azure Stack. La clave privada debe tener establecido el atributo de clave de la máquina local.
- El cifrado de PFX debe ser 3DES (este es el valor predeterminado cuando se exporta desde un cliente de Windows 10 o desde un almacén de certificados de Windows Server 2016).
- Los archivos PFX de certificado deben tener un valor "Digital Signature" (firma digital) y "KeyEncipherment" (cifrado de clave) en el campo "Key Usage" (uso de clave).
- Los archivos PFX de certificado deben tener los valores "Server Authentication (1.3.6.1.5.5.7.3.1)" (Autenticación de servidor (1.3.6.1.5.5.7.3.1)) y "Client Authentication (1.3.6.1.5.5.7.3.2)" (Autenticación de cliente (1.3.6.1.5.5.7.3.2)) en el campo "Enhanced Key Usage" (Uso mejorado de clave).
- El campo "Issued to:" (Emitido para:) del certificado no debe ser el mismo que su campo "Issued by:" (Emitido por:).
- Las contraseñas para todos los archivos PFX de certificado deben ser las mismas en el momento de la implementación.
- La contraseña para el archivo pfx de certificado tiene que ser una contraseña compleja.
- Asegúrese de que los nombres de los firmantes y los nombres alternativos de los firmantes de la extensión de nombre alternativo del firmante (x509v3_config) coinciden. El campo del nombre alternativo del firmante permite especificar nombres de host adicionales (sitios web, direcciones IP y nombres comunes) que estén protegidos por un certificado SSL individual.

> [!NOTE]  
> No se admiten certificados autofirmados.

> [!NOTE]  
> Se admite la presencia de entidades emisoras de certificados intermediarias en una cadena de relaciones de confianza del certificado. 

## <a name="mandatory-certificates"></a>Certificados obligatorios
En la tabla de esta sección se describen los certificados PKI públicos de punto de conexión de Azure Stack que son necesarios tanto para las implementaciones de Azure AD como para las de Azure Stack en AD FS. Los requisitos de certificados se agrupan por área, así como los espacios de nombres utilizados y los certificados que son necesarios para cada espacio de nombres. En la tabla también se describe la carpeta en la que el proveedor de soluciones copia los certificados diferentes para cada punto de conexión público. 

Se requieren certificados con los nombres DNS apropiados para cada punto de conexión de la infraestructura pública de Azure Stack. El nombre DNS de cada punto de conexión se expresa en el formato: *&lt;prefijo>.&lt;región>.&lt;fqdn>*. 

En la implementación, los valores de [region] y [externalfqdn] deben coincidir con los nombres de dominio externo y región que eligió para el sistema de Azure Stack. Por ejemplo, si el nombre de la región es *Redmond* y el nombre de dominio externo fuese *contoso.com*, los nombres DNS tendrían el formato *&lt;prefijo>.redmond.contoso.com*. Los valores de *&lt;prefijo>* son designados de antemano por Microsoft para describir el punto de conexión protegido por el certificado. Además, los valores de  *&lt;prefijo>* de los puntos de conexión de la infraestructura externa dependen del servicio de Azure Stack que use el punto de conexión concreto. 

> [!note]  
> Para los entornos de producción, se recomienda que se generen certificados individuales para cada punto de conexión y que se copien en el directorio correspondiente. Para los entornos de desarrollo, los certificados se pueden proporcionar como un certificado comodín único que abarque todos los espacios de nombres en los campos de Sujeto y Nombre alternativo del sujeto (SAN) y que se copie en todos los directorios. Un certificado único que abarca todos los servicios y puntos de conexión es una postura insegura y, por tanto, solo para desarrollo. Recuerde que, en ambos casos, debe utilizar certificados comodín para puntos de conexión como **acs** y Key Vault donde sean necesarios. 

| Carpeta de implementación | Nombres alternativos del firmante (SAN) y firmante del certificado requeridos | Ámbito (por región) | Nombre del subdominio |
|-------------------------------|------------------------------------------------------------------|----------------------------------|-----------------------------|
| Public Portal | portal.&lt;region>.&lt;fqdn> | Portales | &lt;region>.&lt;fqdn> |
| Admin Portal | adminportal.&lt;region>.&lt;fqdn> | Portales | &lt;region>.&lt;fqdn> |
| Azure Resource Manager Public | management.&lt;region>.&lt;fqdn> | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| Azure Resource Manager Admin | adminmanagement.&lt;region>.&lt;fqdn> | Azure Resource Manager | &lt;region>.&lt;fqdn> |
| ACSBlob | *.blob.&lt;region>.&lt;fqdn><br>(Certificado SSL comodín) | Blob Storage | blob.&lt;region>.&lt;fqdn> |
| ACSTable | *.table.&lt;region>.&lt;fqdn><br>(Certificado SSL comodín) | Table Storage | table.&lt;region>.&lt;fqdn> |
| ACSQueue | *.queue.&lt;region>.&lt;fqdn><br>(Certificado SSL comodín) | Queue Storage | queue.&lt;region>.&lt;fqdn> |
| KeyVault | *.vault.&lt;region>.&lt;fqdn><br>(Certificado SSL comodín) | Key Vault | vault.&lt;region>.&lt;fqdn> |
| KeyVaultInternal | *.adminvault.&lt;region>.&lt;fqdn><br>(Certificado SSL comodín) |  Almacén de claves interno |  adminvault.&lt;region>.&lt;fqdn> |
| Host de extensiones de administración | *.adminhosting.\<region>.\<fqdn> (certificados SSL comodín) | Host de extensiones de administración | adminhosting.\<region>.\<fqdn> |
| Host de extensiones públicas | *.hosting.\<region>.\<fqdn> (certificados SSL comodín) | Host de extensiones públicas | hosting.\<region>.\<fqdn> |

Si implementa Azure Stack con el modo de implementación de Azure AD, solo tiene que solicitar los certificados que se muestran en la tabla anterior. Sin embargo, si implementa Azure Stack utilizando el modo de implementación de AD FS, también debe solicitar los certificados descritos en la tabla siguiente:

|Carpeta de implementación|Nombres alternativos del firmante (SAN) y firmante del certificado requeridos|Ámbito (por región)|Nombre del subdominio|
|-----|-----|-----|-----|
|ADFS|adfs.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL)|ADFS|*&lt;región>.&lt;fqdn>*|
|Grafo|graph.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL)|Grafo|*&lt;región>.&lt;fqdn>*|
|

> [!IMPORTANT]
> Todos los certificados que se indican en esta sección deben tener la misma contraseña. 

## <a name="optional-paas-certificates"></a>Certificados de PaaS opcionales
Si va a implementar los servicios adicionales PaaS de Azure Stack (SQL, MySQL y App Service) después de implementar y configurar Azure Stack, debe solicitar certificados adicionales para cubrir los puntos de conexión de los servicios PaaS. 

> [!IMPORTANT]
> Los certificados que se usan para los proveedores de recursos de App Service, SQL y MySQL deben tener la misma entidad de certificación raíz que los de los puntos de conexión globales de Azure Stack. 

En la tabla siguiente se describen los puntos de conexión y los certificados necesarios para los adaptadores de SQL y MySQL y para App Service. No es necesario copiar estos certificados en la carpeta de implementación de Azure Stack. En su lugar, estos certificados se proporcionan al instalar los proveedores de recursos adicionales. 

|Ámbito (por región)|Certificate|Nombres alternativos del firmante (SAN) y firmante del certificado requeridos|Nombre del subdominio|
|-----|-----|-----|-----|
|SQL, MySQL|SQL y MySQL|&#42;.dbadapter.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL comodín)|dbadapter.*&lt;región>.&lt;fqdn>*|
|App Service|Certificado SSL predeterminado de tráfico web|&#42;.appservice.*&lt;región>.&lt;fqdn>*<br>&#42;.scm.appservice.*&lt;región>.&lt;fqdn>*<br>&#42;.sso.appservice.*&lt;region>.&lt;fqdn>*<br>(Certificado SSL comodín de varios dominios<sup>1</sup>)|appservice.*&lt;región>.&lt;fqdn>*<br>scm.appservice.*&lt;región>.&lt;fqdn>*|
|App Service|API|api.appservice.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;región>.&lt;fqdn>*<br>scm.appservice.*&lt;región>.&lt;fqdn>*|
|App Service|FTP|ftp.appservice.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;región>.&lt;fqdn>*<br>scm.appservice.*&lt;región>.&lt;fqdn>*|
|App Service|SSO|sso.appservice.*&lt;región>.&lt;fqdn>*<br>(Certificado SSL<sup>2</sup>)|appservice.*&lt;región>.&lt;fqdn>*<br>scm.appservice.*&lt;región>.&lt;fqdn>*|

<sup>1</sup> Requiere un certificado con varios nombres alternativos de firmante comodín. Puede que no todas las entidades de certificación públicas admitan varios nombres alternativos del firmante comodín en un solo certificado. 

<sup>2</sup> A &#42;.appservice.*&lt;región>.&lt;fqdn>* el certificado comodín no se puede usar en lugar de estos tres certificados (api.appservice.*&lt;región>.&lt;fqdn>*, ftp.appservice.*&lt;región>.&lt;fqdn>* y sso.appservice.*&lt;región>.&lt;fqdn>*. Appservice requiere explícitamente el uso de certificados independientes para estos puntos de conexión. 

## <a name="learn-more"></a>Más información
Aprenda a [generar certificados PKI para la implementación de Azure Stack](azure-stack-get-pki-certs.md). 

## <a name="next-steps"></a>Pasos siguientes
[Integración de identidades](azure-stack-integrate-identity.md)

