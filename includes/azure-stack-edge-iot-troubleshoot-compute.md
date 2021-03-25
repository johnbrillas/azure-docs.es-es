---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 03/02/2021
ms.openlocfilehash: 57415ec76a3e8d9fc3c160b47668d3419ff6ea5c
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622168"
---
Uso de las respuestas del entorno de ejecución del agente de IoT Edge para solucionar los problemas relacionados con el proceso. A continuación, se ofrece una lista de las posibles respuestas:

* 200 - CORRECTO
* 400 - La configuración de implementación tiene un formato incorrecto o no es válida.
* 417 - El dispositivo no tiene un conjunto de configuración de implementación.
* 412 - La versión del esquema de la configuración de implementación no es válida.
* 406 - El dispositivo de IoT Edge está sin conexión o no envía informes de estado.
* 500 - Error en el entorno en tiempo de ejecución de Azure IoT Edge.

Para más información, consulte [Agente de IoT Edge](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

El siguiente error está relacionado con el servicio de IoT Edge en el dispositivo Azure Stack Edge Pro.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Los módulos de proceso tienen el estado Desconocido y no se pueden usar

#### <a name="error-description"></a>Descripción del error

Todos los módulos del dispositivo muestran el estado Desconocido y no se pueden usar. El estado Desconocido persiste tras un reinicio.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Solución propuesta

Elimine el servicio de IoT Edge y, a continuación, vuelva a implementar los módulos. Para obtener más información, consulte [Eliminación del servicio de IoT Edge](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).


### <a name="modules-show-as-running-but-are-not-working"></a>Los módulos aparecen con el estado en ejecución, pero no funcionan

#### <a name="error-description"></a>Descripción del error

El estado de tiempo de ejecución del módulo indica que está en ejecución, pero no se ven los resultados esperados. 

Esta condición podría deberse a un problema con la configuración de la ruta del módulo que no funciona o que `edgehub` no enruta los mensajes según lo esperado. Puede comprobar los registros `edgehub`. Si ve que hay errores, como que no se puede conectar al servicio IoT Hub, el motivo más común son los problemas de conectividad. Los problemas de conectividad pueden deberse a que el puerto AMPQ que el servicio IoT Hub usa como puerto predeterminado para la comunicación está bloqueado o el servidor proxy web está bloqueando estos mensajes.

#### <a name="suggested-solution"></a>Solución propuesta

Siga estos pasos:
1. Para resolver el error, vaya al recurso IoT Hub del dispositivo y, a continuación, seleccione el dispositivo perimetral. 
1. Vaya a **Establecer módulos > Configuración de tiempo de ejecución**. 
1. Agregue la variable de entorno `Upstream protocol` y asígnele el valor `AMQPWS`. Los mensajes configurados en este caso se envían a través de WebSockets por el puerto 443.

### <a name="modules-show-as-running-but-do-not-have-an-ip-assigned"></a>Los módulos aparecen con el estado en ejecución, pero no tienen una dirección IP asignada

#### <a name="error-description"></a>Descripción del error

El estado de tiempo de ejecución del módulo indica que está en ejecución, pero la aplicación en contenedor no tiene una dirección IP asignada. 

Esta condición se debe a que el intervalo de direcciones IP que ha proporcionado para direcciones IP del servicio externo de Kubernetes no es suficiente. Debe ampliar este intervalo para asegurarse de que abarca todos los contenedores o máquinas virtuales que ha implementado.

#### <a name="suggested-solution"></a>Solución propuesta

En la interfaz de usuario web local del dispositivo, realice los pasos siguientes:
1. Vaya a la página **Proceso**. Seleccione el puerto para el que ha habilitado la red de proceso. 
1. Especifique un intervalo estático y contiguo de direcciones IP para las **direcciones IP del servicio externo de Kubernetes**. Necesita una dirección IP para el servicio `edgehub`. Además, necesita una dirección IP para cada módulo de IoT Edge y para cada máquina virtual que vaya a implementar. 
1. Seleccione **Aplicar**. El intervalo de IP cambiado debe surtir efecto de inmediato.

Para obtener más información, consulte [Cambio de IP de servicio externo para contenedores](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#change-external-service-ips-for-containers).