---
title: Instalación de Defender para IoT
description: Obtenga información sobre cómo instalar un sensor y la consola de administración local para Azure Defender para IoT.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/2/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 49a0129ff26d4a12392066aa6304317d71fdb0f1
ms.sourcegitcommit: 08458f722d77b273fbb6b24a0a7476a5ac8b22e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2021
ms.locfileid: "98247597"
---
# <a name="defender-for-iot-installation"></a>Instalación de Defender para IoT

En este artículo se describe cómo instalar los siguientes elementos de Azure Defender para IoT:

- **Sensor**: los sensores de Defender para IoT recopilan el tráfico de ICS mediante la supervisión pasiva (sin agente). Los sensores, pasivos y no invasivos, tienen un impacto nulo sobre el rendimiento en las redes y los dispositivos de OT e IoT. El sensor se conecta a un puerto SPAN o a un TAP de red e inicia inmediatamente la supervisión de la red. Las detecciones se ven en la consola del sensor. Allí puede verlas, investigarlas y analizarlas en un mapa de red, un inventario de dispositivos y una amplia variedad de informes. Algunos ejemplos son los informes de evaluación de riesgos, las consultas de minería de datos y los vectores de ataque. Obtenga más información sobre las funcionalidades del sensor en la [Guía de usuario del sensor de Defender para IoT (descarga directa)](./getting-started.md).

- **Consola de administración local**: la consola de administración local le permite llevar a cabo la administración de dispositivos, riesgos y vulnerabilidades. También puede usarla para llevar a cabo la supervisión de amenazas y la respuesta a incidentes en toda la empresa. Ofrece una vista unificada de todos los dispositivos de red e indicadores de riesgo y alertas clave de OT e IoT detectados en las instalaciones en las que los sensores están implementados. Use la consola de administración local para ver y administrar sensores en redes aisladas.

En este artículo se trata la siguiente información sobre la instalación:

  - **Hardware:** detalles de los dispositivos físicos Dell y HPE.

  - **Software:** instalación del software de la consola de administración local y del sensor.

  - **Aplicaciones virtuales:** detalles sobre la máquina virtual e instalación del software.

Después de la instalación, conecte el sensor a la red.

## <a name="about-defender-for-iot-appliances"></a>Acerca de los dispositivos de Defender para IoT 

En las secciones siguientes se proporciona información sobre los dispositivos de sensor de Defender para IoT y sobre el dispositivo para la consola de administración local de Defender para IoT.

### <a name="physical-appliances"></a>Dispositivos físicos

El sensor de dispositivo de Azure Defender para IoT se conecta a un puerto SPAN o a un TAP de red e inmediatamente comienza a recopilar el tráfico de ICS mediante supervisión pasiva (sin agente). Este proceso no tiene ningún impacto en las redes y dispositivos OT, porque no se coloca en la ruta de acceso de datos y no examina de manera activa los dispositivos OT.

Están disponibles los siguientes dispositivos de montaje en bastidor:

| **Tipo de implementación** | **Corporativos** | **Empresa** | **SMB** |  |
|--|--|--|--|--|
| **Modelo** | HPE ProLiant DL360 | Dell PowerEdge R340 XL | HPE ProLiant DL20 | HPE ProLiant DL20 |
| **Puertos de supervisión** | hasta 15 RJ45 u 8 OPT | hasta 9 RJ45 o 6 OPT | hasta 8 RJ45 o 6 OPT | 4 RJ45 |
| **Ancho de banda máximo\** _ | 3 GB/s | 1 GB/s | 1 GB/s | 100 MB/s |
| _ *Número máximo de dispositivos protegidos** | 30,000 | 10 000 | 15,000 | 1,000 |

*La capacidad de ancho de banda máxima puede variar en función de la distribución de protocolos.

### <a name="virtual-appliances"></a>Dispositivos virtuales

Están disponibles las siguientes aplicaciones virtuales:

| **Tipo de implementación** | **Empresa** | **SMB** | **Line** |
|--|--|--|--|
| **Descripción** | Aplicación virtual para implementaciones Enterprise | Aplicación virtual para implementaciones de SMB | Aplicación virtual para implementaciones en línea |
| **Ancho de banda máximo\** _ | 150 MB/s | 15 MB/s | 3 MB/s |
| _ *Número máximo de dispositivos protegidos** | 3,000 | 300 | 100 |
| **Tipo de implementación** | Enterprise | SMB | Línea |
| **Descripción** | Aplicación virtual para implementaciones Enterprise | Aplicación virtual para implementaciones de SMB | Aplicación virtual para implementaciones en línea |

*La capacidad de ancho de banda máxima puede variar en función de la distribución de protocolos.

### <a name="hardware-specifications-for-the-on-premises-management-console"></a>Especificaciones de hardware para la consola de administración local

 | Elemento | Descripción |
 |----|--|
 **Descripción** | En una arquitectura de varios niveles, la consola de administración local ofrece visibilidad y control en todos los sitios distribuidos geográficamente. Se integra con pilas de seguridad de SOC, entre las que se incluyen SIEM, sistemas de vales, firewalls de última generación, plataformas de acceso remoto seguras y el espacio aislado de malware de ICS para Defender para IoT. |
 **Tipo de implementación** | Enterprise |
 **Tipo de dispositivo**  | Dell R340, máquina virtual |
 **Número de sensores administrados** | Sin límite |

## <a name="prepare-for-the-installation"></a>Preparación de la instalación

### <a name="access-the-iso-installation-image"></a>Acceso a la imagen de instalación ISO

Se puede acceder a la imagen de instalación desde el portal de Defender para IoT.

Para acceder al archivo, haga lo siguiente:

1. Inicie sesión en su cuenta de Defender para IoT.

2. Vaya a la página **Sensor de red** o **Consola de administración local** y seleccione la versión que quiere descargar.

### <a name="install-from-dvd"></a>Instalación desde DVD

Antes de la instalación, asegúrese de tener lo siguiente:

- Una unidad de DVD portátil con conexión mediante USB.

- Una imagen de instalador ISO.

Para instalarlo:

1. Grabe la imagen en un DVD o prepare un disco en una memoria USB. Conecte una unidad de DVD portátil al equipo, haga clic con el botón secundario en la imagen ISO y seleccione **Grabar en el disco**.

1. Conecte el DVD o el disco en una memoria USB y configure el dispositivo para que arranque desde un DVD o memoria USB.

### <a name="install-from-disk-on-a-key"></a>Instalación desde un disco en memoria USB

Antes de la instalación, asegúrese de tener lo siguiente:

  - Rufus instalado.
  
  - Un disco en una memoria USB versión 3.0 de USB y posteriores. El tamaño mínimo es 4 GB.

  - Un archivo de imagen de instalador ISO.

El disco en memoria USB se borrará en este proceso.

Para preparar un disco en memoria USB:

1. Ejecute Rufus y seleccione **SENSOR ISO**.

2. Conecte el disco en una memoria USB al panel frontal.

3. Configure el BIOS del servidor para que arranque desde la USB.

## <a name="dell-poweredger340xl-installation"></a>Instalación de Dell PowerEdgeR340XL

Antes de instalar el software en el dispositivo Dell, debe cambiar la configuración del BIOS del dispositivo:

  - El [panel frontal de Dell PowerEdge R340](#dell-poweredge-r340-front-panel) y el [panel posterior de Dell PowerEdge R340](#dell-poweredge-r340-back-panel) contiene la descripción de los paneles frontal y posterior, junto con la información necesaria para la instalación, como los controladores y los puertos.

  - La [configuración de BIOS de Dell](#dell-bios-configuration) proporciona información acerca de cómo conectarse a la interfaz de administración de dispositivos de Dell y configurar el BIOS.

  - [Instalación de software (Dell R340)](#software-installation-dell-r340) describe el procedimiento necesario para instalar el software del sensor de Defender para IoT.

### <a name="dell-poweredge-r340xl-requirements"></a>Requisitos de Dell PowerEdge R340XL 

Para instalar el dispositivo Dell PowerEdge R340XL, necesitará lo siguiente:

- Licencia Enterprise para el controlador integrado de acceso remoto de Dell (iDrac)

- Archivo XML de configuración del BIOS

- Versiones de firmware del servidor:

  - Versión del BIOS 2.1.6

  - Versión de iDrac 3.23.23.23

### <a name="dell-poweredge-r340-front-panel"></a>Panel frontal de Dell PowerEdge R340

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-front-panel.jpg" alt-text="Panel frontal de Dell PowerEdge R340.":::

 1. Panel de control izquierdo 
 2. Unidad óptica (opcional) 
 3. Panel de control derecho 
 4. Etiqueta de información 
 5. Unidades  

### <a name="dell-poweredge-r340-back-panel"></a>Panel posterior de Dell PowerEdge R340

:::image type="content" source="media/tutorial-install-components/view-of-dell-poweredge-r340-back-panel.jpg" alt-text="Panel posterior de Dell PowerEdge R340.":::

1. Puerto serie 
2. Puerto de adaptador de red (Gb 1) 
3. Puerto de adaptador de red (Gb 1) 
4. PCIe de media altura 
5. Ranura de tarjeta de expansión PCIe de altura completa 
6. Unidad de fuente de alimentación 1 
7. Unidad de fuente de alimentación 2 
8. Identificación del sistema 
9. Botón de puerto del cable indicador de estado del sistema (CMA) 
10. Puerto USB 3.0 (2) 
11. Puerto de red dedicado iDRAC9 
12. Puerto VGA 

### <a name="dell-bios-configuration"></a>Configuración del BIOS de Dell

La configuración del BIOS de Dell es necesaria para modificar el dispositivo Dell para que funcione con el software.

La configuración del BIOS se lleva a cabo a través de una configuración predefinida. Se puede acceder al archivo desde el [Centro de ayuda](https://help.cyberx-labs.com/).

Importe el archivo de configuración al dispositivo Dell. Antes de usar el archivo de configuración, debe establecer la comunicación entre el dispositivo de Dell y el equipo de administración.

Un iDRAC integrado con el controlador del ciclo de vida (LC) administra el dispositivo Dell. El LC está integrado en todos los servidores Dell PowerEdge y proporciona una funcionalidad que le ayuda a implementar, actualizar, supervisar y dar mantenimiento a sus dispositivos Dell PowerEdge.

Para establecer la comunicación entre el dispositivo Dell y el equipo de administración, debe definir la dirección IP de iDRAC y la dirección IP del equipo de administración en la misma subred.

Cuando se establece la conexión, el BIOS se puede configurar.

Para configurar el BIOS de Dell, realice lo siguiente:

1. [Configuración de las direcciones IP de iDRAC](#configure-idrac-ip-address)

2. [Importación del archivo de configuración del BIOS](#import-the-bios-configuration-file)

#### <a name="configure-idrac-ip-address"></a>Configuración de las direcciones IP de iDRAC

1. Encienda el sensor.

2. Si el sistema operativo ya está instalado, presione la tecla F2 para especificar la configuración del BIOS.

3. Seleccione **iDRAC Settings** (Configuración de iDRAC).

4. Seleccione **Network** (Red).

   > [!NOTE]
   > Durante la instalación, debe configurar la dirección IP y la contraseña de iDRAC predeterminadas que se mencionan en los siguientes pasos. Después de la instalación, deberá cambiar estas definiciones.

5. Cambie la dirección IPv4 estática en **10.100.100.250**.

6. Cambie la máscara de subred estática en **255.255.255.0**.

   :::image type="content" source="media/tutorial-install-components/idrac-network-settings-screen-v2.png" alt-text="Captura de pantalla que muestra la máscara de subred estática":::.

7. Seleccione **Volver** > **Finalizar**.

#### <a name="import-the-bios-configuration-file"></a>Importación del archivo de configuración del BIOS

En este artículo se explica cómo configurar el BIOS mediante el archivo de configuración.

1. Conecte un equipo con una dirección IP preconfigurada estática **10.100.100.200** al puerto de **iDRAC**.

   :::image type="content" source="media/tutorial-install-components/idrac-port.png" alt-text="Captura de pantalla del puerto de dirección IP preconfigurada.":::

2. Abra un explorador web y escriba **10.100.100.250** para conectarse a la interfaz web de iDRAC.

3. Inicie sesión con los privilegios de administrador predeterminados de Dell:

   - Nombre de usuario: **root**

   - Contraseña: **calvin**

4. Las credenciales del dispositivo son:

   - Nombre de usuario: **XXX**

   - Contraseña: **XXX**

     Se inicia la operación de importación del perfil de servidor.

     > [!NOTE]
     > Antes de importar el archivo, asegúrese de cumplir con lo siguiente:
     > - Es el único usuario que está conectado actualmente a iDRAC.
     > - El sistema no está en el menú del BIOS.

5. Diríjase a **Configuration** > **Server Configuration Profile** (Configuración > Perfil de configuración del servidor). Establezca los siguientes parámetros:

   :::image type="content" source="media/tutorial-install-components/configuration-screen.png" alt-text="Captura de pantalla que muestra la configuración del perfil de servidor.":::

   | Parámetro | Configuración |
   |--|--|
   | Tipo de ubicación | Seleccione **Local**. |
   | Ruta del archivo | Seleccione **Choose File** (Elegir archivo) y agregue el archivo XML de configuración. |
   | Componentes de importación | Seleccione **BIOS, NIC, RAID**. |
   | Tiempo máximo de espera | Seleccione **20 minutes** (20 minutos). |

6. Seleccione **Import** (Importar).

7. Para supervisar el proceso, diríjase a **Maintenance** > **Job Queue** (Mantenimiento > Cola de trabajos).

   :::image type="content" source="media/tutorial-install-components/view-the-job-queue.png" alt-text="Captura de pantalla que muestra la cola de trabajos":::.

#### <a name="manually-configuring-bios"></a>Configuración manual del BIOS 

Debe configurar manualmente el BIOS del dispositivo si se cumple lo siguiente:

- No compró el dispositivo en Arrow.

- Tiene un dispositivo, pero tiene acceso al archivo de configuración XML.

Después de acceder al BIOS, diríjase a **Device Settings** (Configuración del dispositivo).

Para realizar la configuración manualmente:

1. Acceda al BIOS del dispositivo directamente con un teclado y una pantalla, o bien use iDRAC.

   - Si no se trata de un dispositivo de Defender para IoT, abra un explorador web y vaya a la dirección IP que se configuró antes. Inicie sesión con los privilegios de administrador predeterminados de Dell. Use **root** como nombre de usuario y **calvin** como contraseña.

   - Si se trata de un dispositivo de Defender para IoT, inicie sesión con **XXX** como nombre de usuario y **XXX** como contraseña.

2. Después de acceder al BIOS, diríjase a **Device Settings** (Configuración del dispositivo).

3. Para elegir la configuración controlada por RAID, seleccione **Integrated RAID controller 1: Dell PERC\<PERC H330 Adapter\> Configuration Utility** (Controlador RAID integrado 1: utilidad de configuración de Dell PERC <adaptador PERC H330>).

4. Seleccione **Configuration Management** (administración de configuración).

5. Seleccione **Create Virtual Disk** (crear disco virtual).

6. En el campo **Select RAID Level** (Seleccionar nivel de RAID), seleccione **RAID5**. En el campo **Virtual Disk Name** (nombre de disco virtual), escriba **ROOT** y seleccione **Physical Disks** (discos físicos).

7. Seleccione **Check All** (comprobar todos) y después seleccione **Apply Changes** (aplicar cambios).

8. Seleccione **Aceptar**.

9. Desplácese hacia abajo y seleccione **Create Virtual Disk** (crear disco virtual).

10. Marque la casilla **Confirm** (confirmar) y, a continuación, seleccione **Yes** (sí).

11. Seleccione **Aceptar**.

12. Vuelva a la pantalla principal y seleccione **System BIOS** (BIOS del sistema).

13. Seleccione **Boot Settings** (configuración de arranque).

14. En la opción **Boot Mode** (modo de arranque), seleccione **BIOS**.

15. Seleccione **Back** (atrás) y, a continuación, seleccione **Finish** (finalizar) para salir de la configuración del BIOS.

### <a name="software-installation-dell-r340"></a>Instalación de software (Dell R340)

El proceso de instalación tarda aproximadamente 20 minutos. Después de la instalación, el sistema se reinicia varias veces.

Para instalarlo:

1. Compruebe que el soporte físico de la versión esté montado en el dispositivo de una de las siguientes maneras:

   - Conecte el CD o el disco en memoria USB con la versión.

   - Monte la imagen ISO mediante iDRAC. Después de iniciar sesión en iDRAC, seleccione la consola virtual y, a continuación, seleccione **Virtual Media** (soporte físico virtual).

2. En la sección **Map CD/DVD** (asignar CD/DVD), seleccione **Choose File** (elegir archivo).

3. Elija el archivo de imagen ISO correspondiente a esta versión en el cuadro de diálogo que se abrirá.

4. Seleccione el botón **Map Device** (asignar dispositivo).

   :::image type="content" source="media/tutorial-install-components/mapped-device-on-virtual-media-screen-v2.png" alt-text="Captura de pantalla que muestra u dispositivo asignado.":::

5. El soporte físico está montado. Seleccione **Cerrar**.

6. Inicie el dispositivo. Cuando use iDRAC, puede reiniciar los servidores al seleccionar el botón **Consul Control** (control de Consul). A continuación, en **Keyboard Macros** (macros de teclado), seleccione el botón **Apply** (aplicar) que iniciará la secuencia de teclas Ctrl+Alt+Suprimir.

7. Seleccione **English** (inglés).

8. Seleccione **SENSOR-RELEASE-\<version\> Enterprise**.

   :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Captura de pantalla que muestra la selección de versión.":::   

9. Defina el perfil de la aplicación y las propiedades de la red:

   :::image type="content" source="media/tutorial-install-components/appliance-profile-screen-v2.png" alt-text="Captura de pantalla que muestra el perfil del dispositivo.":::   

   | Parámetro | Configuración |
   |--|--|
   | **Perfil de hardware** | **enterprise** |
   | **Interfaz de administración** | **eno1** |
   | **Parámetros de red (proporcionados por el cliente)** | - |
   |**dirección IP de la red de administración:** | - |
   | **máscara de subred:** | - |
   | **nombre de host del dispositivo:** | - |
   | **DNS:** | - |
   | **dirección IP de la puerta de enlace predeterminada:** | - |
   | **interfaces de entrada:** |  El sistema genera la lista de interfaces de entrada automáticamente. Para reflejar las interfaces de entrada, copie todos los elementos que se muestran en la lista separados con comas. Tenga en cuenta que no es necesario configurar la interfaz de puente. Esta opción solo se usa para casos de uso especiales. |

10. Después de unos diez minutos, se muestran los dos conjuntos de credenciales. Uno es para un usuario **CyberX** y otro es para un usuario **Support**.  

11. Guarde el id. de dispositivo y las contraseñas. Necesitará estas credenciales para acceder a la plataforma la primera vez que la use.

12. Seleccione **Entrar** para continuar.

## <a name="hpe-proliant-dl20-installation"></a>Instalación de HPE ProLiant DL20

En este artículo se describe el proceso de instalación de HPE ProLiant DL20, que incluye los pasos siguientes:

  - Habilite el acceso remoto y actualice la contraseña de administrador predeterminada.
  - Configure el BIOS y las opciones de RAID.
  - Instale el software.

### <a name="about-the-installation"></a>Acerca de la instalación

  - Se pueden instalar aplicaciones de Enterprise y SMB. El proceso de instalación es idéntico para ambos tipos de dispositivo, excepto para la configuración de matriz.
  - Se especifica un usuario administrativo predeterminado. Se recomienda cambiar la contraseña durante el proceso de configuración de la red.
  - Durante el proceso de configuración de la red, configurará el puerto iLO en el puerto de red 1.
  - El proceso de instalación tarda aproximadamente 20 minutos. Después de la instalación, el sistema se reinicia varias veces.

### <a name="hpe-proliant-dl20-front-panel"></a>Panel frontal del HPE ProLiant DL20

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-front-panel-v2.png" alt-text="Panel frontal del HPE ProLiant DL20.":::

### <a name="hpe-proliant-dl20-back-panel"></a>Panel posterior del HPE ProLiant DL20

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl20-back-panel-v2.png" alt-text="Panel posterior del HPE ProLiant DL20.":::

### <a name="enable-remote-access-and-update-the-password"></a>Habilitación del acceso remoto y actualización de la contraseña

Use el siguiente procedimiento para configurar las opciones de red y actualizar la contraseña predeterminada.

Para habilitar y actualizar la contraseña, haga lo siguiente:

1. Conecte una pantalla y un teclado al dispositivo HP, enciéndalo y presione **F9**.

    :::image type="content" source="media/tutorial-install-components/hpe-proliant-screen-v2.png" alt-text="Captura de pantalla en la que se muestra la ventana HPE ProLiant.":::

2. Diríjase a **System Utilities** > **System Configuration** > **iLO 5 Configuration Utility** > **Network Options** (Utilidades del sistema > Configuración del sistema > Utilidad de configuración iLO 5 > Opciones de red).

    :::image type="content" source="media/tutorial-install-components/system-configuration-window-v2.png" alt-text="Captura de pantalla que muestra la ventana Configuración del sistema.":::

    1.  Seleccione **Shared Network Port-LOM** (puerto de red compartido-LOM) en el campo **Network Interface Adapter** (adaptador de interfaz de red).
    
    1.  Deshabilite el DHCP.
    
    1.  Escriba la dirección IP, la máscara de subred y la dirección IP de puerta de enlace.

3. Seleccione **F10: Save** (F10: guardar).

4. Seleccione **Esc** para volver a la **utilidad de configuración iLO 5** y después seleccione **User Management** (administración de usuarios).

5. Seleccione **Edit/Remove User** (editar/quitar usuario). El administrador es el único usuario predeterminado definido. 

6. Cambie la contraseña predeterminada y seleccione **F10: Save** (F10: guardar).

### <a name="configure-the-hpe-bios"></a>Configuración del BIOS de HPE

En el procedimiento siguiente se describe cómo configurar el BIOS de HPE para las aplicaciones de Enterprise y SMB.

Para configurar el BIOS de HPE, haga lo siguiente:

1. Seleccione **System Utilities** > **System Configuration** > **BIOS/Platform Configuration (RBSU)** (utilidades del sistema > configuración del sistema > configuración de la plataforma/BIOS [RBSU]).

2. En el formulario **BIOS/Platform Configuration (RBSU)** (configuración de la plataforma/BIOS [RBSU]), seleccione **Boot Options** (opciones de arranque).

3. Cambie el **Boot Mode** (modo de arranque) en **Legacy BIOS Mode** (modo de BIOS heredado) y después seleccione **F10: Save** (F10: guardar).

4. Seleccione **Esc** dos veces para cerrar el formulario **System Configuration** (configuración del sistema).

#### <a name="for-the-enterprise-appliance"></a>Para el dispositivo Enterprise, haga lo siguiente

1. Seleccione **Embedded RAID 1: HPE Smart Array P408i-a SR Gen 10** > **Array Configuration** > **Create Array** (RAID integrado 1: HPE Smart Array P408i-a SR Gen 10 > configuración de matriz > crear matriz).

2. En el formulario **Create Array** (crear matriz), seleccione todas las opciones. Hay tres opciones disponibles para el dispositivo **Enterprise**.

#### <a name="for-the-smb-appliance"></a>Para el dispositivo SMB, haga lo siguiente

1. Seleccione **Embedded RAID 1: HPE Smart Array P208i-a SR Gen 10** > **Array Configuration** > **Create Array** (RAID incrustado 1: HPE Smart Array P208i-a SR Gen 10 > configuración de matriz > crear matriz).

2. Seleccione **Proceed to Next Form** (continuar con el siguiente formulario).

3. En el formulario **Set RAID Level** (establecer nivel de RAID), establezca el nivel en **RAID 5** para las implementaciones Enterprise y **RAID 1** para las implementaciones de SMB.

4. Seleccione **Proceed to Next Form** (continuar con el siguiente formulario).

5. En el formulario **Logical Drive Label** (etiqueta de unidad lógica) escriba **Logical Drive 1**.

6. Seleccione **Submit Changes** (enviar cambios).

7. En el formulario **Submit** (enviar), seleccione **Back to Main Menu** (volver al menú principal).

8. Seleccione **F10: Save** (F10: guardar) y después presione **Esc** dos veces.

9. En la ventana **System Utilities** (utilidades del sistema), seleccione **One-Time Boot Menu** (menú de arranque único).

10. En el formulario **One-Time Boot Menu** (menú de arranque único), seleccione **Legacy BIOS One-Time Boot Menu** (menú de arranque único del BIOS heredado).

11. Aparecen las ventanas **Booting in Legacy** (arranque en modo heredado) y **Boot Override** (invalidación de arranque). Elija una opción de invalidación de arranque; por ejemplo, en un shell de CD-ROM, USB, HDD o UEFI.

    :::image type="content" source="media/tutorial-install-components/boot-override-window-one-v2.png" alt-text="Captura de pantalla que muestra la primera ventana de invalidación de arranque.":::

    :::image type="content" source="media/tutorial-install-components/boot-override-window-two-v2.png" alt-text="Captura de pantalla que muestra la segunda ventana de invalidación de arranque.":::
### <a name="software-installation-hpe-proliant-dl20-appliance"></a>Instalación de software (dispositivo HPE ProLiant DL20)

El proceso de instalación tarda aproximadamente 20 minutos. Después de la instalación, el sistema se reinicia varias veces.

Para instalar el software, haga lo siguiente:

1. Conecte la pantalla y el teclado al dispositivo y, a continuación, conéctese a la CLI.

2. Conecte un CD externo o disco en memoria USB con la imagen ISO que descargó de la página **Actualizaciones** en el portal de Defender para IoT.

3. Inicie el dispositivo.

4. Seleccione **English** (inglés).

    :::image type="content" source="media/tutorial-install-components/select-english-screen.png" alt-text="Selección de inglés en la ventana de la CLI.":::

5. Seleccione **SENSOR-RELEASE-<version> Enterprise**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Captura de pantalla de la pantalla para seleccionar una versión.":::

6. En el Asistente de instalación, defina el perfil de dispositivo y las propiedades de la red:

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Captura de pantalla que muestra al Asistente de instalación.":::

    | Parámetro | Configuración |
    | ----------| ------------- |
    | **Perfil de hardware** | Seleccione **Enterprise** u **Office** para las implementaciones de SMB. |
    | **Interfaz de administración** | **eno2** |
    | **Parámetros de red predeterminados (normalmente, el cliente especifica los parámetros)** | **dirección IP de la red de administración:** <br/> <br/>**nombre de host del dispositivo:** <br/>**DNS:** <br/>**la dirección IP de puerta de enlace predeterminada:**|
    | **interfaces de entrada:** | El sistema genera la lista de interfaces de entrada automáticamente.<br/><br/>Para reflejar las interfaces de entrada, copie todos los elementos que se muestran en la lista separados con comas: **eno5, eno3, eno1, eno6, eno4**.<br/><br/>**Para el HPE DL20: no incluya eno1, enp1s0f4u4 (interfaces iLO)**<br/><br/>**PUENTE**: no es necesario configurar la interfaz de puente. Esta opción solo se usa para casos de uso especiales. Presione **Entrar** para continuar. |

7. Después de unos diez minutos, se muestran los dos conjuntos de credenciales. Uno es para un usuario **CyberX** y otro es para un usuario **Support**.

8. Guarde el id. de dispositivo y las contraseñas. Necesitará las credenciales para acceder a la plataforma la primera vez.

9. Seleccione **Entrar** para continuar.

## <a name="hpe-proliant-dl360-installation"></a>Instalación de HPE ProLiant DL360

  - Se especifica un usuario administrativo predeterminado. Se recomienda cambiar la contraseña durante la configuración de la red.

  - Durante la configuración de la red, configurará el puerto iLO.

  - El proceso de instalación tarda aproximadamente 20 minutos. Después de la instalación, el sistema se reinicia varias veces.

### <a name="hpe-proliant-dl360-front-panel"></a>Panel frontal del HPE ProLiant DL360

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-front-panel.png" alt-text="Panel frontal del HPE ProLiant DL360.":::

### <a name="hpe-proliant-dl360-back-panel"></a>Panel posterior del HPE ProLiant DL360

:::image type="content" source="media/tutorial-install-components/hpe-proliant-dl360-back-panel.png" alt-text="Panel posterior del HPE ProLiant DL360.":::

### <a name="enable-remote-access-and-update-the-password"></a>Habilitación del acceso remoto y actualización de la contraseña

Consulte las secciones anteriores para saber sobre la instalación de HPE ProLiant DL20:

  - "Habilitación del acceso remoto y actualización de la contraseña"

  - "Configuración del BIOS de HPE"

La configuración de Enterprise es idéntica.

> [!Note]
> En el formulario de matriz, compruebe que haya seleccionado todas las opciones.

### <a name="ilo-remote-installation-from-a-virtual-drive"></a>Instalación remota de iLO (desde una unidad virtual)

En este procedimiento se describe la instalación de iLO desde una unidad virtual.

Para instalarlo:

1. Inicie sesión en la consola de iLO y, a continuación, haga clic con el botón secundario en la pantalla de servidores.

2. Seleccione **HTML5 Console** (consola HTML5).

3. En la consola, seleccione el icono de CD y elija la opción CD/DVD.

4. Seleccione **Local ISO file** (archivo ISO local).

5. En el cuadro de diálogo, elija el archivo ISO pertinente.

6. Vaya al icono de la izquierda, seleccione **Power** (inicio/apagado) y seleccione **Reset** (restablecer).

7. El dispositivo se reiniciará y ejecutará el proceso de instalación del sensor.

### <a name="software-installation-hpe-dl360"></a>Instalación de software (HPE DL360)

El proceso de instalación tarda aproximadamente 20 minutos. Después de la instalación, el sistema se reinicia varias veces.

Para instalarlo:

1. Conecte la pantalla y el teclado al dispositivo y, a continuación, conéctese a la CLI.

2. Conecte un CD externo o disco en memoria USB con la imagen ISO que descargó de la página **Actualizaciones** en el portal de Defender para IoT.

3. Inicie el dispositivo.

4. Seleccione **English** (inglés).

5. Seleccione **SENSOR-RELEASE-<version> Enterprise**.

    :::image type="content" source="media/tutorial-install-components/sensor-version-select-screen-v2.png" alt-text="Captura de pantalla que muestra la selección de la versión.":::

6. En el Asistente de instalación, defina el perfil de dispositivo y las propiedades de la red.

    :::image type="content" source="media/tutorial-install-components/installation-wizard-screen-v2.png" alt-text="Captura de pantalla que muestra al Asistente de instalación.":::

    | Parámetro | Configuración |
    | ----------| ------------- |
    | **Perfil de hardware** | Seleccione **corporate**. |
    | **Interfaz de administración** | **eno2** |
    | **Parámetros de red predeterminados (proporcionados por el cliente)** | **dirección IP de la red de administración:** <br/>**máscara de subred:** <br/>**nombre de host del dispositivo:** <br/>**DNS:** <br/>**la dirección IP de puerta de enlace predeterminada:**|
    | **interfaces de entrada:**  | El sistema genera una lista de interfaces de entrada automáticamente.<br/><br/>Para reflejar las interfaces de entrada, copie todos los elementos que se muestran en la lista separados con comas.<br/><br/>Tenga en cuenta que no es necesario configurar la interfaz de puente. Esta opción solo se usa para casos de uso especiales. |

7. Después de unos diez minutos, se muestran los dos conjuntos de credenciales. Uno es para un usuario **CyberX** y otro es para un usuario **support**.

8. Guarde el id. de dispositivo y las contraseñas. Necesitará estas credenciales para acceder a la plataforma la primera vez.

9. Seleccione **Entrar** para continuar.

## <a name="sensor-installation-for-the-virtual-appliance"></a>Instalación del sensor para el aplicación virtual

Puede implementar la máquina virtual para el sensor de Defender para IoT en las siguientes arquitecturas:


| Architecture | Especificaciones | Uso | Comentarios |
|---|---|---|---|
| **Empresa** | CPU: 8<br/>Memoria: 32 GB DE RAM<br/>HDD: 1800 GB | Entorno de producción | Valor predeterminado y más común |
| **Pequeñas empresas** | CPU: 4 <br/>Memoria: 8 GB RAM<br/>HDD: 500 GB | Entornos de producción pequeños o de pruebas | -  |
| **Office** | CPU: 4<br/>Memoria: 8 GB RAM<br/>HDD: 100 GB | Entornos de prueba pequeños | -  |

### <a name="prerequisites"></a>Requisitos previos

La consola de administración local admite las opciones de implementación de Hyper-V y VMware. Antes de iniciar la instalación, asegúrese de disponer de los siguientes elementos:

  - VMware (ESXi 5.5 o posterior) o el hipervisor de Hyper-V (Windows 10 Pro o Enterprise) instalados y en funcionamiento

  - Recursos de hardware disponibles para la máquina virtual

  - Archivo de instalación ISO para el sensor de Azure Defender para IoT

Asegúrese de que el hipervisor esté en ejecución.

### <a name="create-the-virtual-machine-esxi"></a>Creación de la máquina virtual (ESXi)

1. Inicie sesión en la máquina ESXi, elija el **almacén de datos** pertinente y seleccione **Explorador del almacén de datos**.

2. **Cargue** la imagen y seleccione **Cerrar**.

3. Vaya a **Virtual Machines** (máquinas virtuales) y, a continuación, seleccione **Create/Register VM** (crear/registrar máquina virtual).

4. Seleccione **Create new virtual machine** (crear nueva máquina virtual) y después seleccione **Next** (siguiente).

5. Agregue un nombre de sensor y elija lo siguiente:

   - Compatibilidad: **&lt;última versión de ESXi&gt;**

   - Familia del SO invitado: **Linux**

   - Versión del SO invitado: **Ubuntu Linux (solo 64 bits)**

6. Seleccione **Next** (Siguiente).

7. Elija el almacén de datos pertinente y seleccione **Next** (siguiente).

8. Cambie los parámetros del hardware virtual según la arquitectura necesaria.

9. En **CD/DVD Drive 1** (unidad de CD/DVD 1), seleccione **Datastore ISO file** (archivo ISO de almacén de datos) y elija el archivo ISO que cargó anteriormente.

10. Seleccione **Siguiente** > **Finalizar**.

### <a name="create-the-virtual-machine-hyper-v"></a>Creación de la máquina virtual (Hyper-V)

En este procedimiento se describe cómo crear una máquina virtual mediante Hyper-V.

Para crear una máquina virtual, haga lo siguiente:

1. Cree un disco virtual en el administrador de Hyper-V.

2. Seleccione **format = VHDX**.

3. Seleccione **type = Dynamic Expanding**.

4. Escriba el nombre y la ubicación del VHD.

5. Escriba el tamaño requerido (según la arquitectura).   

6. Revise el resumen y seleccione **Finalizar**.

7. En el menú **Actions** (acciones), cree una nueva máquina virtual.

8. Escriba un nombre para la máquina virtual.

9. Seleccione **Specify Generation** > **Generation 1** (especificar generación > generación 1).

10. Especifique la asignación de memoria (de acuerdo con la arquitectura) y marque la casilla de la memoria dinámica.

11. Configure el adaptador de red según la topología de red del servidor.

12. Conecte el VHDX creado previamente a la máquina virtual.

13. Revise el resumen y seleccione **Finalizar**.

14. Haga clic con el botón secundario en la máquina virtual nueva y seleccione **Settings** (configuración).

15. Seleccione **Add Hardware** (agregar hardware) y agregue un nuevo adaptador de red.

16. Seleccione el conmutador virtual que se conectará a la red de administración del sensor.

17. Asignar recursos de CPU (según la arquitectura).

18. Conecte la imagen ISO de la consola de administración a una unidad de DVD virtual.

19. Inicie la máquina virtual.

20. En el menú **Actions** (acciones), seleccione **Connect** (conectar) para continuar con la instalación del software.

### <a name="software-installation-esxi-and-hyper-v"></a>Instalación del software (ESXi e Hyper-V)

En esta sección se describe la instalación del software de ESXi e Hyper-V.

Para instalarlo:

1. Abra la consola de la máquina virtual.

2. La máquina virtual se iniciará a partir de la imagen ISO y aparecerá la pantalla de selección de idioma. Seleccione **English** (inglés).

3. Seleccione la arquitectura requerida.

4. Defina el perfil de la aplicación y las propiedades de la red:

    | Parámetro | Configuración |
    | ----------| ------------- |
    | **Perfil de hardware** | &lt;arquitectura requerida&gt; |
    | **Interfaz de administración** | **ens192** |
    | **Parámetros de red (proporcionados por el cliente)** | **dirección IP de la red de administración:** <br/>**máscara de subred:** <br/>**nombre de host del dispositivo:** <br/>**DNS:** <br/>**puerta de enlace predeterminada:** <br/>**interfaces de entrada:**|
    | **interfaces de puente:** | no es necesario configurar la interfaz de puente. Esta opción solo es para casos de uso especiales. |

5. Escriba **Y** para aceptar la configuración.

6. Las credenciales de inicio de sesión se generan y presentan automáticamente. Copie el nombre de usuario y la contraseña en un lugar seguro, ya que se necesitarán para el inicio de sesión y administración.

   - **Support**: usuario administrativo para la administración de usuarios.

   - **CyberX**: equivalente de usuario raíz para acceder al dispositivo.

7. El dispositivo se reinicia.

8. Acceda a la consola de administración mediante la dirección IP configurada previamente: `https://ip_address`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Captura de pantalla que muestra el acceso a la consola de administración.":::

## <a name="virtual-appliance-on-premises-management-console-installation"></a>Aplicación virtual: instalación de la consola de administración local

La máquina virtual de la consola de administración local es compatible con las siguientes arquitecturas:

| Architecture | Especificaciones | Uso | 
|--|--|--|
| Enterprise <br/>(valor predeterminado y más común) | CPU: 8 <br/>Memoria: 32 GB DE RAM<br/> HDD: 1,8 TB | Entornos de producción de gran tamaño | 
| Enterprise | CPU: 4 <br/> Memoria: 8 GB RAM<br/> HDD: 500 GB | Entornos de producción de gran tamaño |
| Enterprise | CPU: 4 <br/>Memoria: 8 GB RAM <br/> HDD: 100 GB | Entornos de prueba pequeños | 
   
### <a name="prerequisites"></a>Requisitos previos

La consola de administración local admite las opciones de implementación de Hyper-V y VMware. Antes de empezar la instalación, compruebe lo siguiente:

- VMware (ESXi 5.5 o posterior) o el hipervisor de Hyper-V (Windows 10 Pro o Enterprise) están instalados y en funcionamiento.

- Los recursos de hardware están disponibles para la máquina virtual.

- Tiene el archivo de instalación ISO para la consola de administración local.
    
- El hipervisor está en ejecución.

### <a name="create-the-virtual-machine-esxi"></a>Creación de la máquina virtual (ESXi)

Para crear la máquina virtual (ESXi), haga lo siguiente:

1. Inicie sesión en la máquina ESXi, elija el **almacén de datos** pertinente y seleccione **Explorador del almacén de datos**.

2. Cargue la imagen y seleccione **Close** (cerrar).

3. Vaya a **Virtual Machines** (máquinas virtuales).

4. Seleccione **Create/Register VM** (crear/registrar máquina virtual).

5. Seleccione **Create new virtual machine** (crear nueva máquina virtual) y seleccione **Next** (siguiente).

6. Agregue un nombre de sensor y elija lo siguiente:

   - Compatibilidad: \<latest ESXi version>

   - Familia del SO invitado: Linux

   - Versión del SO invitado: Ubuntu Linux (solo 64 bits)

7. Seleccione **Next** (Siguiente).

8. Elija el almacén de datos pertinente y seleccione **Next** (siguiente).

9. Cambie los parámetros del hardware virtual según la arquitectura necesaria.

10. En **CD/DVD Drive 1** (unidad de CD/DVD 1), seleccione **Datastore ISO file** (archivo ISO de almacén de datos) y elija el archivo ISO que cargó anteriormente.

11. Seleccione **Siguiente** > **Finalizar**.

### <a name="create-the-virtual-machine-hyper-v"></a>Creación de la máquina virtual (Hyper-V)

Para crear una máquina virtual mediante Hyper-V, haga lo siguiente:

1. Cree un disco virtual en el administrador de Hyper-V.

2. Seleccione el formato **VHDX**.

3. Seleccione **Next** (Siguiente).

4. Seleccione el tipo **Dynamic expanding** (expansión dinámica).

5. Seleccione **Next** (Siguiente).

6. Escriba el nombre y la ubicación del VHD.

7. Seleccione **Next** (Siguiente).

8. Escriba el tamaño requerido (según la arquitectura).

9. Seleccione **Next** (Siguiente).

10. Revise el resumen y seleccione **Finalizar**.

11. En el menú **Actions** (acciones), cree una nueva máquina virtual.

12. Seleccione **Next** (Siguiente).

13. Escriba un nombre para la máquina virtual.

14. Seleccione **Next** (Siguiente).

15. Seleccione **Generation** (generaci+on) y establézcalo en **Generation 1**.

16. Seleccione **Next** (Siguiente).

17. Especifique la asignación de memoria (de acuerdo con la arquitectura) y marque la casilla de la memoria dinámica.

18. Seleccione **Next** (Siguiente).

19. Configure el adaptador de red según la topología de red del servidor.

20. Seleccione **Next** (Siguiente).

21. Conecte el VHDX creado previamente a la máquina virtual.

22. Seleccione **Next** (Siguiente).

23. Revise el resumen y seleccione **Finalizar**.

24. Haga clic con el botón secundario en la máquina virtual nueva y después seleccione **Settings** (configuración).

25. Seleccione **Add Hardware** (agregar hardware) y agregue un nuevo adaptador en **Network Adapter** (adaptador de red).

26. En **Virtual Switch** (conmutador virtual), seleccione el conmutador que se conectará a la red de administración del sensor.

27. Asignar recursos de CPU (según la arquitectura).

28. Conecte la imagen ISO de la consola de administración a una unidad de DVD virtual.

29. Inicie la máquina virtual.

30. En el menú **Actions** (acciones), seleccione **Connect** (conectar) para continuar con la instalación del software.

### <a name="software-installation-esxi-and-hyper-v"></a>Instalación del software (ESXi e Hyper-V)

Al iniciar la máquina virtual, se iniciará el proceso de instalación desde la imagen ISO.

Para instalar el software, haga lo siguiente:

1. Seleccione **English** (inglés).

2. Seleccione la arquitectura necesaria para la implementación.

3. Defina la interfaz de red para la red de administración del sensor: interfaz, IP, subred, servidor DNS y puerta de enlace predeterminada.

4. Las credenciales de inicio de sesión se generan y presentan automáticamente. Guarde estas credenciales en un lugar seguro, ya que se necesitarán para el inicio de sesión y administración.

  - **Support**: usuario administrativo para la administración de usuarios.

  - **CyberX**: equivalente de usuario raíz para acceder al dispositivo.

5. El dispositivo se reinicia.

6. Acceda a la consola de administración mediante la dirección IP configurada previamente: `<https://ip_address>`.

    :::image type="content" source="media/tutorial-install-components/defender-for-iot-management-console-sign-in-screen.png" alt-text="Captura de pantalla que muestra la pantalla de inicio de sesión de la consola de administración.":::

## <a name="post-installation-validation"></a>Validación después de la instalación

Para validar la instalación de un dispositivo físico, debe realizar una serie de pruebas. El mismo proceso de validación se realiza a todos los tipos de dispositivos.

Realice dicha validación mediante la GUI o la CLI. La validación está disponible para los usuarios **Support** y **CyberX**.

La validación después de la instalación debe incluir las siguientes pruebas:

  - **Prueba de comprobación**: compruebe que el sistema se está ejecutando.

  - **Versión**: compruebe que la versión sea correcta.

  - **ifconfig**: compruebe que todas las interfaces de entrada configuradas durante el proceso de instalación se estén ejecutando.

### <a name="checking-system-health-by-using-the-gui"></a>Comprobación del mantenimiento del sistema mediante la GUI

:::image type="content" source="media/tutorial-install-components/system-health-check-screen.png" alt-text="Captura de pantalla que muestra la comprobación del mantenimiento del sistema.":::

#### <a name="sanity"></a>Comprobación

- **Dispositivo**: ejecuta la comprobación del dispositivo. Puede realizar la misma comprobación mediante el comando de la CLI `system-sanity`.

- **Versión**: muestra la versión del dispositivo.

- **Propiedades de red**: muestra los parámetros de red del sensor.

#### <a name="redis"></a>Redis

- **Memoria**: proporciona un panorama general del uso de memoria; por ejemplo, cuánta memoria se usó y cuánta quedó disponible.

- **Clave más larga**: muestra las claves más largas que pueden causar un uso intensivo de la memoria.

#### <a name="system"></a>Sistema

- **Registro principal**: proporciona las últimas 500 filas del registro principal, lo que le permite ver las filas de registro recientes sin exportar todo el registro del sistema.

- **Administrador de tareas**: traduce las tareas que aparecen en la tabla de procesos a los siguientes niveles: 
  
  - Nivel persistente (Redis) 
  - Nivel de efectivo (SQL)

- **Estadísticas de red**: muestra las estadísticas de la red.

- **TOP**: muestra la tabla de procesos. Se trata de un comando de Linux que proporciona una vista dinámica en tiempo real del sistema en ejecución.

- **Comprobación de memoria de copia de seguridad**: proporciona el estado de la memoria de copia de seguridad y comprueba lo siguiente:
  - La ubicación de la carpeta de copia de seguridad. 
  - El tamaño de la carpeta de copia de seguridad.
  - Las limitaciones de la carpeta de copia de seguridad.
  - Cuándo se produjo la última copia de seguridad.
  - La cantidad de espacio disponible para los archivos de copia de seguridad adicionales.

- **ifconfig**: muestra los parámetros de las interfaces físicas del dispositivo.

- **CyberX nload**: muestra el ancho de banda y el tráfico mediante pruebas de seis segundos.

- **Errores del registro principal**: muestra los errores del archivo de registro principal.

Para acceder a la herramienta, haga lo siguiente:

1. Inicie sesión en el sensor con las credenciales del usuario **Support**.

2. Seleccione **System Statistics** (estadísticas del sistema) en la ventana **System Settings** (configuración del sistema).

    :::image type="icon" source="media/tutorial-install-components/system-statistics-icon.png" border="false":::

### <a name="checking-system-health-by-using-the-cli"></a>Comprobación del mantenimiento del sistema mediante la CLI

**Prueba 1: Comprobación**

Compruebe que el sistema está en funcionamiento:

1. Conéctese a la CLI con el terminal de Linux (por ejemplo, PuTTy) y el usuario **Support**.

2. Escriba `system sanity`.

3. Compruebe que todos los servicios están en verde (en ejecución).

    :::image type="content" source="media/tutorial-install-components/support-screen.png" alt-text="Captura de pantalla que muestra los servicios en ejecución.":::

4. Compruebe que se muestre el mensaje **System is UP! (prod)** (el sistema funciona [producción]) en la parte inferior.

**Prueba 2: Comprobación de versión**

Compruebe que se está usando la versión correcta:

1. Conéctese a la CLI con el terminal de Linux (por ejemplo, PuTTy) y el usuario **Support**.

2. Escriba `system version`.

3. Compruebe que se muestre la versión correcta.

**Prueba 3: Validación de la red**

Compruebe que todas las interfaces de entrada configuradas durante el proceso de instalación se estén ejecutando:

1. Conéctese a la CLI con el terminal de Linux (por ejemplo, PuTTy) y el usuario **Support**.

2. Escriba `network list` (el equivalente del comando de Linux `ifconfig`).

3. Compruebe que se muestren las interfaces de entrada necesarias. Por ejemplo, si están instalados dos adaptadores de red de cuatro puertos de cobre, debe haber diez interfaces en la lista.

    :::image type="content" source="media/tutorial-install-components/interface-list-screen.png" alt-text="Captura de pantalla que muestra la lista de interfaces.":::

**Prueba 4: Acceso de administración a la interfaz de usuario**

Compruebe que puede acceder a la GUI web de la consola:

1. Conecte un portátil con un cable Ethernet al puerto de administración (**Gb1**).

2. Defina la dirección del adaptador de red del portátil para que esté en el mismo intervalo que el dispositivo.

    :::image type="content" source="media/tutorial-install-components/access-to-ui.png" alt-text="Captura de pantalla que muestra el acceso de administración a la interfaz de usuario.":::

3. Haga ping a la dirección IP del dispositivo desde el equipo portátil para comprobar la conectividad (valor predeterminado: 10.100.10.1).

4. Abra el explorador web Chrome en el equipo portátil y escriba la dirección IP del dispositivo.

5. En la ventana **La conexión no es privada**, seleccione **Avanzado** y continúe.

6. La prueba se realiza correctamente cuando aparece la pantalla de inicio de sesión de Defender para IoT.

   :::image type="content" source="media/tutorial-install-components/defender-for-iot-sign-in-screen.png" alt-text="Captura de pantalla que muestra el acceso a la consola de administración.":::

## <a name="troubleshooting"></a>Solución de problemas

### <a name="you-cant-connect-by-using-a-web-interface"></a>No es posible conectarse mediante una interfaz web

1. Compruebe que el equipo que está intentando conectar esté en la misma red que el dispositivo.

2. Compruebe que la red de GUI esté conectada al puerto de administración.

3. Haga ping en la dirección IP del dispositivo. Si no se encuentra un ping:

   1. Conecte un monitor y un teclado al dispositivo.

   1. Use el usuario y la contraseña de **Support** para iniciar sesión.

   1. Use el comando `network list` para ver la dirección IP actual.

      :::image type="content" source="media/tutorial-install-components/network-list.png" alt-text="Captura de pantalla que muestra la lista de redes.":::

4. Si los parámetros de red no están configurados correctamente, use el procedimiento siguiente para cambiarlos:

   1. use el comando `network edit-settings`.

   1. Para cambiar la dirección IP de la red de administración, seleccione **Y**.

   1. Para cambiar la máscara de subred, seleccione **Y**.

   1. Para cambiar el DNS, seleccione **Y**.

   1. Para cambiar la dirección IP de puerta de enlace predeterminada, seleccione **Y**.

   1. Para el cambio de la interfaz de entrada (solo para sensor), seleccione **N**.

   1. Seleccione **Y** para aplicar los cambios.

5. Después del reinicio, conéctese con las credenciales del usuario de soporte técnico y use el comando `network list` para comprobar que los parámetros se hayan cambiado.

6. Intente hacer ping y volver a conectarse desde la GUI.

### <a name="the-appliance-isnt-responding"></a>El dispositivo no responde

1. Conecte un monitor y un teclado al dispositivo, o use PuTTY para conectarse de forma remota a la CLI.

2. Use las credenciales del usuario **Support** para iniciar sesión.

3. Use el comando `system sanity` y compruebe que todos los procesos se están ejecutando.

    :::image type="content" source="media/tutorial-install-components/system-sanity-screen.png" alt-text="Captura de pantalla que muestra el comando de comprobación del sistema.":::

Para cualquier otra incidencia, póngase en contacto con [Soporte técnico de Microsoft](https://support.microsoft.com/en-us/supportforbusiness/productselection?sapId=82c88f35-1b8e-f274-ec11-c6efdd6dd099).

## <a name="appendix-a-mirroring-port-on-vswitch-esxi"></a>Apéndice A: Puerto de creación de reflejo en vSwitch (ESXi)

### <a name="configure-a-span-port-on-an-existing-vswitch"></a>Configuración de un puerto SPAN en una instancia de vSwitch existente

Una instancia de vSwitch no tiene funcionalidades de creación de reflejo, pero se puede usar una sencilla solución alternativa para implementar un puerto SPAN.

Para configurar un puerto SPAN, haga lo siguiente:

1. Abra las propiedades de vSwitch.

2. Seleccione **Agregar**.

3. Seleccione **Virtual Machine** > **Next** (máquina virtual > siguiente).

4. Inserte una etiqueta de red **SPAN Network**, seleccione **VLAN ID** > **All** (id. de VLAN > todos) y, después, seleccione **Next** (siguiente).

5. Seleccione **Finalizar**.

6. Seleccione **SPAN Network** > **Edit* (red SPAN > editar).

7. Seleccione **Security** (seguridad) y compruebe que la directiva **Promiscuous Mode** (modo promiscuo) esté establecida en el modo **Accept** (aceptar).

8. Seleccione **OK** (aceptar) y, después, seleccione **Close** (cerrar) para cerrar las propiedades de vSwitch.

9. Abras la propiedades de **XSense VM**.

10. En **Network Adapter 2** (adaptador de red 2), seleccione la red **SPAN**.

11. Seleccione **Aceptar**.

12. Conéctese al sensor y compruebe que la creación de reflejos funcione.

## <a name="appendix-b-access-sensors-from-the-on-premises-management-console"></a>Apéndice B: Acceso a los sensores desde la consola de administración local

Puede mejorar la seguridad del sistema al impedir el acceso de usuario directo al sensor. En su lugar, use la tunelización de proxy para permitir que los usuarios accedan al sensor desde la consola de administración local con una sola regla de firewall. Esta técnica reduce la posibilidad del acceso no autorizado al entorno de red más allá del sensor. La experiencia del usuario al iniciar sesión en el sensor seguirá siendo la misma.

:::image type="content" source="media/tutorial-install-components/sensor-system-graph.png" alt-text="Captura de pantalla que muestra el acceso al sensor.":::

Para habilitar la tunelización, haga lo siguiente:

1. Inicie sesión en la CLI del dispositivo de la consola de administración local con las credenciales de usuario **CyberX** o **Support**.

2. Escriba `sudo cyberx-management-tunnel-enable`.

3. Seleccione **Entrar**.

4. Escriba `--port 10000`.

### <a name="next-steps"></a>Pasos siguientes

[Configuración de la red](how-to-set-up-your-network.md)