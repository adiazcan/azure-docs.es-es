---
title: 'Tutorial: Integración de Azure Active Directory con Imagineer WebVision | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Imagineer WebVision.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b936a3f4-74c1-4437-b0f7-6d1b1de38bb1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2018
ms.author: jeedes
ms.openlocfilehash: d86004680bf13c9716b4ff4e7a41af73ea186f27
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39047195"
---
# <a name="tutorial-azure-active-directory-integration-with-imagineer-webvision"></a>Tutorial: Integración de Azure Active Directory con Imagineer WebVision

En este tutorial, obtendrá información sobre cómo integrar Imagineer WebVision con Azure Active Directory (Azure AD).

La integración de Imagineer WebVision con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Imagineer WebVision.
- Puede permitir que los usuarios inicien sesión automáticamente en Imagineer WebVision (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Imagineer WebVision, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción que permita el inicio de sesión único en Imagineer WebVision

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Imagineer WebVision desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-imagineer-webvision-from-the-gallery"></a>Adición de Imagineer WebVision desde la Galería
Para configurar la integración de Imagineer WebVision en Azure AD, es preciso agregar Imagineer WebVision desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Imagineer WebVision desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Imagineer WebVision**, seleccione **Imagineer WebVision** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Imagineer WebVision en la lista de resultados](./media/imagineerwebvision-tutorial/tutorial_imagineerwebvision_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Imagineer WebVision con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Imagineer WebVision para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Imagineer WebVision.

Para configurar y probar el inicio de sesión único de Azure AD con Imagineer WebVision, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Imagineer WebVision](#create-an-imagineer-webvision-test-user)**: para tener un homólogo de Britta Simon en Imagineer WebVision que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Imagineer WebVision.

**Para configurar el inicio de sesión único de Azure AD con Imagineer WebVision, siga estos pasos:**

1. En Azure Portal, en la página de integración de aplicaciones **Imagineer WebVision**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/imagineerwebvision-tutorial/tutorial_imagineerwebvision_samlbase.png)

3. En la sección **Dominio y direcciones URL de Imagineer WebVision**, lleve a cabo los pasos siguientes:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Imagineer WebVision](./media/imagineerwebvision-tutorial/tutorial_imagineerwebvision_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<YOUR SERVER URL>/<yourapplicationloginpage>`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<YOUR SERVER URL>/<yourapplicationloginpage>`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte al cliente de Imagineer WebVision](mailto:support@itgny.com) para obtener estos valores.

4. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/imagineerwebvision-tutorial/tutorial_imagineerwebvision_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/imagineerwebvision-tutorial/tutorial_general_400.png)

6. Para configurar el inicio de sesión único en **Imagineer WebVision**, tiene que enviar la **dirección URL de metadatos de federación de la aplicación** generada al [equipo de soporte técnico de Imagineer WebVision](mailto:support@itgny.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/imagineerwebvision-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/imagineerwebvision-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/imagineerwebvision-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/imagineerwebvision-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-an-imagineer-webvision-test-user"></a>Creación de un usuario de prueba de Imagineer WebVision

En esta sección, creará el usuario Britta Simon en Imagineer WebVision. Trabaje con el [equipo de soporte técnico de Imagineer WebVision](mailto:support@itgny.com) para agregar los usuarios en la plataforma Imagineer WebVision. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Imagineer WebVision para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Imagineer WebVision, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Imagineer WebVision**.

    ![Vínculo de Imagineer WebVision en la lista de aplicaciones](./media/imagineerwebvision-tutorial/tutorial_imagineerwebvision_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Imagineer WebVision en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Imagineer WebVision.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/imagineerwebvision-tutorial/tutorial_general_01.png
[2]: ./media/imagineerwebvision-tutorial/tutorial_general_02.png
[3]: ./media/imagineerwebvision-tutorial/tutorial_general_03.png
[4]: ./media/imagineerwebvision-tutorial/tutorial_general_04.png

[100]: ./media/imagineerwebvision-tutorial/tutorial_general_100.png

[200]: ./media/imagineerwebvision-tutorial/tutorial_general_200.png
[201]: ./media/imagineerwebvision-tutorial/tutorial_general_201.png
[202]: ./media/imagineerwebvision-tutorial/tutorial_general_202.png
[203]: ./media/imagineerwebvision-tutorial/tutorial_general_203.png

