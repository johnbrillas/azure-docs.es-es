---
title: 'Detección de los servidores que se ejecutan en el entorno de VMware con Azure Migrate: Discovery and assessment'
description: 'Aprenda a detectar los servidores locales que se ejecutan en el entorno de VMware con Azure Migrate: Discovery and assessment'
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 03/17/2021
ms.custom: mvc
ms.openlocfilehash: f394fd4b1b4124c259489580fb5dc320fedf73fa
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/23/2021
ms.locfileid: "104863656"
---
# <a name="tutorial-discover-servers-running-in-vmware-environment-with-azure-migrate-discovery-and-assessment"></a>Tutorial: Detección de los servidores que se ejecutan en el entorno de VMware con Azure Migrate: Discovery and assessment

Como parte del recorrido de la migración a Azure, puede detectar el inventario y las cargas de trabajo locales.

En este tutorial aprenderá a detectar los servidores que se ejecutan en el entorno de VMware con Azure Migrate: Discovery and assessment, mediante un dispositivo de Azure Migrate ligero. El dispositivo se implementará como un servidor que se ejecuta en vCenter Server para detectar ininterrumpidamente los servidores y sus metadatos de rendimiento, las aplicaciones que se ejecutan en dichos servidores, las dependencias de los servidores, y las instancias y bases de datos de SQL Server.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una cuenta de Azure.
> * Preparar el entorno de VMware para la detección.
> * Crear un proyecto.
> * Configurar el dispositivo de Azure Migrate.
> * Iniciar la detección continua.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas siempre que sea posible.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, compruebe que dispone de estos requisitos previos.


**Requisito** | **Detalles**
--- | ---
**Host vCenter Server/ESXi** | Necesita una instancia de vCenter Server que ejecute las versiones 5.5, 6.0, 6.5 o 6.7.<br/><br/> Los servidores deben hospedarse en un host ESXi que ejecute la versión 5.5 u otra posterior.<br/><br/> En vCenter Server, permita las conexiones entrantes en el puerto TCP 443 para que el dispositivo recopile los metadatos de configuración y rendimiento.<br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter Server en el puerto 443. Si vCenter Server escucha a través de un puerto diferente, se puede modificar el puerto al proporcionar los detalles de vCenter Server en el administrador de configuración del dispositivo.<br/><br/> En los hosts ESXi, asegúrese de que el acceso de entrada esté permitido en el puerto TCP 443 para realizar la detección de las aplicaciones instaladas y el análisis de dependencias sin agente en los servidores.
**Dispositivo** | vCenter Server necesita ciertos recursos para poder asignar un servidor al dispositivo de Azure Migrate:<br/><br/> - 32 GB de RAM, 8 CPU virtuales y alrededor de 80 GB de almacenamiento en disco.<br/><br/> - Un conmutador virtual externo y acceso a Internet en el servidor del dispositivo, ya sea directamente o mediante un proxy.
**Servidores** | Todas las versiones de los sistemas operativos Windows y Linux pueden detectar los metadatos de configuración y rendimiento. <br/><br/> Pueden utilizarse todas las versiones de los sistemas operativos Windows o Linux para detectar aplicaciones en los servidores. Consulte [esta página](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) para ver las versiones de sistemas operativos compatibles con el análisis de dependencia sin agente.<br/><br/> Para realizar la detección de las aplicaciones instaladas y el análisis de dependencias sin agente, las herramientas de VMware (posteriores a 10.2.0) deben estar instaladas y en ejecución en los servidores. Los servidores de Windows deben tener instalada la versión 2.0 de PowerShell u otra posterior.<br/><br/> Para detectar las instancias y bases de datos de SQL Server, consulte [aquí](migrate-support-matrix-vmware.md#requirements-for-discovery-of-sql-server-instances-and-databases) las versiones y ediciones de SQL Server compatibles, así como las versiones de los sistemas operativos Windows y los mecanismos de autenticación que se admiten.

## <a name="prepare-an-azure-user-account"></a>Preparación de una cuenta de usuario de Azure

Para crear un proyecto y registrar el dispositivo de Azure Migrate, necesita una cuenta con:

- Permisos de Colaborador o Propietario en la suscripción de Azure.
- Permisos para registrar aplicaciones de Azure Active Directory (AAD).
- Permisos de Colaborador o Propietario, además de los del Administrador de acceso de usuario, en la suscripción de Azure para crear un almacén de claves, que se usará durante la migración de servidores sin agente.

Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción. Si no es el propietario, trabaje con él para asignar los permisos, como se indica a continuación:

1. En Azure Portal, busque "suscripciones" y, en **Servicios**, seleccione **Suscripciones**.

    :::image type="content" source="./media/tutorial-discover-vmware/search-subscription.png" alt-text="Cuadro para buscar la suscripción de Azure":::


2. En la página **Suscripciones**, seleccione aquella en la que desea crear un proyecto.
3. En la suscripción, seleccione **Access control (IAM)**  > **Comprobar acceso**.
4. En **Comprobar acceso**, busque la cuenta de usuario correspondiente.
5. En **Agregar una asignación de roles**, haga clic en **Agregar**.
:::image type="content" source="./media/tutorial-discover-vmware/azure-account-access.png" alt-text="Búsqueda de una cuenta de usuario para comprobar el acceso y asignar un rol":::
    
6. En **Agregar asignación de roles**, seleccione el rol Colaborador o Propietario, y seleccione la cuenta (azmigrateuser en nuestro ejemplo). A continuación, haga clic en **Save**(Guardar).

    :::image type="content" source="./media/tutorial-discover-vmware/assign-role.png" alt-text="Se abre la página Agregar asignación de roles para asignar un rol a la cuenta.":::

1. Con el fin de registrar el dispositivo, la cuenta de Azure necesita **permisos para registrar aplicaciones de AAD**.
1. En Azure Portal, vaya a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
1. En **Configuración de usuario**, compruebe que los usuarios de Azure AD puedan registrar aplicaciones (establecido en **Sí** de forma predeterminada).

    :::image type="content" source="./media/tutorial-discover-vmware/register-apps.png" alt-text="Comprobación en la configuración de usuario de que los usuarios puedan registrar aplicaciones de Active Directory":::

9. Si la configuración de "Registros de aplicaciones" se ha establecido en "No", solicite al administrador global o de inquilinos que asigne el permiso necesario. Como alternativa, el administrador global o de inquilinos puede asignar el rol **Desarrollador de aplicaciones** a una cuenta para permitir el registro de aplicaciones de AAD. [Más información](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-vmware"></a>Preparación de VMware

En vCenter Server, compruebe que la cuenta tiene permisos para crear una máquina virtual mediante un archivo OVA. Esto es necesario al implementar el dispositivo Azure Migrate como una máquina virtual VMware mediante un archivo OVA.

Azure Migrate necesita una cuenta de solo lectura de vCenter Server para detectar y evaluar los servidores que se ejecutan en el entorno de VMware. Si también quiere llevar a cabo la detección de las aplicaciones instaladas y el análisis de dependencias sin agente, la cuenta debe tener habilitados los privilegios de **Virtual Machines > Guest Operations** (Operaciones de invitado).

### <a name="create-an-account-to-access-vcenter"></a>Creación de una cuenta para acceder a vCenter

En el cliente web de vSphere, configure una cuenta de la siguiente manera:

1. Con una cuenta con privilegios de administración, en el cliente web de vSphere > seleccione **Administration** (Administración).
2. En **Access** (Acceso), seleccione **SSO Users and Groups** (Usuarios y grupos de SSO).
3. En **Users** (Usuarios), agregue un nuevo usuario.
4. En **New User** (Nuevo usuario), escriba los detalles de la cuenta. A continuación, haga clic en **Aceptar**.
5. En **Global Permissions** (Permisos globales), seleccione la cuenta de usuario y asígnele el rol **Read-only** (Solo lectura). A continuación, haga clic en **Aceptar**.
6. Si también desea realizar la detección de las aplicaciones instaladas y el análisis de dependencias sin agente, vaya a **Roles**, seleccione el rol **Solo lectura** y, en **Privilegios**, seleccione **Guest Operations** (Operaciones de invitado). Los privilegios se pueden propagar a todos los objetos de vCenter Server. Para ello, seleccione la casilla "Propagate to children" (Propagar a elementos secundarios).

    :::image type="content" source="./media/tutorial-discover-vmware/guest-operations.png" alt-text="Casilla para permitir las operaciones de invitado en el rol de solo lectura":::


> [!NOTE]
> Si lo desea, puede establecer el ámbito de la cuenta de vCenter Server para limitar la detección a los centros de datos, los clústeres, una carpeta de clústeres, los hosts, una carpeta de hosts o servidores concretos. [**Descubra**](set-discovery-scope.md) cómo puede establecer el ámbito de la cuenta de usuario de vCenter Server.

### <a name="create-an-account-to-access-servers"></a>Creación de una cuenta para acceder a los servidores

Para realizar la detección de las aplicaciones instaladas, el análisis de dependencias sin servidor y la detección de instancias y bases de datos de SQL Server, necesita una cuenta de usuario que tenga los privilegios apropiados en los servidores. Puede proporcionar la cuenta de usuario en el administrador de configuración del dispositivo. El dispositivo no instala ningún agente en los servidores.

1. En los servidores de Windows, debe crear una cuenta (local o de dominio) que tenga permisos administrativos en los servidores. Para detectar las instancias y las bases de datos de SQL Server, necesita que la cuenta de Windows o de SQL Server sea miembro del rol de servidor sysadmin. [Descubra](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles) cómo puede asignar el rol apropiado a la cuenta de usuario.
2. En los servidores de Linux, cree una cuenta con privilegios raíz. Como alternativa, puede crear una cuenta con los siguientes permisos en los archivos/bin/netstat y/bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE.

> [!NOTE]
> Ahora puede agregar varias credenciales de servidor en el administrador de configuración para realizar la detección de las aplicaciones instaladas, el análisis de dependencias sin agente y la detección de instancias y bases de datos de SQL Server. Puede agregar varias credenciales de autenticación de dominio, de Windows (no de dominio), de Linux (no de dominio) o de SQL Server. [**Más información**](add-server-credentials.md)

## <a name="set-up-a-project"></a>Configuración de un proyecto

Configure un proyecto nuevo.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, seleccione **Crear proyecto**.
5. En **Crear proyecto**, seleccione su suscripción y grupo de recursos de Azure. Cree un grupo de recursos si no tiene ninguno.
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la región geográfica en la que quiere crearlo. Revise las zonas geográficas admitidas para nubes [públicas](migrate-support-matrix.md#supported-geographies-public-cloud) y [nubes gubernamentales](migrate-support-matrix.md#supported-geographies-azure-government).

    :::image type="content" source="./media/tutorial-discover-vmware/new-project.png" alt-text="Cuadros de nombre de proyecto y región":::

7. Seleccione **Crear**.
8. Espere unos minutos para que el proyecto se implemente. La herramienta **Azure Migrate: Discovery and assessment** se agrega al proyecto nuevo de manera predeterminada.

> [!NOTE]
> Si ya ha creado un proyecto, puede usarlo para registrar otros dispositivos adicionales y, de este modo, detectar y evaluar un número mayor de servidores. [**Más información**](create-manage-projects.md#find-a-project).

## <a name="set-up-the-appliance"></a>Configuración del dispositivo

La herramienta Azure Migrate: Discovery and assessment usa un dispositivo de Azure Migrate ligero. El dispositivo de Azure Migrate detecta los servidores y envía los metadatos de configuración y rendimiento a Azure Migrate. El dispositivo se puede configurar utilizando una plantilla OVA que está disponible para descargase del proyecto.

> [!NOTE]
> Si, por algún motivo, no puede configurar el dispositivo mediante la plantilla, puede hacerlo con un script de PowerShell en un servidor de Windows Server 2016 existente. [**Obtenga más información**](deploy-appliance-script.md#set-up-the-appliance-for-vmware).

### <a name="deploy-with-ova"></a>Implementación con OVA

Para configurar el dispositivo mediante una plantilla de OVA:

1. Proporcione un nombre de dispositivo y genere una clave de proyecto en el portal.
1. Descargue una plantilla OVA e impórtela en vCenter Server. Compruebe que la plantilla OVA sea segura.
1. Cree el dispositivo desde el archivo OVA y compruebe que la conexión a Azure Migrate.
1. Configure el dispositivo por primera vez y regístrelo con el proyecto utilizando la clave del proyecto.

### <a name="1-generate-the-project-key"></a>1. Generación de la clave de proyecto

1. En **Objetivos de migración** > **Windows, Linux and SQL Servers** > **Azure Migrate: Discovery and assessment** (Servidores Windows, Linux y SQL > Azure Migrate: Discovery and assessment), seleccione **Detectar**.
2. En **Discover servers** > **Are your servers virtualized?** (Detectar servidores > ¿Están virtualizados sus servidores?), seleccione **Yes, with VMware vSphere hypervisor** (Sí, con el hipervisor de VMware vSphere).
3. En **1:Generate project key** (1: Generar la clave de proyecto), especifique un nombre para el dispositivo de Azure Migrate que va a configurar para la detección de servidores en el entorno de VMware. Este nombre debe ser alfanumérico y no puede tener más de 14 caracteres.
1. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar durante la creación de recursos.
1. Después de la creación correcta de los recursos de Azure, se genera una **clave de proyecto**.
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

    - Para Azure Government:
    
        **Algoritmo** | **Descargar** | **SHA256**
        --- | --- | ---
        VMware (85,8 MB) | [La versión más reciente](https://go.microsoft.com/fwlink/?linkid=2140337) | 2daaa2a59302bf911e8ef195f8add7d7c8352de77a9af0b860e2a627979085ca

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

Asegúrese de que el servidor del dispositivo puede conectarse a las direcciones URL de Azure de la nube [pública](migrate-appliance.md#public-cloud-urls) y la nube del [Gobierno](migrate-appliance.md#government-cloud-urls).


### <a name="4-configure-the-appliance"></a>4. Configuración del dispositivo

Configure el dispositivo por primera vez.

> [!NOTE]
> Si configura el dispositivo mediante un [**script de PowerShell**](deploy-appliance-script.md) en lugar de la plantilla OVA descargada, los dos primeros pasos de este procedimiento no son pertinentes.

1. En la consola de cliente de vSphere, haga clic con el botón derecho en el servidor y, luego, seleccione **Open Console** (Abrir consola).
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Inicie un explorador en cualquier máquina que pueda conectarse al dispositivo y abra la dirección URL del administrador de configuración del dispositivo: `https://appliance name or IP address: 44368`.

   También puede abrir el administrador de configuración desde el escritorio del servidor del dispositivo mediante la selección de su acceso directo.
1. Acepte los **términos de licencia** y lea la información de terceros.
1. En administrador de configuración > **Configurar los requisitos previos**, realice las siguientes operaciones:
   - **Conectividad**: el dispositivo comprueba que el servidor tenga acceso a Internet. Si el servidor usa un proxy:
     - Haga clic en **Configurar el proxy** y especifique la dirección del proxy, `http://ProxyIPAddress` o `http://ProxyFQDN`, y el puerto de escucha.
     - Especifique las credenciales si el proxy requiere autenticación.
     - Solo se admite un proxy HTTP.
     - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
   - **Time sync** (Sincronización de hora): Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
   - **Instalación de actualizaciones**: el dispositivo garantiza que se instalan las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los servicios que se ejecutan en el servidor del dispositivo.
   - **Install VDDK** (Instalar VDDK): El dispositivo comprueba si VMware vSphere Virtual Disk Development Kit (VDDK) está instalado. Si no está instalado, descargue VDDK 6.7 de VMware y extraiga el contenido del archivo ZIP descargado en la ubicación especificada del dispositivo, tal como se indica en las **instrucciones de instalación**.

     La migración de servidores de Azure Migrate usa VDDK para replicar los servidores durante la migración a Azure. 
1. Si quiere, puede **volver a ejecutar los requisitos previos** en cualquier momento durante la configuración del dispositivo para comprobar si el dispositivo cumple todos los requisitos previos.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-prerequisites.png" alt-text="Panel 1 del administrador de configuración del dispositivo":::

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave de proyecto** copiada desde el portal. Si no tiene la clave, vaya a **Azure Migrate: Discovery and assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
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
    - Si desea limitar el ámbito de la detección a objetos específicos de VMware (centros de datos de vCenter Server, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o máquinas virtuales individuales), consulte las instrucciones de [este artículo](set-discovery-scope.md) para restringir la cuenta que usa Azure Migrate.
1. En **Paso 2: Proporcionar los detalles de vCenter Server**, haga clic en **Agregar origen de detección** para seleccionar el nombre descriptivo de las credenciales en la lista desplegable, especifique la **Dirección IP/FQDN** de vCenter Server. Puede dejar el **puerto** en el valor predeterminado (443) o especificar un puerto personalizado en el que vCenter Server escuche y hacer clic en **Guardar**.
1. Al hacer clic en **Guardar**, el dispositivo intentará validar la conexión a vCenter Server con las credenciales proporcionadas y mostrará el **Estado de validación** en la tabla con la Dirección IP/FQDN de vCenter Server.
1. Puede **volver a validar** la conectividad a vCenter Server en cualquier momento antes de iniciar la detección.

    :::image type="content" source="./media/tutorial-discover-vmware/appliance-manage-sources.png" alt-text="Panel 3 del administrador de configuración de dispositivos para obtener los detalles de vCenter Server":::

### <a name="provide-server-credentials"></a>Proporción de las credenciales del servidor

En **Step 3: Provide server credentials to perform software inventory, agentless dependency analysis and discovery of SQL Server instances and databases** (Paso 3: Proporcione las credenciales de los servidores para realizar el inventario de software, el análisis de dependencias sin agente y la detección de instancias y bases de datos de SQL Server), proporcione las credenciales de varios servidores o, si no desea aprovechar estas características, omita este paso y continúe con la detección de vCenter Server. Puede cambiar la opción en cualquier momento.

:::image type="content" source="./media/tutorial-discover-vmware/appliance-server-credentials-mapping.png" alt-text="Panel 3 del administrador de configuración de dispositivos para obtener los detalles del servidor":::

Si quiere aprovechar estas características, proporcione las credenciales del servidor siguiendo los pasos que se indican a continuación. El dispositivo intentará asignar automáticamente las credenciales a los servidores para ejecutar las características de detección.

- Haga clic en el botón **Agregar credenciales** para agregar credenciales de servidor. Se abrirá un modal donde podrá elegir el valor de **Credentials type** de la lista desplegable.
- Puede proporcionar credenciales de autenticación de dominio, Windows (no de dominio), Linux (no de dominio) o SQL Server. [Obtenga más información](add-server-credentials.md) sobre cómo proporcionar credenciales y cómo administrarlas.
- Para cada tipo de credenciales debe especificar un nombre descriptivo, agregar el **Nombre de usuario** y la **Contraseña** y hacer clic en **Guardar**.
- Si elige credenciales de dominio, también deberá especificar el nombre de dominio completo. El nombre de dominio completo es obligatorio para validar la autenticidad de las credenciales con la instancia de Active Directory de ese dominio.
- Revise los [permisos necesarios](add-server-credentials.md#required-permissions) en la cuenta para detectar las aplicaciones instaladas, el análisis de dependencias sin agente o para la detección de instancias y bases de datos de SQL Server.
- Si quiere agregar varias credenciales a la vez, haga clic en **Agregar más** para guardar y agregar más credenciales.
- Al hacer clic en **Guardar** o **Agregar más**, el dispositivo valida la autenticidad de las credenciales con la instancia de Active Directory del dominio. Esto se hace para evitar bloqueos de cuentas cuando el dispositivo realiza varias iteraciones para asignar credenciales a los respectivos servidores.
- Puede ver el **Estado de validación** de todas las credenciales de dominio en la tabla de credenciales. Solo se validarán las credenciales de dominio.
- Si no pasan la validación, puede hacer clic en el estado **Error** para ver el error encontrado y hacer clic en **Revalidate credentials** (Revalidar credenciales) después de corregir el problema para validar de nuevo las credenciales de dominio con errores.

### <a name="start-discovery"></a>Iniciar detección

1. Haga clic en **Iniciar detección** para iniciar la detección de vCenter Server. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección en Dirección IP/FQDN de vCenter en la tabla de orígenes.
1. Si ha proporcionado las credenciales del servidor, el inventario de software (detección de aplicaciones instaladas) se iniciará automáticamente después de que se haya completado la detección de vCenter Server. El inventario de software se realiza una vez cada 12 horas.
1. El [inventario de software](how-to-discover-applications.md) identifica las instancias de SQL Server que se ejecutan en los servidores y usan la información, el dispositivo intenta conectarse a las instancias a través de la autenticación de Windows o las credenciales de autenticación de SQL Server proporcionadas en el dispositivo y recopila datos de las bases de datos de SQL Server y sus propiedades. La detección de SQL se realiza una vez cada 24 horas.
1. Durante el inventario de software, las credenciales de los servidores agregados se iteran en los servidores y se validan para realizar el análisis de dependencias sin agente. Puede habilitar el análisis de dependencias sin agente de los servidores desde el portal. Los únicos servidores que se pueden seleccionar para habilitar el análisis de dependencias sin agente son los que superan correctamente la validación.

> [!Note]
>Azure Migrate cifrará la comunicación entre el dispositivo de Azure Migrate y las instancias de SQL Server de origen de datos(con la propiedad "Cifrar conexión" establecida en TRUE). Estas conexiones se cifran con [**TrustServerCertificate**](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.trustservercertificate) (establecido en TRUE); la capa de transporte usará SSL para cifrar el canal y evitar la cadena de certificados para validar la confianza. El servidor del dispositivo se debe configurar para [**confiar en la entidad de certificación raíz del certificado**](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).<br/>
Si no se ha aprovisionado ningún certificado en el servidor cuando se inicia, SQL Server genera un certificado autofirmado que se usa para cifrar los paquetes de inicio de sesión. [**Obtenga más información**](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine).

La detección funciona como se indica a continuación:

- El inventario de servidores detectados tarda unos 15 minutos en aparecer en el portal.
- La detección de las aplicaciones instaladas puede tardar un poco. Todo depende del número de servidores que se detecten. En el caso de 500 servidores, el inventario de detección tarda aproximadamente una hora en aparecer en el portal de Azure Migrate.
- Una vez completada la detección de servidores, puede habilitar el análisis de dependencias sin agente en los servidores desde el portal.
- Las instancias y bases de datos de SQL Server comenzarán a aparecer en el portal en un plazo de 24 horas desde el inicio de la detección.

## <a name="next-steps"></a>Pasos siguientes

- [Evaluación de servidores](./tutorial-assess-vmware-azure-vm.md) para migrar a máquinas virtuales de Azure.
- [Evaluación de servidores SQL Server](./tutorial-assess-sql.md) para migrar a Azure SQL.
- [Revise los datos](migrate-appliance.md#collected-data---vmware) que el dispositivo recopila durante la detección.