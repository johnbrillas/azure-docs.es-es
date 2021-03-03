---
title: 'Guía de inicio rápido: habilitación de la administración automática de Azure para máquinas virtuales en Azure Portal'
description: Obtenga información sobre cómo habilitar rápidamente la administración automática para máquinas virtuales en una VM nueva o existente en Azure Portal.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: jushiman
ms.openlocfilehash: 6e0e582ed37230ba3f379f193a229cfec06f066c
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101648040"
---
# <a name="quickstart-enable-azure-automanage-for-virtual-machines-in-the-azure-portal"></a>Inicio rápido: Habilitación de la administración automática para máquinas virtuales en Azure Portal

Introducción a la administración automática para máquinas virtuales mediante Azure Portal para habilitar la administración automática en una VM nueva o existente.


## <a name="prerequisites"></a>Requisitos previos

Si no tiene una suscripción a Azure, [cree una cuenta](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/) antes de empezar.

> [!NOTE]
> Las cuentas de evaluación gratuita no tienen acceso a las máquinas virtuales que se usan en este tutorial. Actualice a una suscripción de pago por uso.

> [!IMPORTANT]
> Debe tener el rol **Colaborador** en el grupo de recursos que contiene las máquinas virtuales para habilitar Automanage mediante una cuenta existente de Automanage. Si va a habilitar Automanage con una cuenta nueva de Automanage, necesita los siguientes permisos: los roles **Propietario** o **Colaborador**, junto con el rol **Administrador de acceso de usuario** de la suscripción.


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://aka.ms/AutomanagePortal-Ignite21).

## <a name="enable-automanage-for-a-single-vm"></a>Habilitación de Automanage para una única máquina virtual

1. Vaya a la máquina virtual que desea habilitar.

2. Haga clic en la entrada **Automanage (versión preliminar)** de la tabla de contenido que encontrará en **Operaciones**.

3. Seleccione **Comenzar**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-GetStarted.png" alt-text="Primeros pasos con una única máquina virtual.":::

4. Elija la configuración de Automanage (entorno, preferencias, cuenta de Automanage) y seleccione **Habilitar**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMManage-Enable.png" alt-text="Habilitar en una única máquina virtual.":::

## <a name="enable-automanage-for-multiple-vms"></a>Habilitación de Automanage para varias máquinas virtuales

1. En la barra de búsqueda, busque y seleccione **Automanage: procedimientos recomendados para máquinas de Azure**.

2. Seleccione **Enable on existing VM** (Habilitar en una máquina virtual existente).

    :::image type="content" source="media\quick-create-virtual-machine-portal\zero-vm-list-view.png" alt-text="Habilitación en una máquina virtual existente.":::

3. En la hoja **Seleccionar máquinas**:
    1. Filtre la lista de máquinas virtuales por su **Suscripción** y **Grupo de recursos**.
    1. Active la casilla de cada máquina virtual que desee incorporar.
    1. Haga clic en el botón **Seleccionar**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-select-machine.png" alt-text="Seleccione una máquina virtual existente en la lista de máquinas virtuales disponibles.":::

4. En **Perfil de configuración**, haga clic en **Browse and change profiles and preferences** (Examinar y cambiar perfiles y preferencias).

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-quick-create.png" alt-text="Examine y cambie perfiles y preferencias.":::

5. En la hoja **Selección del perfil y las preferencias de configuración**:
    1. Seleccione un perfil a la izquierda: *Desarrollo/pruebas* para pruebas, *Prod* para producción.
    1. Haga clic en el botón **Seleccionar**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Examine el perfil de configuración de producción.":::

6. Haga clic en el botón **Habilitar**.


## <a name="enable-automanage-for-a-new-vm"></a>Habilitación de Automanage para una máquina virtual nueva

Inicie sesión en Azure Portal [aquí](https://aka.ms/AutomanagePortal-Ignite21) para crear una máquina virtual y habilitar Automanage.

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.

2. En el cuadro de búsqueda que está encima de la lista de recursos de Azure Marketplace, busque y seleccione la imagen que desea utilizar y, a continuación, elija **Crear**.

> [!NOTE]
> Seleccione las [distribuciones de Linux](automanage-linux.md#supported-linux-distributions-and-versions) y las [versiones de Windows Server](automanage-windows-server.md#supported-windows-server-versions) compatibles con Automanage.

3. Rellene la pestaña **Datos básicos** con los detalles de la máquina virtual.

> [!NOTE]
> Seleccione las [regiones compatibles](automanage-virtual-machines#supported-regions) con Automanage.

4. Vaya a la pestaña **Administración** y elija su **entorno de Automanage**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\VMCreate-Management-Tab.png" alt-text="Habilitar Automanage en la pestaña Administración.":::

5. Deje los valores predeterminados restantes y luego seleccione el botón **Revisar + crear** en la parte inferior de la página.

6. Cuando vea el mensaje en el que se indica que la validación se ha realizado correctamente, seleccione **Crear**.

## <a name="disable-automanage-for-vms"></a>Deshabilitación de Automanage para máquinas virtuales

Deje de usar rápidamente Azure Automanage para máquinas virtuales deshabilitando la administración automática.

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Deshabilite Automanage en una máquina virtual.":::

1. Vaya a la página **Automanage – Azure virtual machine best practices** (Automanage: procedimientos recomendados para máquinas virtuales de Azure) en la que se muestran todas las máquinas virtuales administradas automáticamente.
1. Active la casilla situada junto a la máquina virtual que desea deshabilitar.
1. Haga clic en el botón **Disable automanagement** (Deshabilitar administración automática).
1. Lea detenidamente los mensajes del elemento emergente resultante antes de aceptar **Deshabilitar**.


## <a name="clean-up-resources"></a>Limpieza de recursos

Si ha creado un nuevo grupo de recursos para probar Azure Automanage para máquinas virtuales y ya no lo necesita, puede eliminar el grupo de recursos. Al eliminar el grupo también se eliminan la máquina virtual y todos los recursos del grupo de recursos.

Azure Automanage crea grupos de recursos predeterminados en los que almacenar los recursos. Compruebe los grupos de recursos que tienen la convención de nomenclatura "DefaultResourceGroupRegionName" y "AzureBackupRGRegionName" para limpiar todos los recursos.

1. Seleccione el **Grupo de recursos**.
1. En la página del grupo de recursos, seleccione **Eliminar**.
1. Cuando se le solicite, escriba el nombre del grupo de recursos y, después, seleccione **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, habilitó la administración automática de Azure para máquinas virtuales.

Descubra cómo puede crear y aplicar preferencias personalizadas al habilitar la administración automática en su máquina virtual.

> [!div class="nextstepaction"]
> [Azure Automanage para máquinas virtuales: perfil de configuración personalizado](virtual-machines-custom-preferences.md)
