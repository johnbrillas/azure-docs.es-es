---
title: Solución de problemas de recuperación de archivos de máquina virtual de Azure
description: Solución de problemas al recuperar archivos y carpetas desde una copia de seguridad de máquina virtual de Azure.
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: 0ee856b10c6a5fbea6f6f76b2082949ab9c1e0db
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101700310"
---
# <a name="troubleshoot-issues-in-file-recovery-of-an-azure-vm-backup"></a>Solución de problemas con la recuperación de archivos de una copia de seguridad de máquina virtual de Azure

En este artículo se proporcionan pasos de solución de problemas que pueden ayudarle a resolver problemas al recuperar archivos y carpetas de una copia de seguridad de máquina virtual (VM) de Azure.

## <a name="common-error-messages"></a>Mensajes comunes de error

En esta sección se proporcionan pasos para corregir los mensajes de error que pueden aparecer.

### <a name="exception-caught-while-connecting-to-target"></a>"Exception caught while connecting to Target" (Excepción detectada al conectarse al destino)

**Causa posible**: El script no puede acceder al punto de recuperación.

**Acción recomendada**: Para resolver este problema, siga los pasos que se indican en [El script se ejecuta, pero la conexión genera un error](#the-script-runs-but-the-connection-to-the-iscsi-target-failed).

### <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>"The target has already been logged in via an iSCSI session" (El destino ya ha iniciado sesión mediante una sesión iSCSI)

**Causa posible**: El script ya se ejecutó en la misma máquina y se han conectados las unidades.

**Acción recomendada**: Ya se han conectado los volúmenes del punto de recuperación. No se pueden montar con las mismas letras de unidad de la máquina virtual original. Examine los volúmenes disponibles en el Explorador de archivos.

### <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>"This script is invalid because the disks have been dismounted via portal/exceeded the 12-hr limit. Download a new script from the portal" (Este script no es válido porque se han desmontado los discos a través del portal o se ha superado el límite de 12 horas. Descargue un nuevo script del portal)

**Causa posible**: Se han desmontado los discos desde el portal o se ha superado el límite de tiempo de 12 horas.

**Acción recomendada**: 12 horas después de descargar el script, deja de ser válido y no se puede ejecutar. Vaya al portal y descargue un nuevo script para continuar con la recuperación de archivos.

### <a name="iscsi_tcp-module-cant-be-loaded-or-iscsi_tcp_module-not-found"></a>"iscsi_tcp module can’t be loaded (or) iscsi_tcp_module not found" (no se puede cargar el módulo iscsi_tcp (o) iscsi_tcp_module no se encuentra)

**Acción recomendada**: Para resolver este problema, siga los pasos descritos en [El script se descarga correctamente, pero no se puede ejecutar](#the-script-downloads-successfully-but-fails-to-run).

## <a name="common-problems"></a>Problemas comunes

En esta sección se proporcionan los pasos para solucionar problemas que puede experimentar al descargar y ejecutar el script de recuperación de archivos.

### <a name="you-cant-download-the-script"></a>No se puede descargar el script

1. Asegúrese de que tiene los [permisos necesarios para descargar el script](./backup-azure-restore-files-from-vm.md#select-recovery-point-who-can-generate-script).
1. Compruebe la conexión a las direcciones IP de destino de Azure. Ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:

   `nslookup download.microsoft.com`

    or

    `ping download.microsoft.com`

### <a name="the-script-downloads-successfully-but-fails-to-run"></a>El script se descarga correctamente, pero no se puede ejecutar

Al ejecutar el script de Python para la recuperación de nivel de elemento (ILR) en SUSE Linux Enterprise Server 12 SP4, se produce el error "iscsi_tcp module can’t be loaded" or "iscsi_tcp_module not found" (no se puede cargar el módulo iscsi_tcp o no se encuentra iscsi_tcp_module).

**Causa posible**: El módulo ILR usa **iscsi_tcp** para establecer una conexión TCP con el servicio de copia de seguridad. En la versión SLES 12 SP4, SUSE quitó **iscsi_tcp** del paquete open-iscsi; por lo tanto, se produce un error en la operación de ILR.

**Acción recomendada**:  La ejecución del script de recuperación de archivos no se admite en máquinas virtuales SUSE 12 SP4. Pruebe la operación de restauración en una versión anterior de SUSE 12 SP4.

### <a name="the-script-runs-but-the-connection-to-the-iscsi-target-failed"></a>El script se ejecuta, pero no se pudo establecer la conexión con el destino iSCSI

Es posible que vea el mensaje de error "Exception caught while connecting to Target" (Excepción detectada al conectarse al destino).

1. Asegúrese de que la máquina en la que se ejecuta el script cumple todos los [requisitos de acceso](./backup-azure-restore-files-from-vm.md#step-4-access-requirements-to-successfully-run-the-script).
1. Compruebe la conexión a las direcciones IP de destino de Azure. Ejecute los siguientes comandos en un símbolo del sistema con privilegios elevados:

   `nslookup download.microsoft.com`

   or

   `ping download.microsoft.com`
1. Asegúrese de tener acceso al puerto de salida iSCSI 3260.
1. Compruebe que ningún firewall o grupo de seguridad de red bloquea el tráfico a las direcciones IP de destino de Azure o a las direcciones URL del servicio de recuperación.
1. Asegúrese de que el software antivirus no impide que se ejecute el script.

### <a name="youre-connected-to-the-recovery-point-but-the-disks-werent-attached"></a>Está conectado al punto de recuperación, pero los discos no se asociaron

Para solucionar este problema, siga los pasos correspondientes a su sistema operativo.

#### <a name="windows-file-recovery-fails-on-server-with-storage-pools"></a>La recuperación de archivos de Windows no se realiza correctamente en servidores con bloques de almacenamiento

Al ejecutar el script por primera vez en Windows Server 2012 R2 y Windows Server 2016 (con bloques de almacenamiento), el bloque de almacenamiento podría estar asociado a la máquina virtual en modo de solo lectura.

>[!Tip]
> Asegúrese de que dispone de la [máquina correcta para ejecutar el script](./backup-azure-restore-files-from-vm.md#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).

Para resolver este problema, asigne manualmente acceso de lectura y escritura al bloque de almacenamiento y asocie los discos virtuales:

1. Vaya a **Administrador del servidor** > **Servicios de archivos y almacenamiento** > **Volúmenes** > **Bloques de almacenamiento**.

   ![Captura de pantalla que muestra las opciones de Bloques de almacenamiento.](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

1. En la ventana **Bloque de almacenamiento**, haga clic con el botón derecho en el bloque de almacenamiento disponible y seleccione la opción **Establecer acceso de lectura y escritura**.

   ![Captura de pantalla que muestra las opciones de clic con el botón derecho de un bloque de almacenamiento.](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

1. Una vez asignado el acceso de lectura y escritura al bloque de almacenamiento, haga clic con el botón derecho en la sección **Discos virtuales** y, después, seleccione **Exponer disco virtual**.

   ![Captura de pantalla que muestra opciones de clic con el botón derecho de un disco virtual.](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

#### <a name="linux-file-recovery-fails-to-auto-mount-because-the-disk-doesnt-contain-volumes"></a>En la recuperación de archivos de Linux no se puede llevar a cabo la operación de montaje automático porque el disco no contiene volúmenes

Al realizar la recuperación de archivos, el servicio de copia de seguridad detecta los volúmenes y los monta automáticamente. Sin embargo, si los discos de copia de seguridad tienen particiones sin formato, esos discos no se montan automáticamente y no se puede ver el disco de datos para la recuperación.

Para resolver este problema, vaya a [Recuperación de archivos de las copias de seguridad de máquina virtual de Azure](./backup-azure-restore-files-from-vm.md#lvmraid-arrays-for-linux-vms).

#### <a name="linux-file-recovery-fails-because-the-os-couldnt-identify-the-file-system"></a>Se produce un error en la recuperación de archivos de Linux porque el sistema operativo no puede identificar el sistema de archivos

Al ejecutar el script de recuperación de archivos, el disco de datos no se puede asociar. Aparece el mensaje "The following partitions failed to mount since the OS couldn't identify the filesystem" (No se pudieron montar las siguientes particiones porque el sistema operativo no pudo identificar el sistema de archivos)

Para resolver este problema, compruebe si el volumen está cifrado con una aplicación de otro fabricante. Si lo está, el disco o la máquina virtual no se mostrarán como cifrados en el portal.

1. Inicie sesión en la máquina virtual de copia de seguridad y ejecute el siguiente comando:

   `lsblk -f`

   ![Captura de pantalla que muestra los resultados del comando para enumerar los dispositivos de bloqueo.](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

1. Compruebe el sistema de archivos y el cifrado. Si el volumen está cifrado, no se admite la recuperación de archivos. Puede encontrar más información en [Matriz de compatibilidad de copia de seguridad de máquinas virtuales de Azure](./backup-support-matrix-iaas.md#support-for-file-level-restore).

### <a name="disks-are-attached-but-the-volumes-arent-mounted"></a>Los discos están asociados, pero los volúmenes no están montados

Para solucionar este problema, siga los pasos correspondientes a su sistema operativo.

#### <a name="windows"></a>Windows

Al ejecutar el script de recuperación de archivos para Windows, verá un mensaje "0 recovery volumes attached" (0 volúmenes de recuperación asociados). pero los discos se detectan en la consola de administración de discos.

**Causa posible**: Cuando se asociaron volúmenes mediante iSCSI, algunos volúmenes detectados se desconectaron. Cuando el canal iSCSI se comunica entre la máquina virtual y el servicio, detecta estos volúmenes y los pone en línea, pero no se montan.

   ![Captura de pantalla que muestra 0 volúmenes de recuperación asociados.](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Para identificar y resolver este problema, siga estos pasos:

>[!Tip]
>Asegúrese de que dispone de la [máquina correcta para ejecutar el script](./backup-azure-restore-files-from-vm.md#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).

1. En la ventana de **cmd**, ejecute **diskmgmt** para abrir **Administración de discos**.
1. Busque cualquier disco adicional. En el ejemplo siguiente, el **disco 2** es un disco adicional.

   ![Captura de pantalla de la ventana Administración de discos con un disco adicional.](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

1. Haga clic con el botón derecho en **Nuevo volumen** y seleccione **Cambiar la letra y rutas de acceso de unidad**.

   ![Captura de pantalla que muestra las opciones de clic con el botón derecho en el disco adicional.](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

1. En la ventana **Cambiar la ruta de acceso o la letra de unidad**, seleccione **Asignar la letra de unidad siguiente**, asigne una unidad disponible y elija **Aceptar**.

   ![Captura de pantalla de la ventana Cambiar la ruta de acceso o la letra de unidad.](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

1. Abra el Explorador de archivos para ver la unidad que eligió y explore los archivos.

#### <a name="linux"></a>Linux

>[!Tip]
>Asegúrese de que dispone de la [máquina correcta para ejecutar el script](./backup-azure-restore-files-from-vm.md#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).

Si la máquina virtual Linux protegida usa matrices LVM o RAID, siga los pasos descritos en [Recuperación de archivos de las copias de seguridad de máquina virtual de Azure](./backup-azure-restore-files-from-vm.md#lvmraid-arrays-for-linux-vms).

### <a name="you-cant-copy-the-files-from-mounted-volumes"></a>No se pueden copiar los archivos de volúmenes montados

La copia podría producir un error "0x80070780": El sistema no tiene acceso al archivo". 

Compruebe si el servidor de origen tiene habilitada la desduplicación de disco. En caso afirmativo, asegúrese de que el servidor de restauración también tenga habilitada la desduplicación en las unidades. Puede dejar la desduplicación sin configurar para no desduplicar las unidades del servidor de restauración.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](backup-azure-restore-files-from-vm.md)