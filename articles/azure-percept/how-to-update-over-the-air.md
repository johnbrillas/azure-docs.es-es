---
title: Actualización de Azure Percept DK de forma inalámbrica
description: Aprenda a recibir las actualizaciones de forma inalámbrica de Azure Percept DK.
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: b8f9e6f4bc091abbd1bb08ecbd649c1411e5ab20
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102095398"
---
# <a name="update-your-azure-percept-dk-over-the-air"></a>Actualización de Azure Percept DK de forma inalámbrica

Siga esta guía para obtener información sobre cómo actualizar la placa base de Azure Percept DK de forma inalámbrica con Device Update para IoT Hub.

## <a name="import-your-update-file-and-manifest-file"></a>Importe el archivo de actualización y el archivo manifiesto.

> [!NOTE]
> Si ya ha importado la actualización, puede ir directamente a **Creación de un grupo actualizado de dispositivos**.

1. Vaya a la instancia de Azure IoT Hub que está utilizando para el dispositivo de Azure Percept. En el panel de menú izquierdo, seleccione **Actualizaciones de dispositivos** en **Administración de dispositivos automática**.
 
1. Verá varias pestañas en la parte superior de la pantalla. Seleccione la pestaña **Actualizaciones**.
 
1. Seleccione **+ Importar nueva actualización** bajo el encabezado **Listo para la implementación**.
 
1. Haga clic en los cuadros bajo **Seleccione el archivo manifiesto de importación** y **Select Update Files** (Seleccionar actualización de archivos) para seleccionar el archivo manifiesto (.json) adecuado y un archivo de actualización (.swu). [Aquí](https://go.microsoft.com/fwlink/?linkid=2155625) puede encontrar estos archivos de actualización para el dispositivo de Azure Percept.
 
1. Seleccione el icono de carpeta o el cuadro de texto bajo **Seleccione un contenedor de almacenamiento** y, a continuación, elija la cuenta de almacenamiento adecuada.
 
1. Si ya ha creado un contenedor de almacenamiento, puede volver a utilizarlo. En caso contrario, seleccione **+ Contenedor** a fin de crear un nuevo contenedor de almacenamiento para las actualizaciones de forma inalámbrica. Seleccione el contenedor que desee usar y haga clic en **Seleccionar**.
 
    >[!Note]
    >Si no tiene un contenedor, se le pedirá que cree uno.
 
1. Seleccione **Enviar** para iniciar el proceso de importación. Este envío tardará unos 4 minutos.
 
    >[!Note]
    >Es posible que se le pida que agregue una regla de solicitud de origen cruzado (CORS) para acceder al contenedor de almacenamiento seleccionado. Seleccione **Add rule and retry** (Agregar regla e intentarlo de nuevo) para continuar.
 
    >[!Note]
    >Debido al tamaño de la imagen, es posible que vea la página **Enviando…** hasta un máximo de 5 minutos antes de ver el paso siguiente.
    
1. Se iniciará el proceso de importación y se le redirigirá a la pestaña **Historial de importación** de la página **Actualizaciones de dispositivos**. Haga clic en **Actualizar** para supervisar el progreso mientras se completa el proceso de importación. En función del tamaño de la actualización, esta operación puede tardar unos minutos o más (durante las horas punta, espere que el servicio de importación tarde hasta 1 hora).

1. Cuando la columna Estado indique que la importación se ha realizado correctamente, seleccione la pestaña **Listo para la implementación** y haga clic en **Actualizar**. Ahora debería ver la actualización importada en la lista.
 
## <a name="create-a-device-update-group"></a>Creación de un grupo de Device Update
Device Update para IoT Hub le permite destinar una actualización a grupos específicos de Azure Percept DK. Para crear un grupo, debe agregar una etiqueta al conjunto de dispositivos de destino en Azure IoT Hub.

> [!NOTE]
> Si ya ha creado un grupo, puede ir directamente al paso siguiente.

Requisitos de la etiqueta de grupo:
- Puede agregar cualquier valor a la etiqueta excepto **Sin categorizar**, que es un valor reservado.
- El valor de etiqueta no puede tener más de 255 caracteres.
- El valor de etiqueta solo puede contener estos caracteres especiales: ".", "-", "_", "~".
- Los nombres de etiquetas y grupos distinguen mayúsculas de minúsculas.
- Un dispositivo solo puede tener una etiqueta. Cualquier etiqueta subsiguiente agregada al dispositivo invalidará la etiqueta anterior.
- Un dispositivo solo puede pertenecer a un único grupo.

1. Agregue una etiqueta al dispositivo o dispositivos.
    1. En **IoT Edge** en el panel de navegación izquierdo, busque Azure Percept DK y vaya a su **Dispositivo gemelo**.
    1. Agregue una nueva etiqueta **Device Update para IoT Hub** tal como se muestra a continuación (cambie ```<CustomTagValue>``` por su valor, por ejemplo, AzurePerceptGroup1). Más información sobre [etiquetas de documento JSON](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins#device-twins) de dispositivos gemelos.

    ```
    "tags": {
    "ADUGroup": "<CustomTagValue>"
    },
    ```

 
1. Haga clic en **Guardar** y resuelva los problemas de formato.
 
1. Cree un grupo seleccionando una etiqueta de Azure IoT Hub existente.
    1. Vaya de nuevo a la página de Azure IoT Hub.
    1. Seleccione **Actualizaciones de dispositivos** en **Administración de dispositivos automática** en el panel de menú izquierdo.
    1. Seleccione la pestaña **Grupos** . En esta página se mostrará el número de dispositivos sin agrupar conectados a Device Update.
    1. Seleccione **+ Agregar** para crear un nuevo grupo.
    1. Seleccione una etiqueta de IoT Hub de la lista y haga clic en **Enviar**.
    1. Una vez creado el grupo, se actualizará la lista de grupos y gráficos de cumplimiento de las actualizaciones. El gráfico muestra el número de dispositivos en distintos estados de cumplimiento: **On latest update** (Actualización más reciente), **Nuevas actualizaciones disponibles**, **Actualizaciones en curso** y **Aún sin agrupar**.
 

## <a name="deploy-an-update"></a>Implementación de una actualización
1. Debería ver el grupo recién creado con una nueva actualización que aparece en **Actualizaciones disponibles** (puede que tenga que actualizar una vez). Seleccione la actualización.
 
1. Confirme que el grupo de dispositivos correcto esté seleccionado como grupo de dispositivos de destino. Seleccione una **Fecha de inicio** y una **Hora de inicio** para la implementación y, a continuación, haga clic en **Crear implementación**. 

    >[!CAUTION]
    >Si se establece la hora de inicio en el pasado, la implementación se desencadenará de inmediato.
 
1. Compruebe el gráfico de cumplimiento. Debería ver que la actualización está en curso.
 
1. Una vez completada la actualización, el gráfico de cumplimiento reflejará el nuevo estado de actualización.
 
1. Seleccione la pestaña **Implementaciones** en la parte superior de la página **Actualizaciones del dispositivo**.
 
1. Seleccione la implementación para ver sus detalles. Es posible que tenga que hacer clic en **Actualizar** hasta que el **Estado** cambie a **Correcto**.

## <a name="next-steps"></a>Pasos siguientes

El kit de desarrollo se ha actualizado correctamente. Puede continuar con el desarrollo y el funcionamiento mediante el kit de desarrollo.