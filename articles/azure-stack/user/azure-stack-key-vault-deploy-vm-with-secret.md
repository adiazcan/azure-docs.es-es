---
title: Implementación de una máquina virtual con una contraseña almacenada de forma segura en Azure Stack | Microsoft Docs
description: Aprenda a implementar una máquina virtual usando una contraseña almacenada en el almacén de claves de Azure Stack
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/14/2019
ms.author: mabrigg
ms.openlocfilehash: 8d853ec6b81340577598a66e048d69fe9b15a83c
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302636"
---
# <a name="create-a-virtual-machine-using-a-secure-password-stored-in-azure-stack-key-vault"></a>Creación de una máquina virtual mediante una contraseña segura almacenada en Key Vault de Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Los pasos de este artículo describen la implementación de una máquina virtual de Windows Server con una contraseña almacenada en Key Vault de Azure Stack. El uso de una contraseña de almacén de claves es más seguro que pasar una contraseña de texto sin formato.

## <a name="overview"></a>Información general

Puede almacenar valores, por ejemplo, una contraseña como un secreto, en un almacén de claves de Azure Stack. Después de crear un secreto, puede hacer referencia a él en las plantillas de Azure Resource Manager. El uso de secretos con Resource Manager proporciona las siguientes ventajas:

* No tiene que escribir manualmente el secreto cada vez que implemente un recurso.
* Puede especificar qué usuarios o entidades de servicio pueden acceder a un secreto.

## <a name="prerequisites"></a>Requisitos previos

* Debe suscribirse a una oferta que incluya el servicio Key Vault.
* [Instale PowerShell para Azure Stack.](azure-stack-powershell-install.md)
* [Configure el entorno de PowerShell.](azure-stack-powershell-configure-user.md)

Los pasos siguientes describen el proceso necesario para crear una máquina virtual mediante la recuperación de la contraseña almacenada en un almacén de claves:

1. Cree un secreto de almacén de claves.
2. Actualice el archivo azuredeploy.parameters.json.
3. Implemente la plantilla.

> [!NOTE]  
> Puede seguir estos pasos desde el Kit de desarrollo de Azure Stack o desde un cliente externo, si se conecta a través de VPN.

## <a name="create-a-key-vault-secret"></a>Creación de un secreto de almacén de claves

El script siguiente crea un almacén de claves y almacena en él una contraseña como un secreto. Use el parámetro `-EnabledForDeployment` al crear el almacén de claves. Este parámetro se asegura de que se puede hacer referencia al almacén de claves desde las plantillas de Azure Resource Manager.

```PowerShell

$vaultName = "contosovault"
$resourceGroup = "contosovaultrg"
$location = "local"
$secretName = "MySecret"

New-AzureRmResourceGroup `
  -Name $resourceGroup `
  -Location $location

New-AzureRmKeyVault `
  -VaultName $vaultName `
  -ResourceGroupName $resourceGroup `
  -Location $location
  -EnabledForTemplateDeployment

$secretValue = ConvertTo-SecureString -String '<Password for your virtual machine>' -AsPlainText -Force

Set-AzureKeyVaultSecret `
  -VaultName $vaultName `
  -Name $secretName `
  -SecretValue $secretValue

```

Cuando se ejecuta el script anterior, la salida incluye el identificador URI del secreto. Anote este URI. Tendrá que hacer referencia a él durante la [implementación de máquina virtual Windows con contraseña en la plantilla de almacén de claves](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv). Descargue la carpeta [101-vm-secure-password](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-windows-create-passwordfromkv) en el equipo de desarrollo. Esta carpeta contiene los archivos `azuredeploy.json` y `azuredeploy.parameters.json` que necesitará en los pasos siguientes.

Modificar el archivo `azuredeploy.parameters.json` según los valores del entorno. Los parámetros de especial interés son el nombre del almacén, el grupo de recursos del almacén y el identificador URI del secreto (que se generó en el script anterior). El archivo siguiente es un ejemplo de un archivo de parámetros:

## <a name="update-the-azuredeployparametersjson-file"></a>Actualice el archivo azuredeploy.parameters.json

Actualice el archivo azuredeploy.parameters.json con los valores de KeyVault URI, secretName y adminUsername de la máquina virtual correspondientes a su entorno. El siguiente archivo JSON muestra un ejemplo del archivo de parámetros de plantilla:

```json
{
    "$schema":  "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion":  "1.0.0.0",
    "parameters":  {
       "adminUsername":  {
         "value":  "demouser"
          },
         "adminPassword":  {
           "reference":  {
              "keyVault":  {
                "id":  "/subscriptions/xxxxxx/resourceGroups/RgKvPwd/providers/Microsoft.KeyVault/vaults/KvPwd"
                },
              "secretName":  "MySecret"
           }
         },
       "dnsLabelPrefix":  {
          "value":  "mydns123456"
        },
        "windowsOSVersion":  {
          "value":  "2016-Datacenter"
        }
    }
}

```

## <a name="template-deployment"></a>Implementación de plantilla

Ahora implemente la plantilla con el siguiente script de PowerShell:

```PowerShell  
New-AzureRmResourceGroupDeployment `
  -Name KVPwdDeployment `
  -ResourceGroupName $resourceGroup `
  -TemplateFile "<Fully qualified path to the azuredeploy.json file>" `
  -TemplateParameterFile "<Fully qualified path to the azuredeploy.parameters.json file>"
```

Cuando la plantilla se ha implementado correctamente, se producen en la siguiente salida:

![Salida de la implementación](media/azure-stack-key-vault-deploy-vm-with-secret/deployment-output.png)

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de una aplicación de ejemplo con Key Vault](azure-stack-key-vault-sample-app.md)
* [Implementación de una máquina virtual con un certificado de Key Vault](azure-stack-key-vault-push-secret-into-vm.md)
