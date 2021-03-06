---
title: Selección de un contrato de página en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo seleccionar un contrato de página en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: e2e6da5df434ffd217b0521d4a13cd8ec713d5a1
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111048"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selección de un contrato de página en Azure Active Directory B2C con directivas personalizadas

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Puede seleccionar un contrato de página en Azure Active Directory (Azure AD) B2C mediante su configuración en una [directiva personalizada](active-directory-b2c-overview-custom.md). Un contrato de página es una asociación de elementos que proporciona Azure AD B2C y el contenido que proporciona el usuario. Si piensa usar [JavaScript](javascript-samples.md), deberá definir una versión del contrato de la página para todas las definiciones de contenido de la directiva personalizada.

## <a name="replace-datauri-values"></a>Reemplazo de los valores de DataUri

En las directivas personalizadas, puede que tenga elementos [ContentDefinitions](contentdefinitions.md) que definen las plantillas HTML que se usan en el recorrido del usuario. El elemento **ContentDefinition** contiene un elemento **DataUri** que hace referencia a los elementos de página proporcionados por Azure AD B2C. El elemento **LoadUri** es la ruta de acceso relativa al contenido HTML y CSS que proporcione.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Para seleccionar un contrato de página, cambie los valores de **DataUri** en los elementos [ContentDefinitions](contentdefinitions.md) de las directivas. Al cambiar los antiguos valores de **DataUri** por los nuevos valores, está seleccionando un paquete inmutable. La ventaja de usar este paquete es que sabe que no cambiará y provocará un comportamiento inesperado en la página. 

Para configurar un contrato de página, utilice la siguiente tabla para encontrar los valores de **DataUri**. 

| Valor antiguo de DataUri | Nuevo valor DataUri |
| ----------------- | ----------------- |
| urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0 | 
| urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0 | 
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0 |

## <a name="next-steps"></a>Pasos siguientes

Encuentre más información acerca de cómo personalizar la interfaz de usuario de las aplicaciones en [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).



