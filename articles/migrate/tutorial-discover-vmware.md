---
title: Detección de las máquinas virtuales de VMware con Azure Migrate Server Assessment
description: Obtenga información acerca de cómo detectar las máquinas virtuales locales de VMware con la herramienta Azure Migrate Server Assessment
author: vineetvikram
ms.author: vivikram
ms.manager: abhemraj
ms.topic: tutorial
ms.date: 9/14/2020
ms.custom: mvc
ms.openlocfilehash: 0e06d82c30743a4084cfc5ff856b4a9c8d548146
ms.sourcegitcommit: ca215fa220b924f19f56513fc810c8c728dff420
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98566953"
---
# <a name="tutorial-discover-vmware-vms-with-server-assessment"></a>Tutorial: Detección de máquinas virtuales de VMware con Server Assessment

Como parte del recorrido de la migración a Azure, puede detectar el inventario y las cargas de trabajo locales.

En este tutorial se muestra cómo detectar las máquinas virtuales locales de VMware con la herramienta Azure Migrate: Server Assessment, con un dispositivo ligero de Azure Migrate. El dispositivo se implementa como una máquina virtual de VMware para detectar continuamente las máquinas virtuales y los metadatos del rendimiento, las aplicaciones que se ejecutan en las máquinas virtuales y las dependencias de máquina virtual.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar una cuenta de Azure.
> * Preparar el entorno de VMware para la detección.
> * Cree un proyecto de Azure Migrate.
> * Configurar el dispositivo de Azure Migrate.
> * Iniciar la detección continua.

> [!NOTE]
> Los tutoriales muestran la manera más rápida de probar un escenario y utilizar las opciones predeterminadas siempre que sea posible.  

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, compruebe que dispone de estos requisitos previos.


**Requisito** | **Detalles**
--- | ---
**Host vCenter Server/ESXi** | Necesita una instancia de vCenter Server que ejecute las versiones 5.5, 6.0, 6.5 o 6.7.<br/><br/> Las máquinas virtuales deben estar hospedadas en un host ESXi que ejecute la versión 5.5 o posterior.<br/><br/> En vCenter Server, permita conexiones entrantes en el puerto TCP 443 para que el dispositivo recopile los metadatos de configuración y rendimiento.<br/><br/> De forma predeterminada, el dispositivo se conecta a vCenter en el puerto 443. Si vCenter Server escucha a través de un puerto diferente, se puede modificar el puerto al proporcionar los detalles de vCenter Server en el administrador de configuración del dispositivo.<br/><br/> En el servidor ESXi que hospeda las máquinas virtuales, asegúrese de que permite el acceso de entrada en el puerto TCP 443 para detectar las aplicaciones instaladas en las máquinas virtuales y las dependencias de máquina virtual.
**Dispositivo** | vCenter Server necesita recursos para asignar una máquina virtual al dispositivo de Azure Migrate:<br/><br/> - 32 GB de RAM, 8 CPU virtuales y alrededor de 80 GB de almacenamiento en disco.<br/><br/> - Un conmutador virtual externo y acceso a Internet en la máquina virtual del dispositivo, directamente o a través de un proxy.
**Máquinas virtuales** | Todas las versiones de sistemas operativos Windows y Linux son compatibles con la detección de metadatos de configuración y rendimiento, y con la detección de las aplicaciones instaladas en las máquinas virtuales. <br/><br/> Consulte [esta página](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless) para ver las versiones de sistemas operativos compatibles con el análisis de dependencia sin agente.<br/><br/> Para detectar las aplicaciones instaladas y las dependencias de máquina virtual, el conjunto de utilidades VMware Tools (versión posterior a la 10.2.0) debe estar instalado y en ejecución en las máquinas virtuales, y las máquinas virtuales Windows deben tener instalado PowerShell 2.0 o una versión posterior.


## <a name="prepare-an-azure-user-account"></a>Preparación de una cuenta de usuario de Azure

Para crear un proyecto de Azure Migrate y registrar el dispositivo Azure Migrate, necesita una cuenta con:
- Permisos de Colaborador o Propietario en la suscripción de Azure.
- Permisos para registrar aplicaciones de Azure Active Directory (AAD).
- Permisos de Colaborador o Propietario, más Administrador de acceso de usuario en la suscripción de Azure para crear un almacén de claves, que se usará durante la migración de VMware sin agente.

Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción. Si no es el propietario, trabaje con él para asignar los permisos, como se indica a continuación:

1. En Azure Portal, busque "suscripciones" y, en **Servicios**, seleccione **Suscripciones**.

    ![Cuadro para buscar la suscripción de Azure](./media/tutorial-discover-vmware/search-subscription.png)

2. En la página **Suscripciones**, seleccione aquella en la que desee crear un proyecto de Azure Migrate. 
3. En la suscripción, seleccione **Access control (IAM)**  > **Comprobar acceso**.
4. En **Comprobar acceso**, busque la cuenta de usuario correspondiente.
5. En **Agregar una asignación de roles**, haga clic en **Agregar**.

    ![Búsqueda de una cuenta de usuario para comprobar el acceso y asignar un rol](./media/tutorial-discover-vmware/azure-account-access.png)

6. En **Agregar asignación de roles**, seleccione el rol Colaborador o Propietario, y seleccione la cuenta (azmigrateuser en nuestro ejemplo). A continuación, haga clic en **Save**(Guardar).

    ![Se abre la página Agregar asignación de roles para asignar un rol a la cuenta.](./media/tutorial-discover-vmware/assign-role.png)

1. Con el fin de registrar el dispositivo, la cuenta de Azure necesita **permisos para registrar aplicaciones de AAD**.
1. En Azure Portal, vaya a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
1. En **Configuración de usuario**, compruebe que los usuarios de Azure AD puedan registrar aplicaciones (establecido en **Sí** de forma predeterminada).

    ![Comprobación en la configuración de usuario de que los usuarios puedan registrar aplicaciones de Active Directory](./media/tutorial-discover-vmware/register-apps.png)

9. Si la configuración de "Registros de aplicaciones" se ha establecido en "No", solicite al administrador global o de inquilinos que asigne el permiso necesario. Como alternativa, el administrador global o de inquilinos puede asignar el rol **Desarrollador de aplicaciones** a una cuenta para permitir el registro de aplicaciones de AAD. [Más información](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="prepare-vmware"></a>Preparación de VMware

En vCenter Server, compruebe que la cuenta tiene permisos para crear una máquina virtual mediante un archivo OVA. Esto es necesario al implementar el dispositivo Azure Migrate como una máquina virtual VMware mediante un archivo OVA.

La herramienta Server Assessment necesita una cuenta de vCenter Server de solo lectura con fines de detección y evaluación de las máquinas virtuales de VMware. Si además desea detectar las aplicaciones instaladas y las dependencias de máquina virtual, la cuenta necesita tener privilegios habilitados para **Máquinas virtuales > Operaciones de invitado**.

### <a name="create-an-account-to-access-vcenter"></a>Creación de una cuenta para acceder a vCenter

En el cliente web de vSphere, configure una cuenta de la siguiente manera:

1. Con una cuenta con privilegios de administración, en el cliente web de vSphere > seleccione **Administration** (Administración).
2. En **Access** (Acceso), seleccione **SSO Users and Groups** (Usuarios y grupos de SSO).
3. En **Users** (Usuarios), agregue un nuevo usuario.
4. En **New User** (Nuevo usuario), escriba los detalles de la cuenta. A continuación, haga clic en **Aceptar**.
5. En **Global Permissions** (Permisos globales), seleccione la cuenta de usuario y asígnele el rol **Read-only** (Solo lectura). A continuación, haga clic en **Aceptar**.
6. Si también desea detectar las aplicaciones instaladas y las dependencias de máquina virtual, vaya a **Roles**, seleccione el rol de **solo lectura** y, en **Privilegios**, seleccione **Operaciones de invitado**. Los privilegios se pueden propagar a todos los objetos de vCenter Server. Para ello, seleccione la casilla "Propagate to children" (Propagar a elementos secundarios).
 
    ![Casilla para permitir las operaciones de invitado en el rol de solo lectura](./media/tutorial-discover-vmware/guest-operations.png)


### <a name="create-an-account-to-access-vms"></a>Creación de una cuenta para acceder a las máquinas virtuales

Necesita una cuenta de usuario que tenga los privilegios exigidos en las máquinas virtuales para detectar las aplicaciones instaladas y las dependencias de máquina virtual. Puede proporcionar la cuenta de usuario en el administrador de configuración del dispositivo. El dispositivo no instala ningún agente en las máquinas virtuales.

1. Para las máquinas virtuales Windows, cree una cuenta (local o de dominio) que tenga permisos administrativos en las máquinas virtuales.
2. Para las máquinas virtuales Linux, cree una cuenta con privilegios raíz. Como alternativa, puede crear una cuenta con los siguientes permisos en los archivos/bin/netstat y/bin/ls: CAP_DAC_READ_SEARCH y CAP_SYS_PTRACE.

> [!NOTE]
> Actualmente Azure Migrate admite una cuenta de usuario para las máquinas virtuales Windows y una cuenta de usuario para las máquinas virtuales Linux, que se pueden proporcionar en el dispositivo para la detección de aplicaciones instaladas y dependencias de máquina virtual.


## <a name="set-up-a-project"></a>Configuración de un proyecto

Configure un proyecto nuevo de Azure Migrate.

1. En Azure Portal > **Todos los servicios**, busque **Azure Migrate**.
2. En **Servicios**, seleccione **Azure Migrate**.
3. En **Información general**, seleccione **Crear proyecto**.
5. En **Crear proyecto**, seleccione su suscripción y grupo de recursos de Azure. Cree un grupo de recursos si no tiene ninguno.
6. En **Detalles del proyecto**, especifique el nombre del proyecto y la región geográfica en la que quiere crearlo. Revise las zonas geográficas admitidas para nubes [públicas](migrate-support-matrix.md#supported-geographies-public-cloud) y [nubes gubernamentales](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Cuadros de nombre de proyecto y región](./media/tutorial-discover-vmware/new-project.png)

7. Seleccione **Crear**.
8. Espere unos minutos a que se implemente el proyecto de Azure Migrate. La herramienta **Azure Migrate: Server Assessment** se agrega de forma predeterminada al nuevo proyecto.

![Página que muestra la herramienta Server Assessment agregada de forma predeterminada](./media/tutorial-discover-vmware/added-tool.png)

> [!NOTE]
> Si ya ha creado un proyecto, puede usarlo para registrar dispositivos adicionales con el fin de detectar y evaluar un número mayor de máquinas virtuales. [Más información](create-manage-projects.md#find-a-project).

## <a name="set-up-the-appliance"></a>Configuración del dispositivo

Azure Migrate: Server Assessment usa un dispositivo de Azure Migrate ligero. El dispositivo realiza la detección de máquinas virtuales y envía los metadatos de configuración y rendimiento de las máquinas virtuales a Azure Migrate. El dispositivo se puede configurar mediante una plantilla OVA que se descarga del proyecto de Azure Migrate.

> [!NOTE]
> Si, por algún motivo, no puede configurar el dispositivo mediante la plantilla, puede hacerlo con un script de PowerShell en un servidor de Windows Server 2016 existente. [Más información](deploy-appliance-script.md#set-up-the-appliance-for-vmware).


### <a name="deploy-with-ova"></a>Implementación con OVA

Para configurar el dispositivo mediante una plantilla de OVA:
1. Proporcione un nombre de dispositivo y genere una clave del proyecto de Azure Migrate en el portal.
1. Descargue una plantilla OVA e impórtela en vCenter Server. Compruebe que la plantilla OVA sea segura.
1. Crear el dispositivo y comprobar que se puede conectar a Azure Migrate Server Assessment.
1. Configure el dispositivo por primera vez y regístrelo en el proyecto de Azure Migrate mediante la clave del proyecto de Azure Migrate.

### <a name="1-generate-the-azure-migrate-project-key"></a>1. Generación de la clave del proyecto de Azure Migrate

1. En **Objetivos de migración** > **Servidores** > **Azure Migrate: Server Assessment**, seleccione **Detectar**.
2. En **Detectar máquinas** >  **¿Las máquinas están virtualizadas?** , haga clic en **Sí, con VMware vSphere Hypervisor**.
3. En **1: Generar la clave del proyecto de Azure Migrate**, proporcione un nombre para el dispositivo de Azure Migrate que configurará para la detección de máquinas virtuales de VMware. El nombre debe ser alfanumérico con 14 caracteres como máximo.
1. Haga clic en **Generar clave** para iniciar la creación de los recursos de Azure necesarios. No cierre la página Detectar máquinas durante la creación de recursos.
1. Después de la creación correcta de los recursos de Azure, se genera una **clave del proyecto Azure Migrate**.
1. Copie la clave, ya que la necesitará para completar el registro del dispositivo durante su configuración.

### <a name="2-download-the-ova-template"></a>2. Descarga de la plantilla OVA

En **2: Descargar dispositivo de Azure Migrate**, seleccione el archivo .OVA y haga clic en **Descargar**.

### <a name="verify-security"></a>Comprobación de la seguridad

Compruebe que el archivo OVA es seguro antes de implementarlo:

1. En la máquina en la que descargó el archivo, abra una ventana de comandos de administrador.
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

### <a name="3-create-the-appliance-vm"></a>3. Creación de la máquina virtual del dispositivo

Importe el archivo descargado y cree una máquina virtual.

1. En la consola de cliente de vSphere, haga clic en **File** (Archivo) > **Deploy OVF Template** (Implementar plantilla de OVF).
2. En el Deploy OVF Template Wizard (Asistente para implementar la plantilla de OVF) > **Source** (Origen), especifique la ubicación del archivo OVA.
3. En **Name** (Nombre) y **Location** (Ubicación), especifique un nombre descriptivo para la máquina virtual. Seleccione el objeto de inventario en el que se hospedará la máquina virtual.
5. En **Host/Cluster** (Host o clúster), especifique el host o clúster en que se ejecutará la máquina virtual.
6. En **Storage**, especifique el destino de almacenamiento de la máquina virtual.
7. En **Disk Format** (Formato de disco), especifique el tamaño y el tipo de disco.
8. En **Network Mapping** (Asignación de red), especifique la red a la que se conectará la máquina virtual. La red necesita conectividad a Internet para enviar metadatos a Azure Migrate Server Assessment.
9. Revise y confirme la configuración y haga clic en **Finish** (Finalizar).


### <a name="verify-appliance-access-to-azure"></a>Comprobación de que el dispositivo puede acceder a Azure

Asegúrese de que la VM del dispositivo pueda conectarse a las direcciones URL de Azure para las nubes [públicas](migrate-appliance.md#public-cloud-urls) y [gubernamentales](migrate-appliance.md#government-cloud-urls).


### <a name="4-configure-the-appliance"></a>4. Configuración del dispositivo

Configure el dispositivo por primera vez.

> [!NOTE]
> Si configura la aplicación mediante un [script de PowerShell](deploy-appliance-script.md) en lugar de la OVA descargada, los dos primeros pasos de este procedimiento no son pertinentes.

1. En la consola de cliente de vSphere, haga clic con el botón derecho en la máquina virtual y, luego, seleccione **Open Console** (Abrir consola).
2. Especifique el idioma, la zona horaria y la contraseña del dispositivo.
3. Abra un explorador en cualquier equipo que pueda conectarse a la máquina virtual y abra la dirección URL de la aplicación web del dispositivo: **https://*nombre o dirección IP del dispositivo*: 44368**.

   Como alternativa, puede abrir la aplicación desde el escritorio del dispositivo, para lo que debe seleccionar el acceso directo de la aplicación.
1. Acepte los **términos de licencia** y lea la información de terceros.
1. En la aplicación web > **Set up prerequisites** (Configurar los requisitos previos ), realice las siguientes operaciones:
   - **Connectivity** (Conectividad): la aplicación comprueba que la máquina virtual tiene acceso a Internet. Si la máquina virtual usa un proxy:
     - Haga clic en **Configurar el proxy** y especifique la dirección del proxy (con los formatos http://ProxyIPAddress o http://ProxyFQDN) ) y el puerto de escucha.
     - Especifique las credenciales si el proxy requiere autenticación.
     - Solo se admite un proxy HTTP.
     - Si ha agregado detalles del proxy o ha deshabilitado el proxy o la autenticación, haga clic en **Guardar** para desencadenar la comprobación de conectividad.
   - **Time sync** (Sincronización de hora): Para que la detección funcione correctamente, la hora del dispositivo debe estar sincronizada con la hora de Internet.
   - **Instalación de actualizaciones**: el dispositivo garantiza que se instalan las actualizaciones más recientes. Una vez finalizada la comprobación, puede hacer clic en **Ver servicios del dispositivo** para ver el estado y las versiones de los componentes que se ejecutan en el dispositivo.
   - **Install VDDK** (Instalar VDDK): El dispositivo comprueba si VMware vSphere Virtual Disk Development Kit (VDDK) está instalado. Si no está instalado, descargue VDDK 6.7 de VMware y extraiga el contenido del archivo ZIP descargado en la ubicación especificada del dispositivo, tal como se indica en las **instrucciones de instalación**.

     Server Migration de Azure Migrate usa VDDK para replicar las máquinas durante la migración a Azure. 
1. Si quiere, puede **volver a ejecutar los requisitos previos** en cualquier momento durante la configuración del dispositivo para comprobar si el dispositivo cumple todos los requisitos previos.

### <a name="register-the-appliance-with-azure-migrate"></a>Registro del dispositivo en Azure Migrate

1. Pegue la **clave del proyecto de Azure Migrate** copiada desde el portal. Si no tiene la clave, vaya a **Server Assessment > Detectar > Administrar los dispositivos existentes**, seleccione el nombre del dispositivo que proporcionó en el momento de la generación de la clave y copie la clave correspondiente.
1. Necesitará un código de dispositivo para autenticarse con Azure. Al hacer clic en **Iniciar sesión** se abrirá un modal con el código del dispositivo, tal como se muestra a continuación.

    ![Modal que muestra el código del dispositivo](./media/tutorial-discover-vmware/device-code.png)

1. Haga clic en **Copiar código e Iniciar sesión** para copiar el código del dispositivo y abrir un símbolo del sistema de inicio de sesión de Azure en una nueva pestaña del explorador. Si no aparece, asegúrese de que ha deshabilitado el bloqueador de elementos emergentes en el explorador.
1. En la nueva pestaña, pegue el código del dispositivo e inicie sesión con su nombre de usuario y contraseña de Azure.
   
   No se admite el inicio de sesión con un PIN.
3. En caso de que cierre accidentalmente la pestaña de inicio de sesión sin iniciar sesión, deberá actualizar la pestaña explorador del administrador de configuración del dispositivo para volver a habilitar el botón Iniciar sesión.
1. Una vez que haya iniciado sesión correctamente, vuelva a la pestaña anterior con el administrador de configuración del dispositivo.
1. Si la cuenta de usuario de Azure que se usa para el registro tiene los permisos adecuados en los recursos de Azure creados durante la generación de la clave, se iniciará el registro del dispositivo.
1. Una vez que el dispositivo se ha registrado correctamente, puede ver los detalles de registro haciendo clic en **Ver detalles**.



## <a name="start-continuous-discovery"></a>Inicio de detección continua

El dispositivo necesita conectarse a vCenter Server para detectar los datos de configuración y rendimiento de las máquinas virtuales.

1. En **Paso 1: Proporcionar las credenciales de vCenter Server**, haga clic en **Agregar credenciales** para especificar un nombre descriptivo para las credenciales, agregue un **nombre de usuario** y una **contraseña** para la cuenta de vCenter Server que utilizará el dispositivo para detectar máquinas virtuales en la instancia de vCenter Server.
    - Debe haber configurado una cuenta con los permisos necesarios en el tutorial anterior.
    - Si desea limitar el ámbito de la detección a objetos específicos de VMware (centros de datos de vCenter Server, clústeres, una carpeta de clústeres, hosts, una carpeta de hosts o máquinas virtuales individuales), consulte las instrucciones de [este artículo](set-discovery-scope.md) para restringir la cuenta usada por Azure Migrate.
1. En el **Paso 2: Proporcionar los detalles de vCenter Server**, haga clic en **Agregar origen de detección** para seleccionar el nombre descriptivo de las credenciales en la lista desplegable, especifique la **dirección IP o el FQDN** de la instancia de vCenter Server. Puede dejar el **puerto** en el valor predeterminado (443) o especificar un puerto personalizado en el que vCenter Server escuche y hacer clic en **Guardar**.
1. Al hacer clic en Guardar, el dispositivo intentará validar la conexión a vCenter Server con las credenciales proporcionadas y mostrar el **estado de validación** en la tabla con la dirección IP o el FQDN de vCenter Server.
1. Puede **volver a validar** la conectividad a vCenter Server en cualquier momento antes de iniciar la detección.
1. En **Paso 3: Proporcionar las credenciales de la máquina virtual para detectar las aplicaciones instaladas y para realizar la asignación de dependencias sin agente**, haga clic en **Agregar credenciales** y especifique el sistema operativo para el que se proporcionan las credenciales, el nombre descriptivo de las credenciales y el **nombre de usuario** y la **contraseña**. A continuación, haga clic en **Guardar**.

    - Si lo desea, agregue las credenciales aquí si ha creado una cuenta para utilizarla con la [detección de aplicaciones](how-to-discover-applications.md) o el [análisis de dependencias sin agente](how-to-create-group-machine-dependencies-agentless.md).
    - Si no quiere usar estas características, puede hacer clic en el control deslizante para omitir el paso. Puede invertir la intención en cualquier momento más tarde.
    - Revise los permisos necesarios en la cuenta para la [detección de aplicaciones](migrate-support-matrix-vmware.md#application-discovery-requirements) o el [análisis de dependencia sin agente](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless).

5. Haga clic en **Iniciar detección** para iniciar la detección de máquinas virtuales. Una vez que la detección se ha iniciado correctamente, puede comprobar el estado de detección de la dirección IP o el FQDN de vCenter en la tabla.

La detección funciona como se indica a continuación:
- Los metadatos de las máquinas virtuales detectadas tardan unos 15 minutos en aparecer en el portal.
- La detección de las aplicaciones, los roles y las características instaladas tarda un rato. Todo depende del número de máquinas virtuales que se detectan. En el caso de 500 máquinas virtuales, el inventario de la aplicación tarda aproximadamente una hora en aparecer en el portal de Azure Migrate.
- Una vez completada la detección de máquinas virtuales, puede habilitar el análisis de dependencia sin agente en las máquinas virtuales deseadas desde el portal.


## <a name="next-steps"></a>Pasos siguientes

- [Evalúe las máquinas virtuales de VMware](./tutorial-assess-vmware-azure-vm.md) para la migración a máquinas virtuales de Azure.
- [Revise los datos](migrate-appliance.md#collected-data---vmware) que el dispositivo recopila durante la detección.