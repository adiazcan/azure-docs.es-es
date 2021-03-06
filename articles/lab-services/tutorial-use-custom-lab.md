---
title: Acceso a un laboratorio en Azure DevTest Labs | Microsoft Docs
description: En este tutorial, va a acceder al laboratorio que se ha creado con Azure DevTest Labs, reclamar máquinas virtuales, utilizarlas y, después, anular la reclamación.
services: devtest-lab, lab-services, virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: ab52206230c4dfe2d92c97f1e291ee00a086c570
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49470870"
---
# <a name="tutorial-access-a-lab-in-azure-devtest-labs"></a>Tutorial: Acceso a un laboratorio en Azure DevTest Labs
En este tutorial, va a utilizar el laboratorio que se creó en el [Tutorial: Creación de un laboratorio en Azure DevTest Labs](tutorial-create-custom-lab.md).

En este tutorial realizará lo siguiente:

> [!div class="checklist"]
> * Notificación de una máquina virtual en el laboratorio
> * Conexión a la máquina virtual
> * Anulación de la reclamación de la máquina virtual

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="access-the-lab"></a>Acceso al laboratorio

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione **Todos los recursos** en el menú izquierdo. 
3. Seleccione **DevTest Labs** como tipo de recurso. 
4. Seleccione el laboratorio. 

    ![Selección del laboratorio](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Reclamación de una máquina virtual

1. En la lista de **Máquinas virtuales que se pueden reclamar**, seleccione **...**  (puntos suspensivos) y seleccione **Reclamar máquina**.

    ![Reclamación de una máquina virtual](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. Confirme que ve la máquina virtual en la lista **Mis máquinas virtuales**.

    ![Mi máquina virtual](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

1. Seleccione la máquina virtual en la lista. Verá la **página de máquina virtual** para la suya. Seleccione **Conectar** en la barra de herramientas.

    ![Conexión a la máquina virtual](./media/tutorial-use-custom-lab/connect-button.png)
2. Guarde el archivo **RDP** descargado en el disco duro y utilícelo para conectarse a la máquina virtual. Especifique el nombre de usuario y la contraseña que mencionó cuando se creó la máquina virtual en la sección anterior. 

    > [!NOTE] 
    > Para conectarse a una máquina virtual Linux, el acceso RDP o SSH debe estar habilitado para la máquina virtual. Para conocer el procedimiento de conexión a una máquina virtual Linux a través de RDP, consulte [Instalación y configuración de Escritorio remoto para conectarse a una máquina virtual Linux en Azure](../virtual-machines/linux/use-remote-desktop.md). 


## <a name="unclaim-the-vm"></a>Anulación de la reclamación de la máquina virtual
Cuando haya terminado de usar la máquina virtual, anule la reclamación de dicha máquina siguiendo estos pasos: 

1. En la página de la máquina virtual, seleccione **Anular reclamación** en la barra de herramientas. 

    ![Anulación de la reclamación de una máquina virtual](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. La máquina virtual se apaga antes de anular su reclamación. 

    ![Anulación de reclamación de estado](./media/tutorial-use-custom-lab/unclaim-status.png) 
1. Después de realizar la operación de anulación de reclamación, verá la máquina virtual en la lista **Máquinas virtuales que se pueden reclamar** en la parte inferior. 
    
## <a name="next-steps"></a>Pasos siguientes
Este tutorial le ha mostrado cómo acceder y usar un laboratorio creado con Azure DevTest Labs. Para más información sobre el acceso y el uso de máquinas virtuales en un laboratorio, consulte 

> [!div class="nextstepaction"]
> [Procedimiento: Uso de máquinas virtuales en un laboratorio](devtest-lab-add-vm.md)

