---
title: Creación del jumpbox de Azure VMware Solution
description: Pasos para crear el jumpbox de Azure VMware Solution.
ms.topic: include
ms.date: 03/13/2021
ms.openlocfilehash: f746e11763e1df1686f3134960dea167bf1c9908
ms.sourcegitcommit: afb9e9d0b0c7e37166b9d1de6b71cd0e2fb9abf5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/14/2021
ms.locfileid: "103462272"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-access-private-cloud.md -->

1. En el grupo de recursos, seleccione **+ Agregar**, busque y seleccione **Microsoft Windows 10** y, a continuación, seleccione **Crear**.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Adición de una nueva máquina virtual de Windows 10 para un jumpbox" border="true":::.

1. Escriba la información requerida en los campos y seleccione **Revisar y crear**. 

   Para más información sobre los campos, consulte la tabla siguiente.

   | Campo | Valor |
   | --- | --- |
   | **Suscripción** | El valor se rellena previamente con la suscripción a la que pertenece el grupo de recursos. |
   | **Grupos de recursos** | El valor se rellena previamente para el grupo de recursos actual que creó en el tutorial anterior.  |
   | **Nombre de la máquina virtual** | Introduzca un nombre único para la máquina virtual. |
   | **Región** | Seleccione la ubicación geográfica de la máquina virtual. |
   | **Opciones de disponibilidad** | Deje el valor predeterminado seleccionado. |
   | **Imagen** | Seleccione la imagen de la máquina virtual. |
   | **Tamaño** | Deje el valor de tamaño predeterminado. |
   | **Tipo de autenticación**  | Seleccione **Contraseña**. |
   | **Nombre de usuario** | Escriba el nombre de usuario para iniciar sesión en la máquina virtual. |
   | **Contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
   | **Confirmar contraseña** | Escriba la contraseña para iniciar sesión en la máquina virtual. |
   | **Puertos de entrada públicos** | Seleccione **Ninguno**. Si selecciona Ninguno, puede usar el [acceso JIT](../../security-center/security-center-just-in-time.md#jit-configure) para controlar el acceso a la máquina virtual solo cuando desee acceder a ella. Como alternativa, puede usar [Azure Bastion](../../bastion/tutorial-create-host-portal.md) si quiere tener acceso al servidor de jumpbox de forma segura desde Internet sin exponer ningún puerto de red.  |


1. Una vez superada la validación, seleccione **Crear** para iniciar el proceso de creación de la máquina virtual.

