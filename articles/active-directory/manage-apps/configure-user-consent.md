---
title: Configuración del consentimiento del usuario en una aplicación en Azure Active Directory | Microsoft Docs
description: Aprenda a administrar el modo en que los usuarios dan su consentimiento a los permisos de la aplicación. Puede simplificar la experiencia del usuario proporcionando consentimiento de administrador. Estos métodos se aplican a todos los usuarios finales del inquilino de Azure Active Directory (Azure AD).
services: active-directory
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: barbkess
ms.reviewer: arvindh
ms.openlocfilehash: 991199747f51f379ee6f3efe8009ab9166c33ac7
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832013"
---
# <a name="configure-the-way-end-users-consent-to-an-application-in-azure-active-directory"></a>Configure el modo en que los usuarios finales dan su consentimiento a una aplicación en Azure Active Directory.
Aprenda a configurar el modo en que los usuarios dan su consentimiento en los permisos de la aplicación. Puede simplificar la experiencia del usuario proporcionando consentimiento de administrador. En este artículo, se explican diferentes métodos para configurar el consentimiento del usuario. Estos métodos se aplican a todos los usuarios finales del inquilino de Azure Active Directory (Azure AD). 

Para más información acerca de cómo dar consentimiento a las aplicaciones, consulte [Marco de consentimiento de Azure Active Directory](../develop/consent-framework.md).

## <a name="prerequisites"></a>Requisitos previos

Para otorgar consentimiento de administrador, debe iniciar sesión como administrador global, administrador de una aplicación o administrador de una aplicación en la nube.

## <a name="grant-admin-consent-to-enterprise-apps-in-the-azure-portal"></a>Concesión de consentimiento en aplicaciones empresariales mediante Azure Portal

Para otorgar consentimiento a una aplicación empresarial:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global, administrador de una aplicación o administrador de una aplicación en la nube.
2. Haga clic en **Todos los servicios** en la parte superior del menú de navegación izquierdo. Se abrirá la **extensión de Azure Active Directory**
3. En el cuadro de búsqueda del filtro, escriba **"Azure Active Directory"** y seleccione el elemento **Azure Active Directory**.
4. En el menú de navegación, haga clic en **Aplicaciones empresariales**.
5. Haga clic en **Conceder consentimiento de administrador**. Se le pedirá que inicie sesión para administrar la aplicación.
6. Inicie sesión con una cuenta que tenga permisos para conceder consentimiento de administrador a la aplicación. 
7. Dé su consentimiento a los permisos de la aplicación.

Esta opción solo funciona si la aplicación: 

- está registrada en su inquilino; o bien,
- está registrada en otro inquilino de Azure AD y ha recibido el consentimiento, como mínimo, de un usuario final. Una vez que un usuario final ha dado su consentimiento a una aplicación, Azure AD muestra la aplicación bajo **Aplicaciones empresariales** en Azure Portal.

## <a name="grant-admin-consent-when-registering-an-app-in-the-azure-portal"></a>Concesión de consentimiento de administrador al registrar una aplicación en Azure Portal

Para conceder consentimiento de administrador al registrar una aplicación: 

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global.
2. Vaya a la hoja **Registros de aplicaciones**.
3. Seleccione la aplicación a la que quiere dar su consentimiento.
4. Seleccione **Permisos necesarios**.
5. Haga clic en **Conceder permisos** en la parte superior de la hoja.


## <a name="grant-admin-consent-through-a-url-request"></a>Concesión de consentimiento de administrador mediante una solicitud URL

Para otorgar consentimiento de administrador mediante una solicitud URL:

1. Cree una solicitud para *login.microsoftonline.com* con la configuración de la aplicación y anéxela a `&prompt=admin_consent`. 
2. Una vez que la sesión se haya iniciado con las credenciales del administrador, la aplicación tendrá consentimiento para todos los usuarios.


## <a name="force-user-consent-through-a-url-request"></a>Aplicación forzosa del consentimiento del administrador mediante una solicitud URL

Para hacer que los usuarios finales den su consentimiento en una aplicación cada vez que se autentiquen, anexe `&prompt=consent` a la dirección URL de la solicitud de autenticación.

## <a name="next-steps"></a>Pasos siguientes

[Consentimiento e integración de aplicaciones en Azure AD](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)

[Consentimiento y permisos para las aplicaciones convergentes v2.0 de Azure AD](../develop/active-directory-v2-scopes.md)

[Azure AD en StackOverflow](https://stackoverflow.com/questions/tagged/azure-active-directory)
