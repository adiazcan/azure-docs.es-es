---
title: Cambio de secretos en Azure Stack | Microsoft Docs
description: Aprenda a cambiar los secretos en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: e4131bc8f038957e52b914937b2d45e670be8f5f
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157282"
---
# <a name="rotate-secrets-in-azure-stack"></a>Cambio de secretos en Azure Stack

*Estas instrucciones se aplican solo a sistemas integrados de Azure Stack (versión 1803 y posteriores). No intente el cambio de secretos en versiones de Azure Stack anteriores a la 1802*

Azure Stack usa varios secretos para proteger las comunicaciones entre los recursos y servicios de la infraestructura de Azure Stack.

- **Secretos internos**

Todos los certificados, contraseñas, cadenas seguras y claves que usa la infraestructura de Azure Stack sin la intervención del operador de este.

- **Secretos externos**

Certificados de servicio de infraestructura para servicios de uso externo que proporciona el operador de Azure Stack. Esto incluye los certificados para los siguientes servicios:

- Administrator Portal
- Public Portal
- Administrator Azure Resource Manager
- Global Azure Resource Manager
- Administrator KeyVault
- KeyVault
- Host de extensiones de administración
- ACS (incluido Blob Storage, Table Storage y Queue Storage)
- ADFS *
- Graph *

\* Solo es aplicable si el proveedor de identidades del entorno es los Servicios de federación de Active Directory (AD FS).

> [!Note]
> Todas las demás claves y cadenas seguras, incluyendo BMC, las contraseñas de cambio y las contraseñas de cuentas de usuario y administrador aún las actualiza manualmente el administrador.

> [!Important]
> A partir de la versión 1811 de Azure Stack, el cambio de secretos se ha separado para los certificados internos y externos.

Para mantener la integridad de la infraestructura de Azure Stack, los operadores necesitan la capacidad de poder cambiar periódicamente los secretos de la infraestructura con una frecuencia que sea coherente con los requisitos de seguridad de su organización.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Cambio de secretos con certificados externos desde una nueva entidad de certificación

Azure Stack admite el cambio de secretos con certificados externos desde una nueva entidad de certificación (CA) en los contextos siguientes:

|CA del certificado instalado|CA a la que cambiar|Compatible|Versiones compatibles de Azure Stack|
|-----|-----|-----|-----|
|De autofirmado|A empresarial|No compatible||
|De autofirmado|A autofirmado|No compatible||
|De autofirmado|A pública<sup>*</sup>|Compatible|1803 y posterior|
|De empresarial|A empresarial|Compatible siempre y cuando los clientes usen la MISMA entidad de certificación empresarial utilizada en la implementación|1803 y posterior|
|De empresarial|A autofirmado|No compatible||
|De empresarial|A pública<sup>*</sup>|Compatible|1803 y posterior|
|De pública<sup>*</sup>|A empresarial|No compatible|1803 y posterior|
|De pública<sup>*</sup>|A autofirmado|No compatible||
|De pública<sup>*</sup>|A pública<sup>*</sup>|Compatible|1803 y posterior|

<sup>*</sup>Indica que las entidades de certificación públicas son aquellas que forman parte del programa de raíz de confianza de Windows. Puede ver la lista completa en el artículo [Microsoft Trusted Root Certificate Program: Participants (as of June 27, 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca) [Programa de certificados raíz de confianza de Microsoft: participantes (a partir del 27 de junio de 2017)].

## <a name="alert-remediation"></a>Corrección de alertas

Cuando faltan menos de 30 días para la expiración de los secretos, se generan las siguientes alertas en el portal de administración:

- Expiración de contraseña de cuenta de servicio pendiente
- Expiración de certificado interno pendiente
- Expiración de certificado externo pendiente

Si ejecuta el cambio de secretos mediante las instrucciones que aparecen a continuación se corregirán estas alertas.

> [!Note]
> Los entornos de Azure Stack en versiones anteriores a la 1811 pueden ver alertas para las expiraciones de certificados o secretos internos pendientes.
> Estas alertas son inexactas y deben omitirse sin ejecutar el cambio de secretos interno.
> Las alertas de expiración de los secretos internos inexactas son un problema conocido que se resuelve en la versión 1811: los secretos internos no expiran a menos que el entorno haya estado activo durante dos años.

## <a name="pre-steps-for-secret-rotation"></a>Pasos previos para el cambio de secretos

   > [!IMPORTANT]
   > Si ya se ha realizado el cambio de secretos en su entorno de Azure Stack, debe actualizar el sistema a la versión 1811 o posterior antes de ejecutar nuevamente el cambio de secretos.
   > El cambio de secretos se debe ejecutar a través del [punto de conexión con privilegios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) y requiere credenciales de operador de Azure Stack.
   > Si los operadores de su entorno de Azure Stack no saben si se ha ejecutado el cambio de secretos en el entorno, actualice a la versión 1811 antes de ejecutar el nuevo cambio de secretos.

1. Se recomienda encarecidamente que actualice la instancia de Azure Stack a la versión 1811.

    > [!Note] 
    > Para las versiones anteriores a 1811 no es necesario cambiar los secretos para agregar certificados de host de extensiones. Debe seguir las instrucciones del artículo [Preparación de un host de extensiones de Azure Stack](azure-stack-extension-host-prepare.md) para agregar certificados de un host de extensiones.

2. Los operadores pueden notar que las alertas se abren y cierran automáticamente durante la rotación de los secretos de Azure Stack.  Este comportamiento es el esperado y puede hacerse caso omiso de las alertas.  Los operadores pueden comprobar la validez de estas alertas mediante la ejecución de **Test-AzureStack**.  Para los operadores que usan SCOM para supervisar los sistemas de Azure Stack, poner un sistema en modo de mantenimiento evitará que estas alertas lleguen a sus sistemas ITSM, pero continuarán alertando si no se puede acceder al sistema de Azure Stack.

3. Avise a los usuarios de cualquier operación de mantenimiento. Programe ventanas de mantenimiento normales, en la medida de lo posible, durante horas no laborables. Las operaciones de mantenimiento pueden afectar tanto a las cargas de trabajo del usuario como a las operaciones del portal.

    > [!Note]
    > Los pasos siguientes solo se aplican al cambiar secretos externos de Azure Stack.

4. Ejecute **[Test-AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** y compruebe que todos los resultados de prueba están en buen estado antes cambiar los secretos.
5. Prepare un nuevo conjunto de certificados externos de reemplazo. El nuevo conjunto coincide con las especificaciones de certificado que se describen en [Requisitos de certificados de infraestructura de clave pública de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs). Puede generar una solicitud de firma de certificados (CSR) para comprar o crear nuevos certificados mediante los pasos descritos en [Generación de certificados PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-get-pki-certs) y prepararlos para usarse en su entorno de Azure Stack mediante los pasos de [Preparación de certificados PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-prepare-pki-certs). No olvide validar los certificados que prepare con los pasos descritos en [Validación de certificados PKI](https://docs.microsoft.com/azure/azure-stack/azure-stack-validate-pki-certs).
6. Guarde una copia de seguridad de los certificados usados para el cambio en una ubicación segura. Si se ejecuta el cambio y, después, se produce un error, reemplace los certificados del recurso compartido de archivos por las copias de seguridad antes de volver a ejecutar el cambio. Conserve las copias de seguridad en la ubicación segura.
7. Cree un recurso compartido de archivos al que pueda acceder desde las máquinas virtuales de ERCS. El recurso compartido de archivos debe ser de lectura y escritura para la identidad **CloudAdmin**.
8. Abra una consola de PowerShell ISE desde un equipo que tenga acceso al recurso compartido de archivos. Vaya al recurso compartido de archivos.
9. Ejecute **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** para crear los directorios necesarios para los certificados externos.

> [!IMPORTANT]
> El script CertDirectoryMaker creará una estructura de carpetas a la que se adherirá:
>
> **.\Certificates\AAD** o ***.\Certificates\ADFS*** en función del proveedor de identidades que se utiliza para Azure Stack.
>
> Es de suma importancia que la estructura de carpetas termine con las carpetas **AAD** o **ADFS** y que todos los subdirectorios estén dentro de esta estructura; de lo contrario, **Start-SecretRotation** aparecerá:
> ```PowerShell
> Cannot bind argument to parameter 'Path' because it is null.
> + CategoryInfo          : InvalidData: (:) [Test-Certificate], ParameterBindingValidationException
> + FullyQualifiedErrorId : ParameterArgumentValidationErrorNullNotAllowed,Test-Certificate
> + PSComputerName        : xxx.xxx.xxx.xxx
> ```
>
> Como puede ver, el mensaje de error indicaría que hay un problema para acceder al recurso compartido de archivos, pero en realidad es la estructura de carpetas lo que se aplica aquí.
> Puede encontrar más información en Microsoft AzureStack Readiness Checker: [módulo PublicCertHelper](https://www.powershellgallery.com/packages/Microsoft.AzureStack.ReadinessChecker/1.1811.1101.1/Content/CertificateValidation%5CPublicCertHelper.psm1)
>
> También es importante que la estructura de la carpeta del recurso compartido de archivos comience con la carpeta **Certificates**; de lo contrario, también se producirá un error en la validación.
> El montaje del recurso compartido de archivos debe ser similar a **\\\\\<IPAddress>\\\<ShareName>\\** y debe contener la carpeta **Certificates\AAD** o **Certificates\ADFS** dentro.
>
> Por ejemplo: 
> - Fileshare = **\\\\\<IPAddress>\\\<ShareName>\\**
> - CertFolder = **Certificates\AAD**
> - FullPath = **\\\\\<IPAddress>\\\<ShareName>\Certificates\AAD**

## <a name="rotating-external-secrets"></a>Cambio de secretos externos

Para cambiar los secretos externos:

1. En el directorio **\Certificates\\\<IdentityProvider>** que se ha creado en los pasos previos, coloque el nuevo conjunto de certificados externos de reemplazo en la estructura de directorios según el formato descrito en la sección Certificados obligatorios de [Requisitos de certificados de infraestructura de clave pública de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).

    Ejemplo de la estructura de carpetas para el proveedor de identidades de AAD:
    ```PowerShell
        <ShareName>
        │   │
        │   ├───Certificates
        │   └───AAD
        │       ├───ACSBlob
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSQueue
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSTable
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Extension Host
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Portal
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Admin
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Public
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVault
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVaultInternal
        │       │       <CertName>.pfx
        │       │
        │       ├───Public Extension Host
        │       │       <CertName>.pfx
        │       │
        │       └───Public Portal
        │               <CertName>.pfx

    ```

2. Cree una sesión de PowerShell con el [punto de conexión con privilegios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) mediante la cuenta **CloudAdmin** y almacene las sesiones como una variable. Utilizará esta variable como parámetro en el paso siguiente.

    > [!IMPORTANT]  
    > No escriba la sesión, almacénela como una variable.

3. Ejecute **[Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/Invoke-Command?view=powershell-5.1)**. Pase la variable de sesión de PowerShell del punto de conexión con privilegios como parámetro **Session**.

4. Ejecute **Start-SecretRotation** con los siguientes parámetros:
    - **PfxFilesPath**  
    Especifique la ruta de acceso de red al directorio de certificados que creó anteriormente.  
    - **PathAccessCredential**  
    Un objeto PSCredential con las credenciales del recurso compartido.
    - **CertificatePassword**  
    Una cadena segura de la contraseña usada para todos los archivos de certificados pfx creados.

5. Espere mientras cambian los secretos. El cambio de secretos externo normalmente tarda aproximadamente una hora.

    Cuando se complete correctamente el cambio de secretos, la consola mostrará el mensaje **Overall action status: Success** (Estado global de la acción: correcto).

    > [!Note]
    > Si se produce un error en el cambio de secretos, siga las instrucciones del mensaje de error y vuelva a ejecutar **Start-SecretRotation** con el parámetro **-ReRun**.

    ```PowerShell
    Start-SecretRotation -ReRun
    ```
    Póngase en contacto con soporte técnico si la rotación de secretos falla en reiteradas ocasiones.

6. Después de la finalización correcta del cambio de secretos, quite los certificados del recurso compartido creado en el paso anterior y almacénelos en la ubicación segura de copia de seguridad.

## <a name="walkthrough-of-secret-rotation"></a>Guía detallada sobre cambio de secretos

El siguiente ejemplo de PowerShell muestra los cmdlets y parámetros que debe ejecutar para cambiar los secretos.

```PowerShell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Run Secret Rotation
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>Cambio solo de secretos internos

> [!Note]
> El cambio de secretos internos solo debe realizarse si sospecha que un secreto interno se ha visto comprometido por una entidad malintencionada o si ha recibido una alerta (en la compilación 1811 o posterior) que indica que los certificados internos van a expirar.
> Los entornos de Azure Stack en versiones anteriores a la 1811 pueden ver alertas para las expiraciones de certificados o secretos internos pendientes.
> Estas alertas son inexactas y deben omitirse sin ejecutar el cambio de secretos interno.
> Las alertas de expiración de los secretos internos inexactas son un problema conocido que se resuelve en la versión 1811: los secretos internos no expiran a menos que el entorno haya estado activo durante dos años.

1. Cree una sesión de PowerShell con el [punto de conexión con privilegios](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. En la sesión del punto de conexión con privilegios, ejecute **Start-SecretRotation** sin argumentos.

    > [!Note]
    > Los entornos de Azure Stack en versiones anteriores a 1811 no requerirán la marca **-Internal**. **Start-SecretRotation** cambiará solo los secretos internos.

3. Espere mientras cambian los secretos.

Cuando se complete correctamente el cambio de secretos, la consola mostrará el mensaje **Overall action status: Success** (Estado global de la acción: correcto).
    > [!Note]
    > If secret rotation fails, follow the instructions in the error message and rerun **Start-SecretRotation** with the  **–Internal** and **-ReRun** parameters.  

```PowerShell
Start-SecretRotation -Internal -ReRun
```

Póngase en contacto con soporte técnico si la rotación de secretos falla en reiteradas ocasiones.

## <a name="start-secretrotation-reference"></a>Referencia de Start-SecretRotation

Permite cambiar los secretos de un sistema de Azure Stack. Solo se ejecuta en el punto de conexión con privilegios de Azure Stack.

### <a name="syntax"></a>Sintaxis

#### <a name="for-external-secret-rotation"></a>Para el cambio de secretos externos

```PowerShell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential <PSCredential>] [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>Para el cambio de secretos internos

```PowerShell
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>Para volver a ejecutar el cambio de secretos externos

```PowerShell
Start-SecretRotation [-ReRun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>Para volver a ejecutar el cambio de secretos internos

```PowerShell
Start-SecretRotation [-ReRun] [-Internal]
```

### <a name="description"></a>DESCRIPCIÓN

El cmdlet **Start-SecretRotation** permite cambiar los secretos de la infraestructura de un sistema de Azure Stack. De forma predeterminada, se cambian solo los certificados de todos los puntos de conexión de la infraestructura de red externa. Si se usa con la marca -Internal, se cambian los secretos de infraestructura interna. Al cambiar los puntos de conexión de la infraestructura de red externa, **Start-SecretRotation** se debe ejecutar con un bloque de script **Invoke-Command** con la sesión del punto de conexión con privilegios del entorno de Azure Stack pasada como el parámetro **Session**.

### <a name="parameters"></a>Parámetros

| Parámetro | Escriba | Obligatorio | Posición | Valor predeterminado | DESCRIPCIÓN |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | string  | False  | con nombre  | None  | La ruta de acceso del recurso compartido de archivos al directorio **\Certificates** que contiene todos los certificados del punto de conexión de la red externa. Solo se necesita al rotar secretos externos. El directorio final debe ser **\Certificates**. |
| CertificatePassword | SecureString | False  | con nombre  | None  | La contraseña de todos los certificados que se proporcionan en -PfXFilesPath. Valor obligatorio si se proporciona PfxFilesPath al cambiar secretos externos. |
| Interno | string | False | con nombre | None | La marca Internal se debe utilizar cada vez que un operador de Azure Stack quiera cambiar los secretos de infraestructura interna. |
| PathAccessCredential | PSCredential | False  | con nombre  | None  | La credencial de PowerShell para el recurso compartido de archivos al directorio **\Certificates** que contiene todos los certificados del punto de conexión de la red externa. Solo se necesita al rotar secretos externos.  |
| ReRun | SwitchParameter | False  | con nombre  | None  | ReRun se debe usar en cualquier momento en que se vuelva a intentar el cambio de secretos después de un intento fallido. |

### <a name="examples"></a>Ejemplos

#### <a name="rotate-only-internal-infrastructure-secrets"></a>Cambio de solo los secretos de infraestructura interna

Esto se debe ejecutar a través del [punto de conexión con privilegios del entorno](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) de Azure Stack.

```PowerShell
PS C:\> Start-SecretRotation -Internal
```

Este comando permite cambiar todos los secretos de la infraestructura expuesta a la red interna de Azure Stack.

#### <a name="rotate-only-external-infrastructure-secrets"></a>Cambio de solo los secretos de infraestructura externa  

```PowerShell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {  
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Este comando cambia los certificados TLS que se usan para los puntos de conexión de infraestructura de red externa de Azure Stack.

#### <a name="rotate-internal-and-external-infrastructure-secrets-pre-1811-only"></a>Cambio de secretos de la infraestructura interna y externa (solo **anterior a 1811**)

> [!IMPORTANT]
> Este comando solo se aplica a Azure Stack **anterior a 1811**, ya que el cambio se ha dividido para los certificados internos y externos.
>
> **Desde las versiones posteriores a *1811* ya no se pueden cambiar los certificados internos y externos.**

```PowerShell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Este comando cambia todos los secretos de la infraestructura expuestos a la red interna de Azure Stack, así como los certificados TLS que se usan para los puntos de conexión de la infraestructura de red externa de Azure Stack. Start-SecretRotation cambia todos los secretos generados por Stack, y dado que se han proporcionado certificados, los certificados de punto de conexión externos también se cambiarán.  

## <a name="update-the-baseboard-management-controller-bmc-credential"></a>Actualización de la credencial del controlador de administración de placa base (BMC)

El controlador de administración de placa base (BMC) supervisa el estado físico de sus servidores. Las especificaciones e instrucciones para actualizar el nombre de la cuenta de usuario y contraseña de BMC varían según el proveedor de hardware del fabricante de equipos originales (OEM). Debe actualizar regularmente las contraseñas de los componentes de Azure Stack.

1. Actualice el BMC en los servidores físicos de Azure Stack con las instrucciones del OEM. El nombre de la cuenta de usuario y la contraseña para cada BMC de su entorno deben ser los mismos.
2. Abra un punto de conexión con privilegios en sesiones de Azure Stack. Para obtener instrucciones, consulte [Uso del punto de conexión con privilegios en Azure Stack](azure-stack-privileged-endpoint.md).
3. Cuando el símbolo del sistema de PowerShell cambie a **[IP address or ERCS VM name]: PS >** o a **[azs-ercs01]: PS >**, en función del entorno, ejecute `Set-BmcCredential` mediante `Invoke-Command`. Pase la variable de sesión del punto de conexión con privilegios como parámetro. Por ejemplo: 

    ```PowerShell
    # Interactive Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPCreds = Get-Credential "<Domain>\CloudAdmin" -Message "PEP Credentials"
    $NewBmcPwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBmcUser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

    También puede usar la versión de PowerShell estática con las contraseñas como líneas de código:

    ```PowerShell
    # Static Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEPPwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PEPCreds = New-Object System.Management.Automation.PSCredential ($PEPUser, $PEPPwd)
    $NewBmcPwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force
    $NewBmcUser = "<New BMC User name>"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Pasos siguientes

[Más información acerca de la seguridad de Azure Stack](azure-stack-security-foundations.md)
