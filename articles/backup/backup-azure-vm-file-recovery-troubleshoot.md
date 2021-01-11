---
title: Solución de problemas de recuperación de archivos de máquina virtual de Azure
description: Solución de problemas al recuperar archivos y carpetas desde una copia de seguridad de máquina virtual de Azure.
ms.topic: troubleshooting
ms.date: 07/12/2020
ms.openlocfilehash: bd369577e320cf6dca510183948f41e6cf91779b
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97605299"
---
# <a name="troubleshooting-issues-in-file-recovery-of-azure-vm-backup"></a>Solución de problemas de recuperación de archivos desde una copia de seguridad de máquina virtual de Azure

En este artículo se proporcionan una serie de pasos que pueden ayudarlo a resolver errores de Azure Backup relacionados con problemas al recuperar archivos y carpetas desde una copia de seguridad de máquina virtual de Azure.

## <a name="common-error-messages"></a>Mensajes comunes de error

### <a name="exception-caught-while-connecting-to-target"></a>Excepción detectada al conectarse al destino

**Causa posible**: El script no puede acceder al punto de recuperación.

**Acción recomendada**: Compruebe si la máquina cumple todos los [requisitos de acceso](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script).

### <a name="the-target-has-already-been-logged-in-via-an-iscsi-session"></a>El destino ya ha iniciado sesión mediante una sesión iSCSI

**Causa posible**: El script ya se ejecutó en la misma máquina y se ha conectado la unidad.

**Acción recomendada**: Ya se han conectado los volúmenes del punto de recuperación. Es posible que no se monten con las mismas letras de unidad de la máquina virtual original. Examine todos los volúmenes disponibles en el explorador de archivos.

### <a name="this-script-is-invalid-because-the-disks-have-been-dismounted-via-portalexceeded-the-12-hr-limit-download-a-new-script-from-the-portal"></a>este script no es válido porque se han desmontado los discos a través del portal o se ha superado el límite de 12 horas. Descargue un nuevo script del portal.

**Causa posible**: Se han desmontado los discos desde el portal o se ha superado el límite de 12 horas.

**Acción recomendada**: El script no es válido una vez que han transcurrido 12 horas desde que se descargó y, por tanto, no se puede ejecutar. Visite el portal y descargue un nuevo script para continuar con la recuperación de archivos.

## <a name="troubleshooting-common-scenarios"></a>Solución de problemas de escenarios comunes

En esta sección se proporcionan los pasos para solucionar problemas que pueden suceder al descargar y ejecutar el script de recuperación de archivos.

### <a name="cant-download-the-script"></a>No se puede descargar el script

Acción recomendada:

1. Asegúrese de que tiene todos los [permisos necesarios para descargar el script](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#select-recovery-point-who-can-generate-script).
1. Compruebe que haya conectividad con las IP de destino de Azure.

Para verificar la conexión, ejecute uno de los siguientes comandos desde un símbolo del sistema con privilegios elevados.

`nslookup download.microsoft.com`

or

`ping download.microsoft.com`

### <a name="the-script-downloads-successfully-but-fails-to-run"></a>El script se descarga correctamente, pero no se puede ejecutar

#### <a name="for-sles-12-sp4-os-linux"></a>Para el sistema operativo SLES 12 SP4 (Linux)

**Error**: iscsi_tcp module not found (no se encuentra el módulo iscsi_tcp)

**Causa posible**: Al ejecutar el script de Python para llevar a cabo la recuperación de nivel de elementos en el sistema operativo SUSE Linux (versión 12sp4), se produce un error **_iscsi_tcp module can’t be loaded_* _ (No se puede cargar el módulo iscsi_tcp).

El módulo ILR usa _ *iscsi_tcp** para establecer una conexión TCP con el servicio de copia de seguridad. En la versión 12SP4, SUSE quitó **iscsi_tcp** del paquete open-iscsi; por lo tanto, se produce un error en la operación ILR.

**Acción recomendada**:  La ejecución del script de recuperación de archivos no se admite en máquinas virtuales SUSE 12SP4. Pruebe la operación de restauración en una versión anterior de SUSE 12SP4.

### <a name="the-script-runs-but-connection-to-iscsi-target-failed"></a>El script se ejecuta, pero no se pudo establecer la conexión con el destino iSCSI

**Error**: Exception caught while connecting to Target (Excepción detectada al conectarse al destino)

1. Asegúrese de que la máquina en la que se ejecuta el script cumple todos los [requisitos de acceso](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-4-access-requirements-to-successfully-run-the-script).
1. Compruebe la conectividad con las IP de destino de Azure.
Para verificar la conexión, ejecute uno de los siguientes comandos desde un símbolo del sistema con privilegios elevados.

   `nslookup download.microsoft.com`

   or

   `ping download.microsoft.com`
1. Asegúrese de que haya acceso al puerto de salida iSCSI 3260.
1. Asegúrese también de que no haya ningún firewall o NSG que esté bloqueando el tráfico a las direcciones URL del servicio de recuperación o a las direcciones IP de destino de Azure.
1. Compruebe si el antivirus está bloqueando la ejecución del script.

### <a name="connected-to-recovery-point-but-disks-didnt-get-attached"></a>La conexión se establece con el punto de recuperación, pero no se conectaron discos

Asegúrese de que dispone de la [máquina correcta para ejecutar el script](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).

#### <a name="on-a-windows-vm"></a>En una máquina virtual de Windows

**El bloque de almacenamiento de la máquina virtual se conecta en modo de solo lectura**:  En Windows 2012 R2 y Windows 2016 (con bloque de almacenamiento), al ejecutar el script por primera vez, es posible que el bloque de almacenamiento conectado a la máquina virtual entre en estado de solo lectura.

Para resolver este problema, es necesario establecer manualmente el acceso de lectura y escritura al bloque de almacenamiento por primera vez y conectar los discos virtuales. Siga estos pasos:

1. Establezca el acceso de lectura y escritura.

   Vaya a **Administrador del servidor** > **Servicios de archivos y almacenamiento** > **Volúmenes** > **Grupos de almacenamiento**.

   ![Almacenamiento de Windows](./media/backup-azure-restore-files-from-vm/windows-storage-1.png)

1. En la ventana **Bloque de almacenamiento**, haga clic con el botón derecho en el bloque de almacenamiento disponible y seleccione la opción **Establecer acceso de lectura y escritura**.

   ![Lectura y escritura en almacenamiento de Windows](./media/backup-azure-restore-files-from-vm/windows-storage-read-write-2.png)

1. Una vez asignado el bloque de almacenamiento con acceso de lectura y escritura, conecte el disco virtual.

   Vaya a la **interfaz de usuario de Administrador del servidor**. En la sección **Discos virtuales**, haga clic con el botón derecho para seleccionar la opción **Exponer disco virtual**.

   ![Disco virtual de Administrador del servidor](./media/backup-azure-restore-files-from-vm/server-manager-virtual-disk-3.png)

#### <a name="on-a-linux-vm"></a>En una máquina virtual Linux

##### <a name="file-recovery-fails-to-auto-mount-because-disk-doesnt-contain-volumes"></a>En la recuperación de archivos, no se puede llevar a cabo la operación de montaje automático porque el disco no contiene volúmenes

Al realizar la recuperación de archivos, el servicio de copia de seguridad detecta los volúmenes y los monta automáticamente. Sin embargo, si los discos de copia de seguridad tienen particiones sin formato, esos discos no se montan automáticamente y no podemos ver el disco de datos para la recuperación.

Para solucionar este problema, siga los pasos que se describen en este [artículo](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms).

##### <a name="the-os-couldnt-identify-the-filesystem-causing-linux-file-recovery-to-fail-while-mountings-disks"></a>El sistema operativo no pudo identificar el sistema de archivos que produce un error en la recuperación de archivos de Linux durante el montaje de discos

Al ejecutar el script de recuperación de archivos, el disco de datos no se pudo conectar debido al siguiente error:

The following partitions failed to mount since the OS couldn't identify the filesystem (No se pudieron montar las siguientes particiones porque el sistema operativo no pudo identificar el sistema de archivos)

Para resolver este problema, compruebe si el volumen está cifrado con una aplicación de otro fabricante. Si está cifrado, el disco o la máquina virtual no se mostrarán como cifrados en el portal.

1. Inicie sesión en la máquina virtual de copia de seguridad y ejecute el siguiente comando:

   `*lsblk -f*`

   ![Disco sin volumen](./media/backup-azure-restore-files-from-vm/disk-without-volume-5.png)

2. Compruebe el sistema de archivos y el cifrado.
3. Si el volumen está cifrado, no se admite la recuperación de archivos. [Obtenga más información](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas#support-for-file-level-restore).

### <a name="disks-are-attached-but-unable-to-mount-volumes"></a>Los discos están conectados, pero no pueden montar volúmenes

- Asegúrese de que dispone de la [máquina correcta para ejecutar el script](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).

#### <a name="on-windows-vms"></a>En máquinas virtuales Windows

Al ejecutar el script de recuperación de archivos para Windows, hay un mensaje que indica ***0 volumes are attached** _ (0 volúmenes conectados), pero los discos se detectan en la consola de administración de discos. Al conectar volúmenes a través de iSCSI, algunos volúmenes que se detectan pasan al estado sin conexión. Cuando el canal iSCSI se comunica entre la máquina virtual y el servicio, detecta estos volúmenes y los pone en línea, pero no se montan.

   ![Disco no conectado](./media/backup-azure-restore-files-from-vm/disk-not-attached-6.png)

Para identificar y resolver este problema, siga estos pasos:

1. Vaya a _ *Administración de discos** ejecutando el comando **diskmgmt** en la ventana cmd.
1. Compruebe si se muestra algún disco adicional. En el ejemplo siguiente, el disco 2 es un disco adicional.

   ![Administración de discos 0](./media/backup-azure-restore-files-from-vm/disk-management-7.png)

1. Haga clic con el botón derecho en **Nuevo volumen** y seleccione **Cambiar la letra y rutas de acceso de unidad**.

   ![Administración de discos 1](./media/backup-azure-restore-files-from-vm/disk-management-8.png)

1. En la ventana **Agregar letra o ruta de acceso de unidad**, seleccione **Asignar la letra de unidad siguiente**, asigne una unidad disponible y seleccione **Aceptar**.

   ![Administración de discos 2](./media/backup-azure-restore-files-from-vm/disk-management-9.png)

1. En el Explorador de archivos, vea la letra de la unidad que eligió y explore los archivos.

#### <a name="on-linux-vms"></a>En máquinas virtuales Linux

- Asegúrese de que dispone de la [máquina correcta para ejecutar el script](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#step-2-ensure-the-machine-meets-the-requirements-before-executing-the-script).
- Si la máquina virtual Linux protegida usa matrices LVM o RAID, siga los pasos indicados en este [artículo](https://docs.microsoft.com/azure/backup/backup-azure-restore-files-from-vm#lvmraid-arrays-for-linux-vms).

### <a name="cant-copy-the-files-from-mounted-volumes"></a>No se pueden copiar los archivos de volúmenes montados

La copia podría producir un error **0x80070780: El sistema no tiene acceso al archivo.** En este caso, compruebe si el servidor de origen tiene habilitada la desduplicación de disco. A continuación, asegúrese de que el servidor de restauración también tenga habilitada la desduplicación en las unidades. Puede dejar el rol de desduplicación sin configurar para no desduplicar las unidades del servidor de restauración.

## <a name="next-steps"></a>Pasos siguientes

- [Recuperación de archivos desde una copia de seguridad de máquina virtual de Azure](backup-azure-restore-files-from-vm.md)
