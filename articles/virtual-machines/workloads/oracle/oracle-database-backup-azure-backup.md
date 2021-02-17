---
title: Copia de seguridad y recuperación de una base de datos de Oracle Database 19c en una máquina virtual Linux de Azure mediante Azure Backup
description: Aprenda a realizar una copia de seguridad y recuperar una base de datos de Oracle Database 19c con el servicio de Azure Backup.
author: cro27
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 01/28/2021
ms.author: cholse
ms.reviewer: dbakevlar
ms.openlocfilehash: ac045694e8975509635e03221a8cb9cc84446b55
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2021
ms.locfileid: "99806416"
---
# <a name="back-up-and-recover-an-oracle-database-19c-database-on-an-azure-linux-vm-using-azure-backup"></a>Copia de seguridad y recuperación de una base de datos de Oracle Database 19c en una máquina virtual Linux de Azure mediante Azure Backup

En este artículo se muestra el uso de Azure Backup para tomar instantáneas de disco de los discos de la máquina virtual, que incluyen los archivos de base de datos y el área de recuperación rápida. Con Azure Backup puede tomar instantáneas completas de disco que pueden usarse como copias de seguridad y se almacenan en un [almacén de Recovery Services](../../../backup/backup-azure-recovery-services-vault-overview.md).  Azure Backup también proporciona copias de seguridad coherentes con las aplicaciones, lo que garantiza que no se van a necesitar correcciones adicionales para restaurar los datos. La restauración de datos coherentes con la aplicación reduce el tiempo de restauración, lo que permite volver rápidamente a un estado de ejecución.

> [!div class="checklist"]
>
> * Creación de copias de seguridad de la base de datos con copias de seguridad coherentes con la aplicación
> * Restauración y recuperación de la base de datos a partir de un punto de recuperación
> * Restauración de la máquina virtual a partir de un punto de recuperación

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../../includes/azure-cli-prepare-your-environment.md)]

Para llevar a cabo el proceso de copia de seguridad y recuperación, primero debe crear una máquina virtual Linux que tenga una instancia de Oracle Database 19c instalada. La imagen de Marketplace que se usa actualmente para crear la máquina virtual es **Oracle:oracle-database-19-3:oracle-database-19-0904:latest**. Siga los pasos de la [guía de inicio rápido para la creación de base de datos Oracle](./oracle-database-quick-create.md) a fin de crear una base de datos Oracle para completar este tutorial.


## <a name="prepare-the-environment"></a>Preparación del entorno

Para preparar el entorno, realice estos pasos:

1. Conéctese a la máquina virtual.
1. Prepare la base de datos.

### <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

1. Para crear una sesión de Secure Shell (SSH) con la máquina virtual, use el siguiente comando. Reemplace la combinación de dirección IP y nombre de host con el valor `<publicIpAddress>` para la máquina virtual.
    
   ```bash
   ssh azureuser@<publicIpAddress>
   ```
   
1. Cambie al usuario *root*:

   ```bash
   sudo su -
   ```
    
1. Agregue el usuario de Oracle al archivo */etc/sudoers*:

   ```bash
   echo "oracle   ALL=(ALL)      NOPASSWD: ALL" >> /etc/sudoers
   ```

### <a name="prepare-the-database"></a>Preparación de la base de datos

En este paso se presupone que tiene una instancia de Oracle (*test*) que se ejecuta en una máquina virtual denominada *vmoracle19c*.

1. Cambie al usuario *oracle*:
 
   ```bash
    sudo su - oracle
    ```
    
2. Antes de conectarse, tiene que definir la variable de entorno ORACLE_SID:
    
    ```bash
    export ORACLE_SID=test;
    ```

    También debe agregar la variable ORACLE_SID al archivo `.bashrc` de usuarios `oracle` para futuros inicios de sesión con el siguiente comando:

    ```bash
    echo "export ORACLE_SID=test" >> ~oracle/.bashrc
    ```
    
3. Inicie el cliente de escucha de Oracle si aún no se está ejecutando:

    ```output
    $ lsnrctl start
    ```

    La salida debería tener un aspecto similar al ejemplo siguiente:

    ```bash
    LSNRCTL for Linux: Version 19.0.0.0.0 - Production on 18-SEP-2020 03:23:49

    Copyright (c) 1991, 2019, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/19.0.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 19.0.0.0.0 - Production
    System parameter file is /u01/app/oracle/product/19.0.0/dbhome_1/network/admin/listener.ora
    Log messages written to /u01/app/oracle/diag/tnslsnr/vmoracle19c/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=vmoracle19c.eastus.cloudapp.azure.com)(PORT=1521)))
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))

    Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=vmoracle19c.eastus.cloudapp.azure.com)(PORT=1521)))
    STATUS of the LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 19.0.0.0.0 - Production
    Start Date                18-SEP-2020 03:23:49
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Parameter File   /u01/app/oracle/product/19.0.0/dbhome_1/network/admin/listener.ora
    Listener Log File         /u01/app/oracle/diag/tnslsnr/vmoracle19c/listener/alert/log.xml
    Listening Endpoints Summary...
     (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=vmoracle19c.eastus.cloudapp.azure.com)(PORT=1521)))
    (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
    The listener supports no services
    The command completed successfully
    ```

4.  Cree la ubicación del área de recuperación rápida (FRA):

    ```bash
    mkdir /u02/fast_recovery_area
    ```

5.  Conéctese a la base de datos:

    ```bash
    SQL> sqlplus / as sysdba
    ```

6.  Inicie la base de datos, si no se está ejecutando todavía:

    ```bash
    SQL> startup
    ```

7.  Establezca las variables de entorno de las bases de datos para el área de recuperación rápida:

    ```bash
    SQL> alter system set db_recovery_file_dest_size=4096M scope=both;
    SQL> alter system set db_recovery_file_dest='/u02/fast_recovery_area' scope=both;
    ```
    
8.  Asegúrese de que la base de datos está en modo de registro de archivo para habilitar las copias de seguridad en línea.

    Compruebe primero el estado del archivo de registro:

    ```bash
    SQL> SELECT log_mode FROM v$database;

    LOG_MODE
    ------------
    NOARCHIVELOG
    ```

    Si está en modo NOARCHIVELOG, ejecute los siguientes comandos:

    ```bash
    SQL> SHUTDOWN IMMEDIATE;
    SQL> STARTUP MOUNT;
    SQL> ALTER DATABASE ARCHIVELOG;
    SQL> ALTER DATABASE OPEN;
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```

9.  Cree una tabla para probar las operaciones de copia de seguridad y restauración:

    ```bash
    SQL> create user scott identified by tiger quota 100M on users;
    SQL> grant create session, create table to scott;
    connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    SQL> insert into scott_table VALUES(1,'Line 1');
    SQL> commit;
    SQL> quit
    ```

10. Configure RMAN para que realice copias de seguridad en el área de recuperación rápida que se encuentra en el disco de la máquina virtual:

    ```bash
    $ rman target /
    RMAN> configure snapshot controlfile name to '/u02/fast_recovery_area/snapcf_ev.f';
    RMAN> configure channel 1 device type disk format '/u02/fast_recovery_area/%d/Full_%d_%U_%T_%s';
    RMAN> configure channel 2 device type disk format '/u02/fast_recovery_area/%d/Full_%d_%U_%T_%s'; 
    ```

11. Confirme los detalles de las modificaciones de la configuración:

    ```bash
    RMAN> show all;
    ```    

12.  Ahora, ejecute la copia de seguridad. El siguiente comando tomará una copia de seguridad completa de la base de datos, incluidos los archivos de registro de archivo, como conjunto de copias de seguridad en formato comprimido:

     ```bash
     RMAN> backup as compressed backupset database plus archivelog;
     ```

## <a name="using-azure-backup"></a>Uso de Azure Backup

El servicio Azure Backup proporciona soluciones sencillas, seguras y rentables tanto para realizar copias de seguridad de datos de la nube de Microsoft Azure como para recuperarlos. Azure Backup proporciona copias de seguridad independientes y aisladas para evitar la destrucción accidental de datos originales. Las copias de seguridad se almacenan en un almacén de Recovery Services con administración integrada de puntos de recuperación. La configuración y la escalabilidad son sencillas, las copias de seguridad están optimizadas y puede restaurarlas fácilmente cuando sea necesario.

El servicio de Azure Backup proporciona un [marco](../../../backup/backup-azure-linux-app-consistent.md) para lograr la coherencia de la aplicación durante las copias de seguridad de las máquinas virtuales Windows y Linux para diversas aplicaciones, como Oracle, MySQL, Mongo DB, SAP HANA y PostgreSQL. Esto implica invocar un script previo (para poner en modo inactivo las aplicaciones) antes de tomar una instantánea de los discos y llamar un script posterior (comandos para liberar las aplicaciones) una vez completada la instantánea, de modo que las aplicaciones vuelven al modo normal. Aunque que los scripts previos y posteriores de ejemplo se proporcionan en GitHub, la creación y mantenimiento de los mismos es su responsabilidad. 

Ahora Azure Backup proporciona un marco de scripts previos y posteriores mejorado, de modo que el servicio de Azure Backup proporcionará scripts previos y posteriores empaquetados para aplicaciones específicas. Los usuarios de Azure Backup solo tienen que asignar un nombre a la aplicación y, a continuación, la copia de seguridad de la máquina virtual de Azure invocará automáticamente los scripts previos/posteriores correspondientes. El equipo de Azure Backup dará mantenimiento a los scripts previos y posteriores empaquetados, por lo que los usuarios podrán estar seguros de la compatibilidad, propiedad y validez de estos scripts. Actualmente, las aplicaciones compatibles con el marco mejorado son *Oracle* y *MySQL*.

En esta sección, usará el marco mejorado de Azure Backup para tomar instantáneas coherentes con la aplicación de la máquina virtual en ejecución y la base de datos de Oracle. La base de datos se pondrá en modo de copia de seguridad, lo que permitirá que se produzca una copia de seguridad en línea coherente con las transacciones mientras Azure Backup toma una instantánea de los discos de máquina virtual. La instantánea será una copia completa del almacenamiento y no una instantánea incremental o de copia por escritura, por lo que es un medio eficaz para restaurar la base de datos. La ventaja de usar instantáneas de Azure Backup coherentes con la aplicación es que son muy rápidas de tomar independientemente del tamaño de la base de datos, y una instantánea se puede usar para las operaciones de restauración tan pronto como se toma, sin tener que esperar a que se transfiera al almacén de Recovery Services.

Para usar Azure Backup para crear una copia de seguridad de la base de datos, siga estos pasos:

1. Preparación del entorno para una copia de seguridad coherente con la aplicación.
1. Configuración de las copias de seguridad coherentes con la aplicación.
1. Desencadenamiento de una copia de seguridad coherente con la aplicación de la máquina virtual.

### <a name="prepare-the-environment-for-an-application-consistent-backup"></a>Preparación del entorno para una copia de seguridad coherente con la aplicación

1. Cambie al usuario *root*:

   ```bash
   sudo su -
   ```

1. Cree un nuevo usuario de copia de seguridad:

   ```bash
   useradd -G backupdba azbackup
   ```
   
2. Configure el entorno del usuario de copia de seguridad:

   ```bash
   echo "export ORACLE_SID=test" >> ~azbackup/.bashrc
   echo export ORACLE_HOME=/u01/app/oracle/product/19.0.0/dbhome_1 >> ~azbackup/.bashrc
   echo export PATH='$ORACLE_HOME'/bin:'$PATH' >> ~azbackup/.bashrc
   ```
   
3. Configure la autenticación externa para el nuevo usuario de copia de seguridad. El usuario de copia de seguridad debe tener acceso a la base de datos mediante la autenticación externa, para que no reciba un desafío de contraseña.

   En primer lugar, cambie de vuelta al usuario *oracle*:

   ```bash
   su - oracle
   ```

   Inicie sesión en la base de datos con sqlplus y compruebe la configuración predeterminada de la autenticación externa:
   
   ```bash
   sqlplus / as sysdba
   SQL> show parameter os_authent_prefix
   SQL> show parameter remote_os_authent
   ```
   
   El resultado debe ser similar a este ejemplo: 

   ```output
   NAME                                 TYPE        VALUE
   ------------------------------------ ----------- ------------------------------
   os_authent_prefix                    string      ops$
   remote_os_authent                    boolean     FALSE
   ```

   Ahora, cree un usuario de base de datos *azbackup* autenticado externamente y concédale el privilegio sysbackup:
   
   ```bash
   SQL> CREATE USER ops$azbackup IDENTIFIED EXTERNALLY;
   SQL> GRANT CREATE SESSION, ALTER SESSION, SYSBACKUP TO ops$azbackup;
   ```

   > [!IMPORTANT] 
   > Si recibe el error `ORA-46953: The password file is not in the 12.2 format.` al ejecutar la instrucción `GRANT`, siga estos pasos para migrar el archivo orapwd al formato 12.2:
   >
   > 1. Salga de sqlplus.
   > 1. Mueva el archivo de contraseña con el formato antiguo a un nuevo nombre.
   > 1. Migre el archivo de contraseña.
   > 1. Quite el archivo antiguo.
   > 1. Ejecute el siguiente comando:
   >
   >    ```bash
   >    mv $ORACLE_HOME/dbs/orapwtest $ORACLE_HOME/dbs/orapwtest.tmp
   >    orapwd file=$ORACLE_HOME/dbs/orapwtest input_file=$ORACLE_HOME/dbs/orapwtest.tmp
   >    rm $ORACLE_HOME/dbs/orapwtest.tmp
   >    ```
   >
   > 1. Vuelva a ejecutar la operación `GRANT` en sqlplus.
   >
   
4. Cree un procedimiento almacenado para registrar los mensajes de la copia de seguridad en el registro de alertas de la base de datos:

   ```bash
   sqlplus / as sysdba
   SQL> GRANT EXECUTE ON DBMS_SYSTEM TO SYSBACKUP;
   SQL> CREATE PROCEDURE sysbackup.azmessage(in_msg IN VARCHAR2)
   AS
     v_timestamp     VARCHAR2(32);
   BEGIN
     SELECT TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS')
     INTO v_timestamp FROM DUAL;
     DBMS_OUTPUT.PUT_LINE(v_timestamp || ' - ' || in_msg);
     SYS.DBMS_SYSTEM.KSDWRT(SYS.DBMS_SYSTEM.ALERT_FILE, in_msg);
   END azmessage;
   /
   SQL> SHOW ERRORS
   SQL> QUIT
   ```
   
### <a name="set-up-application-consistent-backups"></a>Configuración de las copias de seguridad coherentes con la aplicación  

1. Cambie al usuario *root*:

   ```bash
   sudo su -
   ```

2. Cree el directorio de trabajo de copia de seguridad coherente con la aplicación:

   ```bash
   if [ ! -d "/etc/azure" ]; then
      sudo mkdir /etc/azure
   fi
   ```

3. Cree un archivo en el directorio */etc/azure* denominado *workload.conf* con el siguiente contenido, que debe comenzar con `[workload]`. El siguiente comando creará el archivo y rellenará el contenido:

   ```bash
   echo "[workload]
   workload_name = oracle
   command_path = /u01/app/oracle/product/19.0.0/dbhome_1/bin/
   timeout = 90
   linux_user = azbackup" > /etc/azure/workload.conf
   ```

4. Descargue los scripts preOracleMaster.sql y postOracleMaster.sql del [repositorio de GitHub](https://github.com/Azure/azure-linux-extensions/tree/master/VMBackup/main/workloadPatch/DefaultScripts) y cópielos en el directorio */etc/azure*.

5. Cambie los permisos del archivo.

```bash
   chmod 744 workload.conf preOracleMaster.sql postOracleMaster.sql 
   ```

### <a name="trigger-an-application-consistent-backup-of-the-vm"></a>Desencadenamiento de una copia de seguridad coherente con la aplicación de la máquina virtual

# <a name="portal"></a>[Portal](#tab/azure-portal)

1.  En Azure Portal, vaya a su grupo de recursos **rg-oracle** y haga clic en la máquina virtual **vmoracle19c**.

2.  En la hoja **Copia de seguridad**, cree un nuevo **almacén de Recovery Services** en el grupo de recursos **rg-oracle** con el nombre **myVault**.
    En **Elegir directiva de copia de seguridad**, use **(nuevo) DailyPolicy**. Si quiere cambiar la frecuencia de la copia de seguridad o el período de retención, seleccione **Crear una nueva directiva** en su lugar.

    ![Página para agregar almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-01.png)

3.  Para continuar, haga clic en **Habilitar copia de seguridad**.

    > [!IMPORTANT]
    > Tras hacer clic en **Enable backup** (Habilitar copia de seguridad), el proceso no se inicia hasta que expira el tiempo programado. Para configurar la copia de seguridad inmediata, complete el paso siguiente.

4. En la página del grupo de recursos, haga clic en el almacén de Recovery Services recién creado, **myVault**. Sugerencia: Es posible que deba actualizar la página para que aparezca.

5.  En la hoja **myVault - elementos de copia de seguridad**, en **Recuento de elementos de copia de seguridad**, seleccione el número de elementos de copia de seguridad.

    ![Página de detalles del almacén myVault de Recovery Services](./media/oracle-backup-recovery/recovery-service-02.png)

6.  En la hoja **Elementos de copia de seguridad (Azure Virtual Machine)** , en el lado derecho de la página, haga clic en el botón de puntos suspensivos ( **...** ) y después en **Realizar copia de seguridad ahora**.

    ![Comando Realizar copia de seguridad ahora de los almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-03.png)

7.  Acepte el valor predeterminado de Conservar la copia de seguridad hasta y haga clic en el botón **Aceptar**. Espere a que finalice el proceso de copia de seguridad. 

    Para ver el estado del trabajo de copia de seguridad, haga clic en **Trabajos de copia de seguridad**.

    ![Página del trabajo de los almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-04.png)

    El estado del trabajo de copia de seguridad aparece en la siguiente imagen:

    ![Página del trabajo de los almacenes de Recovery Services con el estado](./media/oracle-backup-recovery/recovery-service-05.png)
    
    Tenga en cuenta que, aunque solo se necesitan unos segundos para ejecutar la instantánea, puede tardar algún tiempo en transferirse al almacén y el trabajo de copia de seguridad no se completará hasta que finaliza la transferencia.

8. Para una copia de seguridad coherente con la aplicación, resuelva los errores en el archivo de registro. El archivo de registro se encuentra en /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/extension.log.

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Creación y configuración de un almacén de Recovery Services:

   ```azurecli
   az backup vault create --location eastus --name myVault --resource-group rg-oracle
   ```

2. Habilite la protección de copias de seguridad de la máquina virtual:

   ```azurecli
   az backup protection enable-for-vm \
      --resource-group rg-oracle \
      --vault-name myVault \
      --vm vmoracle19c \
      --policy-name DefaultPolicy
   ```

3. Desencadene una copia de seguridad para que se ejecute ahora en lugar de esperar a que la copia de seguridad se desencadene según la programación predeterminada (5 A. M. UTC): 

   ```azurecli
   az backup protection backup-now \
      --resource-group rg-oracle \
      --vault-name myVault \
      --backup-management-type AzureIaasVM \
      --container-name vmoracle19c \
      --item-name vmoracle19c 
   ```

   Puede supervisar el progreso del trabajo de copia de seguridad mediante `az backup job list` y `az backup job show`.

---

## <a name="recovery"></a>Recuperación

Para recuperar la base de datos, complete estos pasos:

1. Eliminación de los archivos de la base de datos.
1. Generación de un script de restauración desde el almacén de Recovery Services.
1. Montaje del punto de restauración.
1. Realización de la recuperación.

### <a name="remove-the-database-files"></a>Eliminación de los archivos de la base de datos 

Más adelante en este artículo obtendrá información sobre cómo probar el proceso de recuperación. Para poder probarlo, tiene que eliminar los archivos de la base de datos.

1.  Cierre la instancia de Oracle:

    ```bash
    sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

2.  Quite los archivos de datos y las copias de seguridad:

    ```bash
    sudo su - oracle
    cd /u02/oradata/TEST
    rm -f *.dbf
    cd /u02/fast_recovery_area
    rm -f *
    ```

### <a name="generate-a-restore-script-from-the-recovery-services-vault"></a>Genere un script de restauración desde el almacén de Recovery Services.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En Azure Portal, busque el almacén *myVault* de Recovery Services y selecciónelo.

    ![Elementos de copia de seguridad del almacén myVault de Recovery Services](./media/oracle-backup-recovery/recovery-service-06.png)

2. En la hoja **Información general**, seleccione **Elementos de copia de seguridad** y seleccione **Máquina virtual de Azure**, que debe tener un número de elementos de copia de seguridad distinto de cero.

    ![Número de elementos de copia de seguridad de máquinas virtuales de Azure en los almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-07.png)

3. En la página Elementos de copia de seguridad (Azure Virtual Machines), se muestra la máquina virtual **vmoracle19c**. Haga clic en los puntos suspensivos de la derecha para abrir el menú y seleccione **Recuperación de archivos**.

    ![Captura de pantalla de la página de recuperación de archivos de los almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-08.png)

4. En el panel **Recuperación de archivos (versión preliminar)** , haga clic en **Descargar script**. Después, guarde el archivo de descarga (.py) en una carpeta en el equipo cliente. Se genera una contraseña para ejecutar el script. Copie la contraseña en un archivo para uso posterior. 

    ![Operaciones de guardado del archivo de script descargado](./media/oracle-backup-recovery/recovery-service-09.png)

5. Copie el archivo .py en la máquina virtual.

    En el ejemplo siguiente se muestra cómo usar un comando de copia de seguridad (scp) para mover el archivo a la máquina virtual. También puede copiar el contenido en el portapapeles y pegarlo en un nuevo archivo que se haya configurado en la máquina virtual.

    > [!IMPORTANT]
    > En el ejemplo siguiente, asegúrese de actualizar los valores de dirección IP y carpeta. Los valores deben corresponder a la carpeta donde se guarda el archivo.
    >

    ```bash
    $ scp vmoracle19c_xxxxxx_xxxxxx_xxxxxx.py azureuser@<publicIpAddress>:/tmp
    ```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para enumerar los puntos de recuperación de la máquina virtual, use az backup recovery point list. En este ejemplo, se selecciona el punto de recuperación más reciente de la máquina virtual denominada myVM que está protegida en myRecoveryServicesVault:

```azurecli
   az backup recoverypoint list \
      --resource-group rg-oracle \
      --vault-name myVault \
      --backup-management-type AzureIaasVM \
      --container-name vmoracle19c \
      --item-name vmoracle19c \
      --query [0].name \
      --output tsv
```

Para obtener el script que conecta el punto de recuperación a la máquina virtual o lo monta en esta, use az backup restore files mount-rp. En el ejemplo siguiente se obtiene el script de la máquina virtual denominada myVM que está protegida en myRecoveryServicesVault.

Reemplace myRecoveryPointName por el nombre del punto de recuperación que obtuvo en el comando anterior:

```azurecli
   az backup restore files mount-rp \
      --resource-group rg-oracle \
      --vault-name myVault \
      --container-name vmoracle19c \
      --item-name vmoracle19c \
      --rp-name myRecoveryPointName
```

El script se descarga y se muestra una contraseña, como en el ejemplo siguiente:

```bash
   File downloaded: vmoracle19c_eus_4598131610710119312_456133188157_6931c635931f402eb543ee554e1cf06f102c6fc513d933.py. Use password c4487e40c760d29
```

Copie el archivo .py en la máquina virtual.

En el ejemplo siguiente se muestra cómo usar un comando de copia de seguridad (scp) para mover el archivo a la máquina virtual. También puede copiar el contenido en el portapapeles y pegarlo en un nuevo archivo que se haya configurado en la máquina virtual.

> [!IMPORTANT]
> En el ejemplo siguiente, asegúrese de actualizar los valores de dirección IP y carpeta. Los valores deben corresponder a la carpeta donde se guarda el archivo.
>

```bash
$ scp vmoracle19c_xxxxxx_xxxxxx_xxxxxx.py azureuser@<publicIpAddress>:/tmp
```
---

### <a name="mount-the-restore-point"></a>Montaje del punto de restauración

1. Cree un punto de montaje de restauración y copie el script en él.

    En el ejemplo siguiente, cree un directorio */restore* para el montaje de la instantánea, mueva el archivo al directorio y cambie el archivo para que sea propiedad del usuario raíz y sea ejecutable.

    ```bash 
    ssh azureuser@<publicIpAddress>
    sudo su -
    mkdir /restore
    chmod 777 /restore
    cd /restore
    cp /tmp/vmoracle19c_xxxxxx_xxxxxx_xxxxxx.py /restore
    chmod 755 /restore/vmoracle19c_xxxxxx_xxxxxx_xxxxxx.py
    ```
    
    Ahora ejecute el script para restaurar la copia de seguridad. Se le pedirá que proporcione la contraseña generada en Azure Portal. 
  
   ```bash
    ./vmoracle19c_xxxxxx_xxxxxx_xxxxxx.py
    ```

    En el ejemplo siguiente se muestra lo que verá después de ejecutar el script anterior. Cuando se le pregunte si desea continuar, escriba **Y**.

    ```output
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : b1ad68e16dfafc6

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sdc  |  /dev/sdc1  |  /restore/vmoracle19c-20201215123912/Volume1

    2)  | /dev/sdd  |  /dev/sdd1  |  /restore/vmoracle19c-20201215123912/Volume2

    3)  | /dev/sdd  |  /dev/sdd2  |  /restore/vmoracle19c-20201215123912/Volume3

    4)  | /dev/sdd  |  /dev/sdd15  |  /restore/vmoracle19c-20201215123912/Volume5

    The following partitions failed to mount since the OS couldn't identify the filesystem.

    ************ Volumes from unknown filesystem ************

    Sr.No.  |  Disk  |  Volume  |  Partition Type

    1)  | /dev/sdb  |  /dev/sdb14  |  BIOS Boot partition

    Please refer to '/restore/vmoracle19c-2020XXXXXXXXXX/Scripts/MicrosoftAzureBackupILRLogFile.log' for more details.

    ************ Open File Explorer to browse for files. ************

    After recovery, remove the disks and close the connection to the recovery point by clicking the 'Unmount Disks' button from the portal or by using the relevant unmount command in case of powershell or CLI.

    After unmounting disks, run the script with the parameter 'clean' to remove the mount paths of the recovery point from this machine.

    Please enter 'q/Q' to exit...
    ```

2. Se confirmará el acceso a los volúmenes montados.

    Para salir, escriba **q** y busque los volúmenes montados. Para crear una lista de los volúmenes agregados, en un símbolo del sistema, escriba **df -h**.
    
    ```
    [root@vmoracle19c restore]# df -h
    Filesystem      Size  Used Avail Use% Mounted on
    devtmpfs        3.8G     0  3.8G   0% /dev
    tmpfs           3.8G     0  3.8G   0% /dev/shm
    tmpfs           3.8G   17M  3.8G   1% /run
    tmpfs           3.8G     0  3.8G   0% /sys/fs/cgroup
    /dev/sdd2        30G  9.6G   18G  36% /
    /dev/sdb1       126G  736M  119G   1% /u02
    /dev/sda1       497M  199M  298M  41% /boot
    /dev/sda15      495M  9.7M  486M   2% /boot/efi
    tmpfs           771M     0  771M   0% /run/user/54322
    /dev/sdc1       126G  2.9G  117G   3% /restore/vmoracle19c-20201215123912/Volume1
    /dev/sdd1       497M  199M  298M  41% /restore/vmoracle19c-20201215123912/Volume2
    /dev/sdd2        30G  9.6G   18G  36% /restore/vmoracle19c-20201215123912/Volume3
    /dev/sdd15      495M  9.7M  486M   2% /restore/vmoracle19c-20201215123912/Volume5
    ```

### <a name="perform-recovery"></a>Realizar la recuperación

1. Vuelva a copiar los archivos de copia de seguridad que faltan en el área de recuperación rápida:

    ```bash
    cd /restore/vmoracle19c-2020XXXXXXXXXX/Volume1/fast_recovery_area/TEST
    cp * /u02/fast_recovery_area/TEST
    cd /u02/fast_recovery_area/TEST
    chown -R oracle:oinstall *
    ```

2. Los siguientes comandos usan RMAN para restaurar los archivos de datos que faltan y recuperar la base de datos:

    ```bash
    sudo su - oracle
    rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open;
    ```
    
3. Compruebe que el contenido de la base de datos se haya recuperado completamente:

    ```bash
    RMAN> SELECT * FROM scott.scott_table;
    ```

4. Desmonte el punto de restauración.

   ```bash
   umount /restore/vmoracle19c-20210107110037/Volume*
   ```

    En la hoja **Recuperación de archivos (versión preliminar)** de Azure Portal, haga clic en **Desmontar discos**.

    ![Comando Unmount Disks (Desmontar discos)](./media/oracle-backup-recovery/recovery-service-10.png)
    
    También puede desmontar los volúmenes de recuperación si ejecuta de nuevo el script de Python con la opción **-clean**.

## <a name="restore-the-entire-vm"></a>Restauración de toda la máquina virtual

En lugar de restaurar los archivos eliminados de los almacenes de Recovery Services, puede restaurar toda la máquina virtual.

Para restaurar toda la máquina virtual, complete estos pasos:

1. Detenga y elimine vmoracle19c.
1. Recupere la máquina virtual.
1. Establezca la dirección IP pública.
1. Conéctese a la máquina virtual.
1. Inicie la base de datos para la fase de montaje y realice la recuperación.

### <a name="stop-and-delete-vmoracle19c"></a>Detención y eliminación de vmoracle19c

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. En Azure Portal, vaya a la máquina virtual **vmoracle19c** y, a continuación, seleccione **Detener**.

1. Cuando la máquina virtual deje de ejecutarse, seleccione **Eliminar** y, a continuación, **Sí**.

   ![Comando Eliminar almacén](./media/oracle-backup-recovery/recover-vm-01.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Detenga y desasigne la máquina virtual:

    ```azurecli
    az vm deallocate --resource-group rg-oracle --name vmoracle19c
    ```

2. Elimine la máquina virtual. Cuando se le solicite, escriba "y" (sí):

    ```azurecli
    az vm delete --resource-group rg-oracle --name vmoracle19c
    ```

---

### <a name="recover-the-vm"></a>Recuperación de la máquina virtual

# <a name="portal"></a>[Portal](#tab/azure-portal)

1. Cree una cuenta de almacenamiento para almacenamiento provisional en Azure Portal.

   1. En Azure Portal, seleccione **+ Crear un recurso** y busque y seleccione **Cuenta de almacenamiento**.
    
      ![Captura de pantalla que muestra dónde se crean los recursos.](./media/oracle-backup-recovery/storage-1.png)
    
    
   1. En la página Crear cuenta de almacenamiento, elija el grupo de recursos existente **rg-oracle**, asigne el nombre **oracrestore** a la cuenta de almacenamiento y elija **Storage V2 (generalpurpose V2)** para el tipo de cuenta. Cambie la Replicación a **Almacenamiento con redundancia local (LRS)** y establezca el Rendimiento en **Estándar**. Asegúrese de que la ubicación está establecida en la misma región que todos los demás recursos del grupo. 
    
      ![Página de adición de cuenta de almacenamiento](./media/oracle-backup-recovery/recovery-storage-1.png)
   
   1. Haga clic en Revisar y crear y, a continuación, en Crear.

2. En Azure Portal, busque el almacén *myVault* de Recovery Services y haga clic en él.

    ![Elementos de copia de seguridad del almacén myVault de Recovery Services](./media/oracle-backup-recovery/recovery-service-06.png)
    
3.  En la hoja **Información general**, seleccione **Elementos de copia de seguridad** y seleccione **Máquina virtual de Azure**, que debe tener un número de elementos de copia de seguridad distinto de cero.

    ![Número de elementos de copia de seguridad de máquinas virtuales de Azure en los almacenes de Recovery Services](./media/oracle-backup-recovery/recovery-service-07.png)

4.  En la página Elementos de copia de seguridad (Azure Virtual Machines), se muestra la máquina virtual **vmoracle19c**. Haga clic en el nombre de la máquina virtual.

    ![Página de la máquina virtual de recuperación](./media/oracle-backup-recovery/recover-vm-02.png)

5.  En la hoja **vmoracle19c**, elija un punto de restauración que tenga un tipo de coherencia **Coherente con la aplicación** y haga clic en el botón de puntos suspensivos ( **...** ) de la derecha para abrir el menú.  En el menú, haga clic en **Restaurar VM**.

    ![Comando Restaurar máquina virtual](./media/oracle-backup-recovery/recover-vm-03.png)

6.  En la hoja **Restaurar máquina virtual**, elija **Crear nuevo** y **Crear nueva máquina virtual**. Escriba el nombre de la máquina virtual **vmoracle19c** y elija la red virtual **vmoracle19cVNET**. La subred se rellenará automáticamente en función de la red virtual que seleccione. El proceso de restauración de VM requiere una cuenta de almacenamiento de Azure en el mismo grupo de recursos y región. Puede elegir la cuenta de almacenamiento **orarestore** que configuró antes.

    ![Valores de configuración de la restauración](./media/oracle-backup-recovery/recover-vm-04.png)

7.  Para restaurar la máquina virtual, haga clic en el botón **Restaurar**.

8.  Para ver el estado del proceso de restauración, haga clic en **Trabajos** y después en **Trabajos de copia de seguridad**.

    ![Comando de estado de los trabajos de copia de seguridad](./media/oracle-backup-recovery/recover-vm-05.png)

    Haga clic en la operación de restauración **En curso** para mostrar el estado del proceso de restauración:

    ![Estado del proceso de restauración](./media/oracle-backup-recovery/recover-vm-06.png)

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

Para configurar la cuenta de almacenamiento y el recurso compartido de archivos, ejecute los siguientes comandos en la CLI de Azure.

1. Cree la cuenta de almacenamiento en el mismo grupo de recursos y ubicación que la máquina virtual:

   ```azurecli
   az storage account create -n orarestore -g rg-oracle -l eastus --sku Standard_LRS
   ```

2. Recupere la lista de puntos de recuperación disponibles. 

   ```azurecli
   az backup recoverypoint list \
      --resource-group rg-oracle \
      --vault-name myVault \
      --backup-management-type AzureIaasVM \
      --container-name vmoracle19c \
      --item-name vmoracle19c \
      --query [0].name \
      --output tsv
   ```

3. Restaure el punto de recuperación en la cuenta de almacenamiento. Sustituya `<myRecoveryPointName>` por un punto de recuperación de la lista que se generó en el paso anterior:

   ```azurecli
   az backup restore restore-disks \
      --resource-group rg-oracle \
      --vault-name myVault \
      --container-name vmoracle19c \
      --item-name vmoracle19c \
      --storage-account orarestore \
      --rp-name <myRecoveryPointName> \
      --target-resource-group rg-oracle
   ```

4. Recupere los detalles del trabajo de restauración. El siguiente comando obtiene más detalles sobre el trabajo restaurado desencadenado, incluido su nombre, que se necesita para recuperar el URI de la plantilla. 

   ```azurecli
   az backup job list \
       --resource-group rg-oracle \
       --vault-name myVault \
       --output table
   ```

   El resultado será similar al siguiente `(Note down the name of the restore job)`:

   ```output
   Name                                  Operation        Status     Item Name    Start Time UTC                    Duration
   ------------------------------------  ---------------  ---------  -----------  --------------------------------  --------------
   c009747a-0d2e-4ac9-9632-f695bf874693  Restore          Completed  vmoracle19c  2021-01-10T21:46:07.506223+00:00  0:03:06.634177
   6b779c98-f57a-4db1-b829-9e8eab454a52  Backup           Completed  vmoracle19c  2021-01-07T10:11:15.784531+00:00  0:21:13.220616
   502bc7ae-d429-4f0f-b78e-51d41b7582fc  ConfigureBackup  Completed  vmoracle19c  2021-01-07T09:43:55.298755+00:00  0:00:30.839674
   ```

5. Recupere los detalles del URI que se usará para volver a crear la máquina virtual. Sustituya el nombre del trabajo de restauración del paso anterior por `<RestoreJobName>`.

    ```azurecli
      az backup job show \
        -v myVault \
        -g rg-oracle \
        -n <RestoreJobName> \
        --query properties.extendedInfo.propertyBag
    ```

   La salida es parecida a esta:

   ```output
   {
   "Config Blob Container Name": "vmoracle19c-75aefd4b34c64dd39fdcd3db579783f2",
   "Config Blob Name": "config-vmoracle19c-c009747a-0d2e-4ac9-9632-f695bf874693.json",
   "Config Blob Uri": "https://orarestore.blob.core.windows.net/vmoracle19c-75aefd4b34c64dd39fdcd3db579783f2/config-vmoracle19c-c009747a-0d2e-4ac9-9632-f695bf874693.json",
   "Job Type": "Recover disks",
   "Recovery point time ": "1/7/2021 10:11:19 AM",
   "Target Storage Account Name": "orarestore",
   "Target resource group": "rg-oracle",
   "Template Blob Uri": "https://orarestore.blob.core.windows.net/vmoracle19c-75aefd4b34c64dd39fdcd3db579783f2/azuredeployc009747a-0d2e-4ac9-9632-f695bf874693.json"
   }
   ```

   Se muestra el nombre de la plantilla, que se encuentra al final del URI del blob de la plantilla, que en este ejemplo es `azuredeployc009747a-0d2e-4ac9-9632-f695bf874693.json`, y el nombre del contenedor de blobs, que es `vmoracle19c-75aefd4b34c64dd39fdcd3db579783f2`. 

   Use estos valores en el siguiente comando para asignar variables como preparación para la creación de la máquina virtual. Se genera una clave SAS para el contenedor de almacenamiento con una duración de 30 minutos.  


   ```azurecli
   expiretime=$(date -u -d "30 minutes" '+%Y-%m-%dT%H:%MZ')
   connection=$(az storage account show-connection-string \
    --resource-group rg-oracle \
    --name orarestore \
    --query connectionString)
   token=$(az storage blob generate-sas \
    --container-name <ContainerName> \
    --name <TemplateName> \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
   url=$(az storage blob url \
    --container-name <ContainerName> \
    --name <TemplateName> \
    --connection-string $connection \
    --output tsv)
   ```

   Ahora, implemente la plantilla para crear la máquina virtual.

   ```azurecli
   az deployment group create \
      --resource-group rg-oracle \
      --template-uri $url?$token
   ```

   Se le solicitará que proporcione un nombre para la máquina virtual.

---

### <a name="set-the-public-ip-address"></a>Establecimiento de la dirección IP pública

Una vez restaurada la máquina virtual, debe reasignar la dirección IP original a la nueva máquina virtual.

# <a name="portal"></a>[Portal](#tab/azure-portal)

1.  En Azure Portal, vaya a la máquina virtual **vmoracle19c**. Observará que se le ha asignado una nueva dirección IP pública y un adaptador de red similar a vmoracle19c-NIC-XXXXXXXXXXXX, pero no tiene una dirección DNS. Cuando se elimina la máquina virtual original, se conservan su adaptador de red e IP pública y se vuelven a asociar a la nueva máquina virtual en los pasos siguientes.

    ![Lista de direcciones IP públicas](./media/oracle-backup-recovery/create-ip-01.png)

2.  Parada de la máquina virtual

    ![Creación de la dirección IP](./media/oracle-backup-recovery/create-ip-02.png)

3.  Vaya a **Redes**.

    ![Asociación de la dirección IP](./media/oracle-backup-recovery/create-ip-03.png)

4.  Haga clic en **Asociar interfaz de red**, elija el adaptador de red original **vmoracle19cVMNic, al que sigue asociada la dirección IP pública original y haga clic en **Aceptar**.

    ![Selección del tipo de recurso y los valores de NIC](./media/oracle-backup-recovery/create-ip-04.png)

5.  Ahora debe desasociar el adaptador de red que se creó con la operación de restauración de la máquina virtual, ya que está configurado como interfaz principal. Haga clic en **Desasociar interfaz de red** y elija el nuevo adaptador de red similar a **vmoracle19c-NIC-XXXXXXXXXXXX** y, a continuación, haga clic en **Aceptar**.

    ![Captura de pantalla que muestra dónde se selecciona Desasociar interfaz de red.](./media/oracle-backup-recovery/create-ip-05.png)
    
    La máquina virtual recreada tendrá ahora el adaptador de red original, que está asociado con la dirección IP y las reglas de grupo de seguridad de red originales.
    
    ![Valor de la dirección IP](./media/oracle-backup-recovery/create-ip-06.png)
    
6.  Vuelva a **Información general** y haga clic en **Iniciar**. 

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

1. Detenga y desasigne la máquina virtual:

   ```azurecli
   az vm deallocate --resource-group rg-oracle --name vmoracle19c
   ```

2. Enumere el adaptador de red actual de la máquina virtual generado en la restauración.

   ```azurecli
   az vm nic list --resource-group rg-oracle --vm-name vmoracle19c
   ```

   La salida tendrá un aspecto similar al siguiente, que muestra el nombre del adaptador de red generado por la restauración como `vmoracle19cRestoredNICc2e8a8a4fc3f47259719d5523cd32dcf`.

   ```output
   {
    "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx/resourceGroups/rg-oracle/providers/Microsoft.Network/networkInterfaces/vmoracle19cRestoredNICc2e8a8a4fc3f47259719d5523cd32dcf",
    "primary": true,
    "resourceGroup": "rg-oracle"
   }
   ```

3. Asocie el adaptador de red original, que debe tener un nombre con el formato `<VMName>VMNic`; en este caso, `vmoracle19cVMNic`. La dirección IP pública original sigue conectada a este adaptador de red y se restaurará en la máquina virtual cuando se vuelva a asociar el adaptador de red. 

   ```azurecli
   az vm nic add --nics vmoracle19cVMNic --resource-group rg-oracle --vm-name vmoracle19c
   ```

4. Desasociación de adaptador de red generado por la restauración

   ```azurecli
   az vm nic remove --nics vmoracle19cRestoredNICc2e8a8a4fc3f47259719d5523cd32dcf --resource-group rg-oracle --vm-name vmoracle19c
   ```

5. Inicie la máquina virtual:

   ```azurecli
   az vm start --resource-group rg-oracle --name vmoracle19c
   ```

---

### <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

Para conectarse a la máquina virtual, use el script siguiente:

```azurecli
ssh <publicIpAddress>
```

### <a name="start-the-database-to-mount-stage-and-perform-recovery"></a>Inicio de la base de datos para la fase de montaje y realización de la recuperación

1. Es posible que la instancia se esté ejecutando, ya que el inicio automático ha intentado iniciar la base de datos durante el arranque de la máquina virtual. Sin embargo, la base de datos requiere de una recuperación y es probable que solo esté en fase de montaje, por lo que primero se ejecuta un cierre preparatorio.

    ```bash
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> shutdown immediate
    SQL> startup mount
    SQL> recover automatic database;
    SQL> alter database open;
    ```
    
1. Compruebe que el contenido de la base de datos se haya recuperado:

    ```bash
    SQL> select * from scott.scott_table;
    ```

La creación y recuperación de la copia de seguridad de la base de datos de Oracle Database 19c en una máquina virtual Linux de Azure han terminado.

## <a name="delete-the-vm"></a>Eliminación de la máquina virtual

Cuando ya no necesite la máquina virtual, puede usar el comando siguiente para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados:

```azurecli
az group delete --name rg-oracle
```

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Creación de máquinas virtuales de alta disponibilidad](../../linux/create-cli-complete.md)

[Ejemplos de la CLI de Azure para implementación de máquinas virtuales](../../linux/cli-samples.md)
