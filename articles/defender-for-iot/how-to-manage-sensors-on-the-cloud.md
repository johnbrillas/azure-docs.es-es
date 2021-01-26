---
title: Incorporación y administración de sensores en el portal de Defender para IoT
description: Aprenda a incorporar, ver y administrar sensores en el portal de Defender para IoT.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/27/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: 427ea3884a3db6ba33405014435cf1f962670064
ms.sourcegitcommit: 61d2b2211f3cc18f1be203c1bc12068fc678b584
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2021
ms.locfileid: "98562716"
---
# <a name="onboard-and-manage-sensors-in-the-defender-for-iot-portal"></a>Incorporación y administración de sensores en el portal de Defender para IoT

En este artículo se describe cómo incorporar, ver y administrar sensores en el [portal de Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started).

## <a name="onboard-sensors"></a>Sensores incorporados

Para incorporar un sensor, debe registrarlo en Azure Defender para IoT y descargar un archivo de activación de sensor.

### <a name="register-the-sensor"></a>Registro del sensor

Para registrarse:

1. Vaya a la **página principal** del [portal de Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started).
1. Seleccione **Onboard sensor** (Incorporar sensor).
1. Cree un nombre de sensor. Se recomienda incluir la dirección IP del sensor instalado como parte del nombre o usar un nombre que sea fácil de identificar. De esta forma, se garantiza un seguimiento más sencillo y una nomenclatura coherente entre el nombre de registro en el [portal de Azure Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) y la dirección IP del sensor implementado que se muestra en la consola del sensor.
1. Asocie el sensor a una suscripción de Azure.
1. Elija un modo de administración de sensor mediante el botón de alternancia **Cloud connected** (Conectado a la nube). Si el botón de alternancia está activado, el sensor está conectado a la nube. Si el botón de alternancia está desactivado, el sensor está administrado de forma local.

   - **Cloud-connected sensors** (Sensores conectados a la nube): la información que detecta el sensor se muestra en la consola del sensor. Además, se proporciona información de alertas mediante un centro de IoT, que puede compartirse con otros servicios de Azure, como Azure Sentinel.

   - **Locally managed sensors** (Sensores administrados de forma local): la información que detectan los sensores se muestra en la consola del sensor. Si trabaja en una red aislada y quiere una vista unificada de toda la información detectada por varios sensores administrados de forma local, trabaje con la consola de administración local.

   En el caso de los sensores conectados a la nube, el nombre que se define durante la incorporación es el nombre que aparece en la consola del sensor. Este nombre no se puede cambiar directamente desde la consola. En el caso de los sensores administrados de forma local, el nombre que se aplique durante la incorporación se almacenará en Azure y se puede actualizar en la consola del sensor.

1. Elija un centro de IoT que sirva como puerta de enlace entre este sensor y Azure Defender para IoT.
1. Si el sensor está conectado a la nube, asócielo a un centro de IoT y luego defina un nombre de sitio y una zona. También puede agregar etiquetas descriptivas. El nombre del sitio, la zona y las etiquetas son entradas descriptivas en la página [Sites and sensors](#view-onboarded-sensors) (Sitios y sensores).

### <a name="download-the-sensor-activation-file"></a>Descarga del archivo de activación del sensor

El archivo de activación del sensor contiene instrucciones sobre el modo de administración del sensor. Descargue un archivo de activación único para cada sensor que implemente. Un usuario que inicia sesión en la consola del sensor por primera vez carga el archivo de activación en el sensor.

Para descargar un archivo de activación:

1. En la página **Onboard sensor** (Incorporar sensor), seleccione **Download activation file** (Descargar archivo de activación).
1. Ponga el archivo a disposición del usuario que inicia sesión en la consola del sensor por primera vez.

## <a name="view-onboarded-sensors"></a>Visualización sensores incorporados

En el [portal de Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started), puede ver información básica sobre los sensores incorporados. 

1. Seleccione **Sites and sensors** (Sitios y sensores).
1. En la página **Sites and sensors** (Sitios y sensores), use herramientas de búsqueda y filtro para buscar la información de sensor que necesite.

La información disponible incluye:

- Cuántos sensores se incorporaron
- El número de sensores conectados a la nube y administrados localmente
- El centro asociado a un sensor incorporado
- Detalles agregados sobre un sensor, como el nombre asignado al sensor durante la incorporación, la zona asociada al sensor u otra información descriptiva agregada con etiquetas

## <a name="manage-onboarded-sensors"></a>Administración de sensores incorporados

Use el [portal de Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started) para realizar las tareas de administración relacionadas con los sensores.

### <a name="export"></a>Exportación

Para exportar información del sensor incorporado, seleccione el icono **Exportar** en la parte superior de la página **Sites and sensors** (Sitios y sensores).

### <a name="edit"></a>Editar

Use las herramientas de edición de **Sites and sensors** (Sitios y sensores) para agregar y editar el nombre del sitio, la zona y las etiquetas.

### <a name="delete"></a>Eliminar

Si elimina un sensor conectado a la nube, no se enviará información al centro de IoT. Elimine los sensores conectados localmente cuando deje de trabajar con ellos.

Para eliminar un sensor:

1. Haga clic en el botón de puntos suspensivos **(...)** del sensor que quiera administrar. 
1. Confirme la eliminación.

### <a name="reactivate"></a>Reactivar

Se recomienda que actualice el modo en el que se administra el sensor. Por ejemplo:

- **Work in cloud-connected mode instead of locally managed mode** (Trabajar en el modo conectado a la nube en lugar del modo administrado localmente): para ello, actualice el archivo de activación para el sensor conectado localmente con un archivo de activación correspondiente a un sensor conectado a la nube. Después de la reactivación, las detecciones del sensor se mostrarán tanto en el portal del sensor como en el de [Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started). Una vez que el archivo de reactivación se haya cargado correctamente, se enviará la información de las alertas recién detectadas a Azure.

- **Work in locally connected mode instead of cloud-connected mode** (Trabajar en el modo conectado localmente en lugar del modo conectado a la nube): para ello, actualice el archivo de activación de un sensor conectado a la nube con un archivo de activación correspondiente a un sensor administrado localmente. Después de la reactivación, la información de detección del sensor solo se mostrará en el sensor.

- **Associate the sensor to a new IoT hub** (Asociar el sensor a un nuevo centro de IoT):  Para ello, vuelva a registrar el sensor y cargue un nuevo archivo de activación.

Para reactivar un sensor:

1. Vaya a la página **Sites and sensors** (Sitios y sensores) del [portal de Defender para IoT](https://portal.azure.com/#blade/Microsoft_Azure_IoT_Defender/IoTDefenderDashboard/Getting_Started).

2. Seleccione el sensor para el que quiere cargar un nuevo archivo de activación.

3. Elimine el sensor.

4. Vuelva a incorporar el sensor desde la página **Incorporación** en el nuevo modo o con un nuevo centro de IoT.

5. Descargue el archivo de activación de la página **Download activation file** (Descargar archivo de activación).

6. Inicie sesión en la consola del sensor de Defender para IoT.

7. En la consola del sensor, seleccione **Configuración del sistema** y, a continuación, seleccione **Reactivación**.

   :::image type="content" source="media/how-to-manage-sensors-on-the-cloud/reactivate.png" alt-text="Carga del archivo de activación para reactivar el sensor.":::

8. Seleccione **Cargar** y seleccione el archivo guardado.

9. Seleccione **Activar**. 

## <a name="see-also"></a>Consulte también

[Activación y configuración del sensor](how-to-activate-and-set-up-your-sensor.md)
