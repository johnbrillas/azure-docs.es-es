---
title: Traslado de máquinas virtuales de Azure cifradas entre regiones con Azure Resource Mover
description: Aprenda a trasladar máquinas virtuales de Azure cifradas a otra región con Azure Resource Mover.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: tutorial
ms.date: 02/04/2021
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 0bc70e14e341d9681c75933455eae6b0278724ca
ms.sourcegitcommit: 706e7d3eaa27f242312d3d8e3ff072d2ae685956
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99981954"
---
# <a name="tutorial-move-encrypted-azure-vms-across-regions"></a>Tutorial: Traslado de máquinas virtuales de Azure cifradas entre regiones

En este artículo aprenderá a trasladar máquinas virtuales de Azure cifradas a otra región de Azure mediante [Azure Resource Mover](overview.md). Esto es lo que queremos decir con cifrado:

- Máquinas virtuales que tienen discos con Azure Disk Encryption habilitado. [Más información](../virtual-machines/windows/disk-encryption-portal-quickstart.md)
- O bien, máquinas virtuales que usan claves administradas por el cliente (CMK) para el cifrado en reposo (cifrado del lado servidor). [Más información](../virtual-machines/disks-enable-customer-managed-keys-portal.md)


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Comprobar los requisitos previos. 
> * Para las máquinas virtuales con Azure Disk Encryption habilitado, copiar las claves y los secretos del almacén de claves de la región de origen al almacén de claves de la región de destino.
> * Preparar las máquinas virtuales para su traslado y seleccionar los recursos de la región de origen que desea mover.
> * Resolver las dependencias de recursos.
> * Para las máquinas virtuales con Azure Disk Encryption habilitado, asignar manualmente el almacén de claves de destino. Para las máquinas virtuales que usan el cifrado del lado servidor con claves administradas por el cliente, asignar manualmente un conjunto de cifrado de disco en la región de destino.
> * Trasladar el almacén de claves o el conjunto de cifrado de disco.
> * Preparar y trasladar el grupo de recursos de origen. 
> * Preparar y trasladar los demás recursos.
> * Elegir si desea descartar o confirmar el traslado. 
> * Opcionalmente, quitar los recursos de la región después del traslado.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar. Luego, inicie sesión en el [Portal de Azure](https://portal.azure.com).

## <a name="prerequisites"></a>Requisitos previos

**Requisito** |**Detalles**
--- | ---
**Permisos de suscripción** | Compruebe que tiene acceso como *propietario* a la suscripción que contiene los recursos que desea trasladar.<br/><br/> **¿Por qué necesito acceso de Propietario?** La primera vez que agregue un recurso de un par de origen y destino específicos a una suscripción de Azure, Azure Resource Mover creará una [identidad administrada asignada por el sistema](../active-directory/managed-identities-azure-resources/overview.md#managed-identity-types), anteriormente llamada identidad de servicio administrado (MSI), en la que confiará la suscripción. Para crear la identidad y asignarle el rol requerido (Colaborador y Administrador de acceso de usuario en la suscripción de origen), la cuenta que utilice para agregar recursos necesita permisos de *Propietario* de la suscripción. [Obtenga más información](../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles) sobre los roles de Azure.
**Soporte técnico de máquina virtual** | Asegúrese de que las máquinas virtuales que desea trasladar están admitidas.<br/><br/> - [Compruebe](support-matrix-move-region-azure-vm.md#windows-vm-support) las máquinas virtuales Windows admitidas.<br/><br/> - [Compruebe](support-matrix-move-region-azure-vm.md#linux-vm-support) las máquinas virtuales Linux y las versiones de kernel admitidas.<br/><br/> - Compruebe la configuración admitida de [proceso](support-matrix-move-region-azure-vm.md#supported-vm-compute-settings), [almacenamiento](support-matrix-move-region-azure-vm.md#supported-vm-storage-settings) y [redes](support-matrix-move-region-azure-vm.md#supported-vm-networking-settings).
**Requisitos de Key Vault (Azure Disk Encryption)** | Si tiene habilitado Azure Disk Encryption para las máquinas virtuales, además del almacén de claves de la región de origen, necesitará un almacén de claves en la región de destino. [Cree un almacén de claves](../key-vault/general/quick-create-portal.md).<br/><br/> Para los almacenes de claves de la región de origen y de destino, necesita estos permisos:<br/><br/> - Permisos de claves: operaciones de administración de claves (Get, List); operaciones criptográficas (Decrypt y Encrypt).<br/><br/> - Permisos de secretos: operaciones de administración de secretos (Get, List y Set)<br/><br/> - Certificado (List y Get).
**Conjunto de cifrado de disco (cifrado del lado servidor con CMK)** | Si usa máquinas virtuales con cifrado del lado servidor mediante una CMK, además del conjunto de cifrado de disco de la región de origen, necesitará un conjunto de cifrado de disco en la región de destino. [Cree un conjunto de cifrado de disco](../virtual-machines/disks-enable-customer-managed-keys-portal.md#set-up-your-disk-encryption-set).<br/><br/> No se admite el traslado entre regiones si usa claves de HSM para las claves administradas por el cliente.
**Cuota de la región de destino** | La suscripción necesita tener cuota suficiente para crear los recursos que va a trasladar a la región de destino. Si no dispone de cuota suficiente, [solicite límites adicionales](../azure-resource-manager/management/azure-subscription-service-limits.md).
**Cargos de la región de destino** | Compruebe los precios y los cargos asociados con la región de destino a la que va a trasladar las máquinas virtuales. El uso de la [calculadora de precios](https://azure.microsoft.com/pricing/calculator/) le resultará útil.


## <a name="verify-key-vault-permissions-azure-disk-encryption"></a>Comprobación de los permisos de Key Vault (Azure Disk Encryption)

Si va a mover máquinas virtuales que tienen habilitado Azure Disk Encryption, en los almacenes de claves de las regiones de origen y de destino, compruebe o establezca los permisos para asegurarse de que el traslado de máquinas virtuales cifradas funcione según lo previsto. 

1. En Azure Portal, abra el almacén de claves en la región de origen.
2. En **Configuración**, seleccione **Directivas de acceso**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png" alt-text="Botón para abrir las directivas de acceso del almacén de claves." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/key-vault-access-policies.png":::

3. Si no hay permisos de usuario, seleccione **Agregar directiva de acceso** y especifique los permisos. Si la cuenta de usuario ya tiene una directiva, establezca los permisos en **Usuario**.

    - Si las máquinas virtuales que quiere trasladar tienen Azure Disk Encryption (ADE) habilitado, en **Permisos de las claves** > **Operaciones de administración de claves**, seleccione **Get** (Obtener) y **List** (Enumerar) si no están seleccionados.
    - Si usa claves administradas por el cliente (CMK) para cifrar las claves de cifrado de disco utilizadas para el cifrado en reposo (cifrado del lado servidor), en **Permisos de las claves** > **Operaciones de administración de claves**, seleccione **Get** (Obtener) y **List** (Enumerar). Además, en **Operaciones criptográficas**, seleccione **Decrypt** (Descifrar) y **Encrypt** (Cifrar).
 
    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/set-vault-permissions.png" alt-text="Lista desplegable para seleccionar los permisos del almacén de claves." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/set-vault-permissions.png":::

4. En **Permisos de los secretos**, **Operaciones de administración de secretos**, seleccione **Get** (Obtener), **List** (Enumerar) y **Set** (Establecer). 
5. Si va a asignar permisos a una nueva cuenta de usuario, en **Seleccionar la entidad de seguridad**, seleccione el usuario al que va a asignar los permisos.
6. En **Directivas de acceso**, asegúrese de que **Azure Disk Encryption para el cifrado de volúmenes** está habilitado.
7. Repita el procedimiento para el almacén de claves de la región de destino.


### <a name="copy-the-keys-to-the-destination-key-vault"></a>Copia de las claves en el almacén de claves de destino

Debe copiar los secretos y las claves de cifrado del almacén de claves de origen en el almacén de claves de destino mediante el uso de un script que se proporciona.

- Ejecute el script en PowerShell. Se recomienda ejecutar la versión más reciente de PowerShell.
- Específicamente, el script requiere estos módulos:
    - Az.Compute
    - Az.KeyVault (versión 3.0.0)
    - Az.Accounts (versión 2.2.3)

Ejecute del modo siguiente:

1. Vaya al [script](https://raw.githubusercontent.com/AsrOneSdk/published-scripts/master/CopyKeys/CopyKeys.ps1) en GitHub.
2. Copie el contenido del script en un archivo local y asígnele el nombre *Copy-keys.ps1*.
3. Ejecute el script.
4. Inicie sesión en Azure.
5. En el elemento emergente **User Input** (Entrada de usuario), seleccione la suscripción de origen, el grupo de recursos y la máquina virtual de origen. A continuación, seleccione la ubicación de destino y los almacenes de destino para el cifrado de claves y discos.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/script-input.png" alt-text="Elemento emergente para escribir los valores del script." :::


6. Cuando el script finaliza, la salida de pantalla indica que la copia de las claves se realizó correctamente.

## <a name="prepare-vms"></a>Preparación de máquinas virtuales

1. Después de [comprobar que las máquinas virtuales cumplen los requisitos](#prerequisites), asegúrese de que las máquinas virtuales que desea trasladar estén activadas. Todos los discos de las máquinas virtuales que desea que estén disponibles en la región de destino deben estar conectados e inicializados en la máquina virtual.
3. Compruebe que las máquinas virtuales dispongan de los certificados raíz de confianza más recientes, así como de una lista actualizada de revocación de certificados (CRL). Para ello, siga estos pasos:
    - En las máquinas virtuales Windows, instale las actualizaciones de Windows más recientes.
    - En las máquinas virtuales Linux, siga las indicaciones del distribuidor para que las máquinas dispongan de los certificados y la lista de revocación de certificados más recientes. 
4. Permita la conectividad saliente desde las máquinas virtuales de la manera siguiente:
    - Si utiliza un servidor proxy de firewall basado en direcciones URL para controlar la conectividad de salida, debe facilitar el acceso a estas [URL](support-matrix-move-region-azure-vm.md#url-access).
    - Si utiliza reglas de grupo de seguridad de red para determinar la conectividad saliente, cree estas [reglas de etiquetas de servicio](support-matrix-move-region-azure-vm.md#nsg-rules).

## <a name="select-resources-to-move"></a>Selección de los recursos que se van a trasladar


- Puede seleccionar cualquier tipo de recurso admitido de cualquiera de los grupos de recursos de la región de origen seleccionada.  
- Los recursos se trasladarán a una región de destino que está en la misma suscripción que la región de origen. Si desea cambiar la suscripción, puede hacerlo después de trasladar los recursos.

Seleccione los recursos como se indica a continuación:

1. En Azure Portal, busque *resource mover*. A continuación, en **Servicios**, seleccione **Azure Resource Mover**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/search.png" alt-text="Resultados de la búsqueda de &quot;resource mover&quot; en Azure Portal." :::

2. En **Overview** (Información general), haga clic en **Move across regions** (Mover entre regiones).

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png" alt-text="Botón que permite agregar recursos para trasladarlos a otra región." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/move-across-regions.png":::

3. En **Mover recursos** > **Origen + destino**, seleccione la suscripción y la región de origen.
4. En **Destino**, seleccione la región a la que desea trasladar las máquinas virtuales. A continuación, haga clic en **Siguiente**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/source-target.png" alt-text="Página para seleccionar las regiones de origen y de destino." :::

5. En **Recursos que se van a mover** , haga clic en **Seleccionar recursos**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-resources.png" alt-text="Botón para seleccionar el recurso que se va a trasladar." :::

6. En **Select resources** (Seleccionar recursos), seleccione las máquinas virtuales. Solo es posible agregar [recursos que se puedan mover](#prepare-vms). A continuación, haga clic en **Hecho**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-vm.png" alt-text="Página para seleccionar las máquinas virtuales que se van a trasladar." :::

    > [!NOTE]
    >  En este tutorial, vamos a seleccionar una máquina virtual que usa el cifrado del lado servidor (rayne-vm) con una clave administrada por el cliente y una máquina virtual con Disk Encryption habilitado (rayne-vm-ade).

7.  En **Recursos que se van a mover**, haga clic en **Siguiente**.
8. En **Review** (Revisar), compruebe la configuración de origen y la de destino. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/review.png" alt-text="Página para revisar la configuración y realizar el traslado de los recursos." :::

9. Haga clic en **Continuar** para empezar a agregar recursos.
10. Seleccione el icono de notificaciones para realizar un seguimiento del progreso. Una vez que el proceso de agregar finalice correctamente, seleccione **Added resource(s) to move** (Se han agregado recursos para mover) en las notificaciones.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png" alt-text="Notificación para confirmar que los recursos se han agregado correctamente." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/added-resources-notification.png":::
    
    
11. Después de hacer clic en la notificación, revise los recursos de la página **Entre regiones**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-prepare-pending.png" alt-text="Páginas que muestran recursos agregados con la preparación pendiente." :::

> [!NOTE]
> - Los recursos que agregue se colocan en estado *Prepare pending* (Preparación pendiente).
> - El grupo de recursos de las máquinas virtuales se agrega automáticamente.
> - Si modifica las entradas de la columna **Destination configuration** (Configuración de destino) para usar un recurso que ya existe en la región de destino, el estado del recurso se establece en *Commit pending* (Pendiente de confirmación), ya que no es necesario iniciar un traslado.
> - Si quiere eliminar un recurso que se ha agregado, el método para hacerlo dependerá del punto en el que se encuentre en el proceso de traslado. [Más información](remove-move-resources.md).


## <a name="resolve-dependencies"></a>Resolución de dependencias

1. Si algún recurso muestra el mensaje *Validate dependencies* (Validar dependencias) en la columna **Issues** (Problemas), seleccione el botón **Validate dependencies** (Validar dependencias).

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png" alt-text="Botón para comprobar las dependencias." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/check-dependencies.png":::

    Se iniciará el proceso de validación.
2. Si se encuentran dependencias, haga clic en **Add dependencies** (Agregar dependencias).  

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png" alt-text="Botón para agregar dependencias." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/add-dependencies.png":::


3. En **Add dependencies** (Agregar dependencias), deje la opción predeterminada **Show all dependencies** (Mostrar todas las dependencias).

    - La opción **Show all dependencies** (Mostrar todas las dependencias) recorre en iteración todas las dependencias directas e indirectas de un recurso. Por ejemplo, para una máquina virtual, muestra la NIC, la red virtual, los grupos de seguridad de red (NSG), etc.
    - La opción **Show first level dependencies only** (Mostrar solo dependencias de primer nivel) muestra solo las dependencias directas. Por ejemplo, para una máquina virtual, muestra la NIC, pero no la red virtual.
 
4. Seleccione los recursos dependientes que desea agregar y, a continuación, seleccione **Add dependencies** (Agregar dependencias).

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png" alt-text="Selección de dependencias de la lista de dependencias." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-dependencies.png":::

5. Vuelva a validar las dependencias. 

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png" alt-text="Página para volver a validar." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/validate-again.png":::

## <a name="assign-destination-resources"></a>Asignación de los recursos de destino

Los recursos de destino asociados al cifrado necesitan una asignación manual.

- Si va a mover una máquina virtual que tiene Azure Disk Encryption (ADE) habilitado, el almacén de claves de la región de destino aparecerá como una dependencia.
- Si va a mover una máquina virtual que tiene cifrado del lado servidor y que usa claves administradas por el cliente (CMK), el conjunto de cifrado de disco de la región de destino aparece como una dependencia. 
- Dado que en este tutorial se trasladan una máquina virtual con ADE habilitado y una máquina virtual con CMK, tanto el almacén de claves de destino como el conjunto de cifrado de disco se muestran como dependencias.

Realice una asignación manual como se indica a continuación:

1. En la entrada del conjunto de cifrado de disco, seleccione **Resource not assigned** (Recurso no asignado) en la columna **Destination configuration** (Configuración de destino).
2. En **Configuration settings** (Opciones de configuración), seleccione el conjunto de cifrado de disco de destino. A continuación, seleccione **Save changes** (Guardar cambios).
3. Puede seleccionar guardar y validar las dependencias del recurso que está modificando, o simplemente guardar los cambios y validar todo lo que modifique de una sola vez.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png" alt-text="Página para seleccionar el conjunto de cifrado de disco de la región de destino." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/select-destination-set.png":::

    Después de agregar el recurso de destino, el estado del conjunto de cifrado de disco se convierte en *Commit move pending* (Confirmar movimiento pendiente).
3. En la entrada del almacén de claves, seleccione **Resource not assigned** (Recurso no asignado) en la columna **Destination configuration** (Configuración de destino). En **Configuration settings** (Opciones de configuración), seleccione el almacén de claves de destino. Guarde los cambios. 

En esta fase, el estado del conjunto de cifrado de disco y del almacén de claves se convierten en *Commit move pending* (Confirmar movimiento pendiente).

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png" alt-text="Página para seleccionar la preparación de otros recursos." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-other-resources.png":::

Para confirmar y finalizar el proceso de traslado de los recursos de cifrado.

1. En **Across regions** (Entre regiones), seleccione el recurso (conjunto de cifrado de disco o almacén de claves) > **Commit move** (Confirmar traslado).
2. En **Mover recursos**, haga clic en **Confirmar**.

> [!NOTE]
> Una vez confirmado el traslado, el recurso tendrá el estado *Delete source pending* (Eliminar origen pendiente).


## <a name="move-the-source-resource-group"></a>Traslado del grupo de recursos de origen 

Antes de poder preparar y trasladar las máquinas virtuales, el grupo de recursos de la máquina virtual debe estar presente en la región de destino. 

### <a name="prepare-to-move-the-source-resource-group"></a>Preparación del grupo de recursos de origen para el traslado

Durante el proceso de preparación, Resource Mover genera plantillas de Azure Resource Manager mediante la configuración del grupo de recursos. Los recursos que pertenecen al grupo no se ven afectados.

Lleve a cabo los siguientes preparativos:

1. En **Entre regiones**, seleccione el grupo de recursos de origen > **Preparar**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png" alt-text="Preparación del grupo de recursos." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/prepare-resource-group.png":::

2. En **Preparar recursos**, haga clic en **Preparar**.

> [!NOTE]
> Una vez preparado el grupo de recursos, tendrá el estado *Initiate move pending* (Iniciar movimiento pendiente). 

 
### <a name="move-the-source-resource-group"></a>Traslado del grupo de recursos de origen

Empiece a trasladar el grupo de recursos como se indica a continuación:

1. En **Entre regiones**, seleccione el grupo de recursos > **Iniciar movimiento**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png" alt-text="Botón para iniciar el traslado." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/initiate-move-resource-group.png":::

2. En **Mover recursos**, haga clic en **Iniciar movimiento**. El grupo de recursos pasará al estado *Initiate move in progress* (Iniciar movimiento en curso).   
3. Una vez iniciado el traslado, se creará el grupo de recursos de destino, basado en la plantilla de Resource Manager generada. El grupo de recursos de origen pasa al estado *Commit move Pending* (Confirmar movimiento pendiente).

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png" alt-text="Revisión del estado Confirmar movimiento pendiente." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-commit-move-pending.png":::

Para confirmar y finalizar el proceso de traslado:

1. En **Entre regiones**, seleccione el grupo de recursos > **Confirmar movimiento**.
2. En **Mover recursos**, haga clic en **Confirmar**.

> [!NOTE]
> Una vez confirmado el traslado, el grupo de recursos de origen tendrá el estado *Delete source pending* (Eliminar origen pendiente).

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png" alt-text="Revisión del estado Eliminar origen pendiente." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resource-group-delete-move-pending.png":::

## <a name="prepare-resources-to-move"></a>Preparación de los recursos que se van a trasladar

Ahora que los recursos de cifrado y el grupo de recursos de origen se han trasladado, puede preparar el traslado de otros recursos que tengan el estado *Prepare pending* (Preparación pendiente).


1. En **Across regions** (Entre regiones), vuelva a validar y resuelva los problemas.
2. Si desea editar la configuración de destino antes de trasladar los recursos, seleccione el vínculo en la columna **Destination configuration** (Configuración de destino) para el recurso y edite la configuración. Si edita la configuración de la máquina virtual de destino, el tamaño de esta máquina virtual no puede ser inferior al tamaño de la máquina virtual de origen.
3. Seleccione **Prepare** (Preparar) para los recursos en estado *Prepare pending* (Preparación pendiente) que desea trasladar.
3. En **Prepare resources** (Preparar recursos), seleccione **Prepare** (Preparar).

    - Durante el proceso de preparación, el agente de movilidad de Azure Site Recovery se instala en las máquinas virtuales para replicarlas.
    - Los datos de las máquinas virtuales se replican periódicamente en la región de destino. Esto no afecta a la máquina virtual de origen.
    - Resource Mover genera plantillas de Resource Manager para los demás recursos de origen.

Una vez preparados los recursos, pasarán al estado *Initiate move pending* (Iniciar movimiento pendiente).

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png" alt-text="Página que muestra los recursos que tienen el estado Iniciar movimiento pendiente." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-initiate-move-pending.png":::



## <a name="initiate-the-move"></a>Inicio de la migración

Con los recursos ya preparados, puede iniciar el traslado. 

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Initiate move pending* (Iniciar movimiento pendiente). A continuación, haga clic en **Iniciar movimiento**.
2. En **Mover recursos**, haga clic en **Iniciar movimiento**.
3. Haga un seguimiento del progreso del traslado en la barra de notificaciones.

    - En el caso de las máquinas virtuales, se crearán réplicas en la región de destino. La máquina virtual de origen se apagará y se producirá cierto tiempo de inactividad (normalmente es cuestión de minutos).
    - Resource Mover recreará otros recursos mediante las plantillas de Resource Manager preparadas. Normalmente, no se produce tiempo de inactividad.
    - Después de trasladar los recursos, tendrán el estado *Commit move pending* (Confirmar movimiento pendiente).

:::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" alt-text="Página que muestra los recursos en estado Confirmar movimiento pendiente." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move-pending.png" :::


## <a name="discard-or-commit"></a>¿Descartar o confirmar?

Tras la operación inicial de traslado, puede decidir si desea confirmarla o descartarla. 

- **Descartar**: puede descartar un traslado cuando esté haciendo pruebas y no quiera trasladar verdaderamente el recurso de origen. Si el traslado se descarta, el recurso volverá a tener el estado *Initiate move pending* (Iniciar movimiento pendiente).
- **Confirmación**: la confirmación completa la operación de traslado a la región de destino. Tras la confirmación, un recurso de origen tendrá el estado *Delete source pending* (Eliminar origen pendiente) y el usuario puede decidir si desea eliminarlo.


## <a name="discard-the-move"></a>Descartar la operación de traslado 

Para descartar la operación de traslado, siga estos pasos:

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Commit move pending* (Confirmar movimiento pendiente) y haga clic en **Descartar movimiento**.
2. En **Descartar movimiento**, haga clic en **Descartar**.
3. Haga un seguimiento del progreso del traslado en la barra de notificaciones.


> [!NOTE]
> Después de haber descartado recursos, las máquinas virtuales tendrán el estado *Initiate move pending* (Iniciar movimiento pendiente).

## <a name="commit-the-move"></a>Confirmación de la operación de traslado

Si desea completar la operación de traslado, confírmela. 

1. En **Entre regiones**, seleccione los recursos que tengan el estado *Commit move pending* (Confirmar movimiento pendiente) y haga clic en **Confirmar movimiento**.
2. En **Confirmar recursos**, haga clic en **Confirmar**.

    :::image type="content" source="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" alt-text="Página para confirmar los recursos y completar la operación de traslado." lightbox="./media/tutorial-move-region-encrypted-virtual-machines/resources-commit-move.png" :::

3. Haga un seguimiento del progreso de la operación de confirmación en la barra de notificaciones.

> [!NOTE]
> - Tras confirmar el traslado, las máquinas virtuales dejarán de replicarse. La confirmación no afecta a la máquina virtual de origen.
> - La confirmación tampoco afecta a los recursos de redes de origen.
> - Una vez confirmado el traslado, los recursos tendrán el estado *Delete source pending* (Eliminar origen pendiente).



## <a name="configure-settings-after-the-move"></a>Configuración de parámetros tras el traslado

- El servicio Mobility no se desinstala automáticamente de las máquinas virtuales. Puede desinstalarlo manualmente, o bien mantenerlo si tiene previsto trasladar el servidor de nuevo.
- Modifique las reglas de control de acceso basado en rol (RBAC) de Azure después de la migración.

## <a name="delete-source-resources-after-commit"></a>Eliminación de los recursos de origen después de la confirmación

Después del traslado, puede eliminar los recursos de la región de origen si lo desea. 

1. En **Across Regions** (Entre regiones), seleccione cada uno de los recursos de origen que desee eliminar. A continuación, seleccione **Delete source** (Eliminar origen).
2. En **Delete source** (Eliminar origen), revise lo que intenta eliminar y, en **Confirm delete** (Confirmar eliminación), escriba **yes** (Sí). La acción es irreversible, por lo que debe comprobarlo con cuidado.
3. Después de escribir **yes** (Sí), seleccione **Delete source** (Eliminar origen).

> [!NOTE]
>  En el portal de Resource Mover no se pueden eliminar grupos de recursos, almacenes de claves ni servidores SQL Server. Debe eliminarlos de forma individual en la página de propiedades de cada recurso.


## <a name="delete-additional-resources-created-for-move"></a>Eliminación de recursos adicionales creados para el traslado

Después del traslado, puede eliminar manualmente la colección de traslado, junto con los recursos de Site Recovery que se crearon.

- La colección de traslado está oculta de forma predeterminada. Para verla, debe activar los recursos ocultos.
- El almacenamiento en caché tiene un bloqueo que debe eliminarse antes de que se pueda eliminar la colección.

Realice la eliminación de la siguiente manera: 
1. Identifique los recursos pertenecientes al grupo ```RegionMoveRG-<sourceregion>-<target-region>```.
2. Compruebe que todas las máquinas virtuales y los demás recursos de la región de origen se hayan trasladado o eliminado. De este modo, se garantiza que no haya recursos pendientes que los utilicen.
2. Eliminación de los recursos:

    - El nombre de la colección de traslado es ```movecollection-<sourceregion>-<target-region>```.
    - El nombre de la cuenta de almacenamiento en caché es ```resmovecache<guid>```.
    - El nombre del almacén es ```ResourceMove-<sourceregion>-<target-region>-GUID```.
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Ha trasladado máquinas virtuales de Azure cifradas y sus recursos dependientes a otra región de Azure.


Ahora veremos cómo trasladar grupos elásticos y bases de datos de Azure SQL a otra región.

> [!div class="nextstepaction"]
> [Traslado de recursos de Azure SQL](./tutorial-move-region-sql.md)
