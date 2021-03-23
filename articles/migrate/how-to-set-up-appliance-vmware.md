---
title: Configurar un dispositivo de Azure Migrate para VMware
description: Obtenga información sobre cómo configurar un dispositivo de Azure Migrate para evaluar y migrar VM de VMware.
author: vikram1988
ms.author: vibansa
ms.manager: abhemraj
ms.topic: how-to
ms.date: 04/16/2020
ms.openlocfilehash: bac82b2939e5b6a674c75be2cd330dd0fa4b8487
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102035805"
---
# <a name="set-up-an-appliance-for-vmware-vms"></a>Configuración de un dispositivo para máquinas virtuales de VMware

Siga este artículo para configurar el dispositivo de Azure Migrate para su evaluación con la herramienta [Azure Migrate:Server Assessment](migrate-services-overview.md#azure-migrate-server-assessment-tool) y para la migración sin agente mediante la herramienta [Azure Migrate:Server Migration](migrate-services-overview.md#azure-migrate-server-migration-tool).

El [dispositivo de Azure Migrate](migrate-appliance.md) es un dispositivo ligero que Azure Migrate: evaluación de servidores y migración de servidores usan para detectar los servidores que se ejecutan en vCenter Server, enviar los metadatos de configuración y rendimiento del servidor a Azure y para la replicación de servidores con la migración sin agente.

Puede implementar el dispositivo mediante un par de métodos:

- Cree un servidor en vCenter Server mediante una plantilla OVA descargada. Este es el método que se describe en este artículo.
- Configure el dispositivo en un servidor existente mediante un script de instalador de PowerShell. [Este método](deploy-appliance-script.md) se debe usar si no se puede emplear una plantilla OVA o si trabaja en Azure Government.

Una vez creada la aplicación, compruebe que se puede conectar a Azure Migrate: evaluación de servidores, regístrela en el proyecto de Azure Migrate y configure el dispositivo para iniciar la detección.

## <a name="deploy-with-ova"></a>Implementación con OVA

Para configurar el dispositivo mediante una plantilla de OVA:
1. Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
1. Descargue una plantilla OVA e impórtela en vCenter Server. Compruebe que la plantilla OVA sea segura.
1. Cree la máquina virtual del dispositivo desde el archivo OVA, y compruebe que se puede conectar a Azure Migrate.
1. Configure el dispositivo por primera vez y regístrelo en el proyecto mediante la clave del proyecto de Azure Migrate.

### <a name="1-generate-the-azure-migrate-project-key"></a>1. Generación de la clave del proyecto de Azure Migrate

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, seleccione **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Sí, con VMware vSphere Hypervisor**.
3. En **1: Generación de la clave del proyecto de Azure Migrate**, proporcione un nombre para el dispositivo de Azure Migrate que configurará para la detección de servidores en el entorno de VMware. El nombre debe ser alfanumérico con 14 caracteres como máximo.
1. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar durante la creación de recursos.
1. Después de la creación correcta de los recursos de Azure, se genera una **clave del proyecto Azure Migrate**.
1. Copie la clave, ya que la necesitará para completar el registro del dispositivo durante su configuración.

### <a name="2-download-the-ova-template"></a>2. Descarga de la plantilla OVA

En **2: Descargar dispositivo de Azure Migrate**, seleccione el archivo .OVA y haga clic en **Descargar**.

### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo OVA es seguro antes de implementarlo:

1. En el servidor en el que descargó el archivo, abra una ventana de comandos de administrador.
2. Ejecute el siguiente comando para generar el código hash del archivo OVA:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Ejemplo de uso: ```C:\>CertUtil -HashFile C:\Users\Administrator\Desktop\MicrosoftAzureMigration.ova SHA256```

3. Compruebe las versiones más recientes del dispositivo y los valores hash:

    - Para la nube pública de Azure:
    
        **Algoritmo** | **Descargar** | **SHA256**
        --- | --- | ---
        VMware (11,9 GB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140333) | e9c9a1fe4f3ebae81008328e8f3a7933d78ff835ecd871d1b17f367621ce3c74


### <a name="3-create-the-appliance-server"></a>3. Creación del servidor del dispositivo

Importación del archivo descargado y creación de un servidor en el entorno de VMware

1. En la consola de cliente de vSphere, haga clic en **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).
2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo OVA.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para el servidor. Seleccione el objeto de inventario en el que se hospedará el servidor.
5. En **Host/Cluster** (Host o clúster), especifique el host o clúster en el que se ejecutará el servidor.
6. En **Storage** (Almacenamiento), especifique el destino de almacenamiento para el servidor.
7. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
8. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará el servidor. La red necesita conectividad a Internet, para enviar metadatos a Azure Migrate.
9. Revise y confirme la configuración y haga clic en **Finish** (Finalizar).


### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que el servidor de dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [pública](migrate-appliance.md#public-cloud-urls) y del [Gobierno](migrate-appliance.md#government-cloud-urls).


### <a name="4-configure-the-appliance"></a>4. Configuración del dispositivo

Configure el dispositivo por primera vez.

> [!NOTE]
> Si configura el dispositivo mediante un [**script de PowerShell**](deploy-appliance-script.md) en lugar de la plantilla OVA descargada, los dos primeros pasos de este procedimiento no son pertinentes.

1. En la consola de cliente de vSphere, haga clic con el botón derecho en el servidor y, luego, seleccione **Open Console** (Abrir consola).
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Abra un explorador en cualquier equipo que pueda conectarse al servidor del dispositivo y abra la dirección URL del administrador de configuración del dispositivo: `https://appliance name or IP address: 44368`.

   También puede abrir el administrador de configuración desde el escritorio del servidor del dispositivo mediante la selección de su acceso directo.
1. Acepte los **términos de licencia** y lea la información de terceros.
1. En administrador de configuración > **Configurar los requisitos previos**, realice las siguientes operaciones:
   - **Conectividad**: el dispositivo comprueba que el servidor tenga acceso a Internet. Si el servidor usa un proxy:
     - Haga clic en **Configurar el proxy** y especifique la dirección del proxy con los formatos `http://ProxyIPAddress` o `http://ProxyFQDN` y el puerto de escucha.
     - Especifique las credenciales si el proxy requiere autenticación.
     - Solo se admite un proxy HTTP.
     - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
   - **Time sync** (Sincronización de hora): Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
   - **Instalación de actualizaciones**: el dispositivo garantiza que se instalan las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los servicios que se ejecutan en el servidor del dispositivo.
   - **Install VDDK** (Instalar VDDK): El dispositivo comprueba si VMware vSphere Virtual Disk Development Kit (VDDK) está instalado. Si no está instalado, descargue VDDK 6.7 de VMware y extraiga el contenido del archivo ZIP descargado en la ubicación especificada del dispositivo, tal como se indica en las **instrucciones de instalación**.

     La migración de servidores de Azure Migrate usa VDDK para replicar los servidores durante la migración a Azure. 
1. Si quiere, puede **volver a ejecutar los requisitos previos** en cualquier momento durante la configuración del dispositivo para comprobar si el dispositivo cumple todos los requisitos previos.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-prerequisites.png" alt-text="Panel 1 del administrador de configuración del dispositivo":::


## <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave del proyecto de Azure Migrate** copiada desde el portal. Si no tiene la clave, vaya a **Server Assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
1. Necesitará un código de dispositivo para autenticarse con Azure. Al hacer clic en **Iniciar sesión** se abrirá un modal con el código del dispositivo, tal como se muestra a continuación.

    :::image type="content" source="./media/tutorial-discover-vmware/device-code.png" alt-text="Modal que muestra el código del dispositivo":::

1. Haga clic en **Copiar código e Iniciar sesión** para copiar el código del dispositivo y abrir un símbolo del sistema de inicio de sesión de Azure en una nueva pestaña del explorador. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
1. En la nueva pestaña, pegue el código del dispositivo e inicie sesión con su nombre de usuario y contraseña de Azure.
   
   No se admite el inicio de sesión con un PIN.
3. En caso de que cierre accidentalmente la pestaña de inicio de sesión sin iniciar sesión, deberá actualizar la pestaña explorador del administrador de configuración del dispositivo para volver a habilitar el botón Iniciar sesión.
1. Una vez que haya iniciado sesión correctamente, vuelva a la pestaña anterior con el administrador de configuración del dispositivo.
1. Si la cuenta de usuario de Azure que se usa para el registro tiene los permisos adecuados en los recursos de Azure creados durante la generación de la clave, se iniciará el registro del dispositivo.
1. Una vez que el dispositivo se ha registrado correctamente, puede ver los detalles de registro haciendo clic en **Ver detalles**.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-registration.png" alt-text="Panel 2 del Administrador de configuración del dispositivo":::

## <a name="start-continuous-discovery"></a>Inicio de detección continua

### <a name="provide-vcenter-server-details"></a>Proporción de los detalles de vCenter Server

El dispositivo necesita conectarse a vCenter Server para detectar los datos de configuración y rendimiento de los servidores.

1. En **Step 1: Provide vCenter Server credentials** (Paso 1: Proporcionar las credenciales de vCenter Server), haga clic en **Agregar credenciales** para especificar un nombre descriptivo para las credenciales, agregue un **Nombre de usuario** y una **Contraseña** para la cuenta de vCenter Server que el dispositivo usará para detectar los servidores que se ejecutan en vCenter Server.
    - Debe haber configurado una cuenta con los permisos necesarios, como se explica en este artículo.
    - Si desea limitar el ámbito de la detección a objetos específicos de VMware (centros de datos de vCenter Server, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o máquinas virtuales individuales), consulte las instrucciones de [este artículo](set-discovery-scope.md) para restringir la cuenta usada por Azure Migrate.
1. En **Paso 2: Proporcionar los detalles de vCenter Server**, haga clic en **Agregar origen de detección** para seleccionar el nombre descriptivo de las credenciales en la lista desplegable, especifique la **Dirección IP/FQDN** de vCenter Server. Puede dejar el **puerto** en el valor predeterminado (443) o especificar un puerto personalizado en el que vCenter Server escuche y hacer clic en **Guardar**.
1. Al hacer clic en **Guardar**, el dispositivo intentará validar la conexión a vCenter Server con las credenciales proporcionadas y mostrará el **Estado de validación** en la tabla con la Dirección IP/FQDN de vCenter Server.
1. Puede **volver a validar** la conectividad a vCenter Server en cualquier momento antes de iniciar la detección.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-manage-sources.png" alt-text="Panel 3 del administrador de configuración de dispositivos para obtener los detalles de vCenter Server":::

### <a name="provide-server-credentials"></a>Proporción de las credenciales del servidor

En **Step 3: Provide server credentials to perform software inventory, agentless dependency analysis and discovery of SQL Server instances and databases** (Paso 3: Proporcionar credenciales de servidor para realizar el inventario de software, el análisis de dependencias sin agente y la detección de instancias y bases de datos de SQL Server), puede elegir proporcionar varias credenciales de servidor o, si no desea aprovechar estas características, puede omitir el paso y continuar con la detección de vCenter Server. Puede cambiar la intención en cualquier momento más tarde.

:::image type="content" source="./media/tutorial-discover-vmware/appliance-server-credentials-mapping.png" alt-text="Panel 3 del administrador de configuración de dispositivos para obtener los detalles del servidor":::

> [!Note]
> La detección y evaluación de las instancias y bases de datos de SQL Server que se ejecutan en el entorno de VMware se encuentran ahora en versión preliminar. Para probar esta característica, use [**este vínculo**](https://aka.ms/AzureMigrate/SQL) para crear un proyecto en la región **Este de Australia**. Si ya tiene un proyecto en la región Este de Australia y desea probar esta característica, asegúrese de que ha completado estos [**requisitos previos**](how-to-discover-sql-existing-project.md) en el portal.

Si quiere aprovechar estas características, puede proporcionar las credenciales del servidor siguiendo los pasos que se indican a continuación. El dispositivo intentará asignar automáticamente las credenciales a los servidores para ejecutar las características de detección.

- Puede agregar credenciales de servidor haciendo clic en el botón **Agregar credenciales**. Se abrirá un modal en el que puede elegir el **Tipo de credenciales** en la lista desplegable.
- Puede proporcionar credenciales de autenticación de dominio, Windows (no de dominio), Linux (no de dominio) o SQL Server. [Obtenga más información](add-server-credentials.md) sobre cómo proporcionar credenciales y cómo administrarlas.
- Para cada tipo de credenciales debe especificar un nombre descriptivo, agregar el **Nombre de usuario** y la **Contraseña** y hacer clic en **Guardar**.
- Si elige credenciales de dominio, también deberá especificar el FQDN del dominio. El FQDN es necesario para validar la autenticidad de las credenciales con la instancia de Active Directory de ese dominio.
- Revise los [permisos necesarios](add-server-credentials.md#required-permissions) en la cuenta para detectar las aplicaciones instaladas, el análisis de dependencias sin agente o para la detección de instancias y bases de datos de SQL Server.
- Si quiere agregar varias credenciales a la vez, haga clic en **Agregar más** para guardar y agregar más credenciales.
- Al hacer clic en **Guardar** o **Agregar más**, el dispositivo valida la autenticidad de las credenciales con la instancia de Active Directory del dominio. Esto se hace para evitar bloqueos de cuentas cuando el dispositivo realiza varias iteraciones para asignar credenciales a los respectivos servidores.
- Puede ver el **Estado de validación** de todas las credenciales de dominio en la tabla de credenciales. Solo se validarán las credenciales de dominio.
- Si no pasan la validación, puede hacer clic en el estado **Error** para ver el error encontrado y hacer clic en **Revalidate credentials** (Revalidar credenciales) después de corregir el problema para validar de nuevo las credenciales de dominio con errores.


### <a name="start-discovery"></a>Iniciar detección

1. Haga clic en **Iniciar detección** para iniciar la detección de vCenter Server. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección en Dirección IP/FQDN de vCenter en la tabla de orígenes.
1. Si ha proporcionado las credenciales del servidor, el inventario de software (detección de aplicaciones instaladas) se iniciará automáticamente después de que se haya completado la detección de vCenter Server. El inventario de software se realiza una vez cada 12 horas.
1. El [inventario de software](how-to-discover-applications.md) identifica las instancias de SQL Server que se ejecutan en los servidores y usan la información, el dispositivo intenta conectarse a las instancias a través de la autenticación de Windows o las credenciales de autenticación de SQL Server proporcionadas en el dispositivo y recopila datos de las bases de datos de SQL Server y sus propiedades. La detección de SQL se realiza una vez cada 24 horas.
1. Durante el inventario de software, las credenciales de los servidores agregados se iterarán en los servidores y se validarán para el análisis de dependencias sin agente. Puede habilitar el análisis de dependencias sin agente para los servidores desde el portal. Solo se pueden seleccionar los servidores en los que la validación se realiza correctamente para habilitar el análisis de dependencias sin agente.

La detección funciona como se indica a continuación:
- El inventario de servidores detectados tarda unos 15 minutos en aparecer en el portal.
- La detección de las aplicaciones instaladas puede tardar un poco. Todo depende del número de servidores que se detecten. En el caso de 500 servidores, el inventario de detección tarda aproximadamente una hora en aparecer en el portal de Azure Migrate.
- Una vez completada la detección de servidores, puede habilitar el análisis de dependencias sin agente en los servidores desde el portal.
- Las instancias y bases de datos de SQL Server comenzarán a aparecer en el portal en un plazo de 24 horas desde el inicio de la detección.

## <a name="next-steps"></a>Pasos siguientes

Revise los tutoriales para la [valoración de VMware](./tutorial-assess-vmware-azure-vm.md) y la [migración sin agente](tutorial-migrate-vmware.md).