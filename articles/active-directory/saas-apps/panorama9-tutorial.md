---
title: 'Tutorial: Integración de Azure Active Directory con Panorama9 | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Panorama9.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5e28d7fa-03be-49f3-96c8-b567f1257d44
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: ad2a9dba54df3ae7a3e2604437c8a65c88dcca94
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850897"
---
# <a name="tutorial-azure-active-directory-integration-with-panorama9"></a>Tutorial: Integración de Azure Active Directory con Panorama9

En este tutorial, aprenderá a integrar Panorama9 con Azure Active Directory (Azure AD).

La integración de Panorama9 con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Panorama9
- Puede permitir que los usuarios inicien sesión automáticamente en Panorama9 (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Panorama9, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Panorama9

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregación de Panorama9 desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-panorama9-from-the-gallery"></a>Agregación de Panorama9 desde la galería
Para configurar la integración de Panorama9 en Azure AD, será preciso que agregue Panorama9 desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Panorama9 desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Panorama9**.

    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/tutorial_panorama9_search.png)

1. En el panel de resultados, seleccione **Panorama9** y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/tutorial_panorama9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Panorama9 con un usuario de prueba llamado "Britta Simon."

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Panorama9 para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Panorama9.

Para establecer la relación de vínculo, en Panorama9, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Panorama9, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Panorama9](#creating-a-panorama9-test-user)**: el objetivo es tener un homólogo de Britta Simon en Panorama9 que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Panorama9.

**Para configurar el inicio de sesión único de Azure AD con Panorama9, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Panorama9**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_panorama9_samlbase.png)

1. En la sección **Dominio y direcciones URL de Panorama9**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_panorama9_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una URL como: `https://dashboard.panorama9.com/saml/access/3262`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.panorama9.com/saml20/<tenant-name>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de Panorama9](https://support.panorama9.com) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, copie el valor de **HUELLA DIGITAL** del certificado.

    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_panorama9_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Panorama9**, haga clic en **Configurar Panorama9** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_panorama9_configure.png) 

1. En otra ventana del explorador web, inicie sesión en el sitio de la compañía Panorama9 como administrador.

1. En la barra de herramientas de la parte superior, haga clic en **Administrar** y luego en **Extensiones**.
   
   ![Extensiones](./media/panorama9-tutorial/ic790023.png "Extensiones")
1. En el diálogo **Extensiones**, haga clic en **Inicio de sesión único**.
   
   ![Inicio de sesión único](./media/panorama9-tutorial/ic790024.png "Inicio de sesión único")
1. En la sección **Configuración** , lleve a cabo estos pasos:
   
   ![Configuración](./media/panorama9-tutorial/ic790025.png "Configuración")
   
     a. En el cuadro de texto **Identity Provider URL** (Dirección URL del proveedor de identidades), pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que ha copiado en Azure Portal.
   
    b. En el cuadro de texto **Certificate Fingerprint** (Huella digital de certificado), pegue el valor de **Huella digital** del certificado que haya copiado de Azure Portal.    
         
1. Haga clic en **Save**(Guardar).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/panorama9-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-panorama9-test-user"></a>Creación de un usuario de prueba de Panorama9

Para permitir que los usuarios de Azure AD inicien sesión en Panorama9, deben aprovisionarse en Panorama9.  

En el caso de Panorama9, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de la compañía **Panorama9** como administrador.

1. En el menú en la parte superior, haga clic en **Administrar** y luego haga clic en **Usuarios**.
   
  ![Usuarios](./media/panorama9-tutorial/ic790027.png "Usuarios")

1. En la sección Usuarios, haga clic en **+** para agregar un nuevo usuario.

 ![Usuarios](./media/panorama9-tutorial/ic790028.png "Usuarios")

1. Vaya a la sección Datos de usuario y escriba la dirección de correo electrónico de un usuario válido de Azure Active Directory que desee aprovisionar en el cuadro de texto **Correo electrónico**.

1. En la sección Usuarios, haga clic en **Guardar**.
   
> [!NOTE]
    > El titular de la cuenta de Azure Active Directory recibirá un mensaje de correo y seguirá un vínculo para confirmar su cuenta antes de que se active.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Panorama9.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Panorama9, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Panorama9**.

    ![Configurar inicio de sesión único](./media/panorama9-tutorial/tutorial_panorama9_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Panorama9 en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Panorama9.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/panorama9-tutorial/tutorial_general_01.png
[2]: ./media/panorama9-tutorial/tutorial_general_02.png
[3]: ./media/panorama9-tutorial/tutorial_general_03.png
[4]: ./media/panorama9-tutorial/tutorial_general_04.png

[100]: ./media/panorama9-tutorial/tutorial_general_100.png

[200]: ./media/panorama9-tutorial/tutorial_general_200.png
[201]: ./media/panorama9-tutorial/tutorial_general_201.png
[202]: ./media/panorama9-tutorial/tutorial_general_202.png
[203]: ./media/panorama9-tutorial/tutorial_general_203.png

