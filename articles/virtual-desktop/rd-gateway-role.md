---
title: 'Implementación del rol Puerta de enlace de Escritorio remoto en Windows Virtual Desktop: Azure'
description: Procedimiento para implementar el rol Puerta de enlace de Escritorio remoto en Windows Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 01/26/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: ca3c36f339d5cbca6d5b5990415fba180ae42a13
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798031"
---
# <a name="deploy-the-rd-gateway-role-in-windows-virtual-desktop"></a>Implementación del rol Puerta de enlace de Escritorio remoto en Windows Virtual Desktop

En este artículo se explica cómo implementar los servidores de puerta de enlace de Escritorio remoto en su entorno. Puedes instalar los roles de servidor en equipos físicos o máquinas virtuales, en función de si estás creando un entorno local, basado en la nube o híbrido.

## <a name="install-the-rd-gateway-role"></a>Instalación del rol Puerta de enlace de escritorio remoto

1. Inicie sesión en el servidor de destino con credenciales administrativas.

2. En **Administrador del servidor**, seleccione **Administrar** y, a continuación, seleccione **Agregar roles y características**. Se abrirá el instalador **Agregar roles y características**.

3. En **Antes de empezar**, seleccione **Siguiente**.

4. En **Seleccionar tipo de instalación**, seleccione **Instalación basada en características o en roles** y, a continuación, haga clic en **Siguiente**.

5. En **Seleccionar servidor de destino**, elija **Seleccionar un servidor del grupo de servidores**. En **Grupo de servidores**, seleccione el nombre del equipo local. Cuando finalice, seleccione **Siguiente**.

6. En **Seleccionar roles de servidor** > **Roles**, seleccione **Servicios de Escritorio remoto**. Cuando finalice, seleccione **Siguiente**.

7. En **Servicios de Escritorio remoto**, seleccione **Siguiente**.

8. En **Seleccionar servicios de rol**, seleccione solo **Puerta de enlace de Escritorio remoto**. Cuando se le pida que agregue las características necesarias, seleccione **Agregar características**. Cuando finalice, seleccione **Siguiente**.

9. En **Servicios de acceso y directivas de redes**, seleccione **Siguiente**.

10. En **Rol de servidor web (IIS)** , seleccione **Siguiente**.

11. En **Seleccionar servicios de rol**, seleccione **Siguiente**.

12. En **Confirmar selecciones de instalación**, haga clic en **Instalar**. No cierre el instalador mientras se está realizando el proceso de instalación.

## <a name="configure-rd-gateway-role"></a>Configuración del rol Puerta de enlace de Escritorio remoto

Una vez instalado el rol Puerta de enlace de Escritorio remoto, deberá configurarlo.

Para configurar el rol Puerta de enlace de Escritorio remoto:

1. Abra el **Administrador del servidor** y, a continuación, seleccione **Servicios de Escritorio remoto**.

2. Vaya a **Servidores**, haga clic con el botón derecho en el nombre del servidor y seleccione **Administrador de puerta de enlace de Escritorio remoto**.

3. En el Administrador de puerta de enlace de Escritorio remoto, haga clic con el botón derecho en el nombre de la puerta de enlace y seleccione **Propiedades**.

4. Abra la pestaña **Certificado SSL**, seleccione la burbuja **Import a certificate into the RD Gateway** (Importar un certificado a la puerta de enlace de Escritorio remoto) y, a continuación, seleccione **Examinar e importar certificado...** .

5. Seleccione el nombre del archivo PFX y, luego, seleccione **Abrir**.

6. Escriba la contraseña del archivo PFX cuando se le solicite.

7. Después de haber importado el certificado y su clave privada, la pantalla debe mostrar los atributos clave del certificado.

>[!NOTE]
>Dado que se supone que el rol Puerta de enlace de Escritorio remoto es público, se recomienda usar un certificado emitido públicamente. Si usa un certificado emitido de forma privada, deberá asegurarse de configurar de antemano todos los clientes con la cadena de confianza del certificado.

## <a name="next-steps"></a>Pasos siguientes

Si quiere agregar alta disponibilidad al rol Puerta de enlace de Escritorio remoto, consulte [Adición de alta disponibilidad al front-end web de la puerta de enlace y de la web de Escritorio remoto](/windows-server/remote/remote-desktop-services/rds-rdweb-gateway-ha).