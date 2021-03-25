---
title: Azure Stream Analytics en IoT Edge
description: Cree trabajos perimetrales en Azure Stream Analytics e impleméntelos en dispositivos que ejecutan Azure IoT Edge.
ms.service: stream-analytics
author: an-emma
ms.author: raan
ms.topic: conceptual
ms.date: 12/18/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: c2a062b75caa84a0e3c342ca1ce45ccce12f2bb7
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98012621"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>Azure Stream Analytics en IoT Edge
 
Azure Stream Analytics en IoT Edge permite a los desarrolladores implementar inteligencia analítica casi en tiempo real más próxima a los dispositivos IoT para que puedan desbloquear el valor total de los datos generados por los dispositivos. Azure Stream Analytics está diseñado con los objetivos de baja latencia, resiliencia, uso eficiente de ancho de banda y cumplimiento. Las empresas pueden implementar la lógica de control cerca de las operaciones industriales y complementar los análisis de macrodatos que se realizan en la nube.

Azure Stream Analytics en IoT Edge se ejecuta en el marco de [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). Una vez que el trabajo se crea en Stream Analytics, puede implementarlo y administrarlo mediante IoT Hub.

## <a name="common-scenarios"></a>Escenarios frecuentes

En esta sección se describen los escenarios comunes de Stream Analytics en IoT Edge. En el diagrama siguiente se muestra el flujo de datos entre los dispositivos de IoT y la nube de Azure.

:::image type="content" source="media/stream-analytics-edge/edge-high-level-diagram.png" alt-text="Diagrama de alto nivel de IoT Edge":::

### <a name="low-latency-command-and-control"></a>Control y comando de baja latencia

Los sistemas de seguridad en la fabricación deben responder a los datos operativos con una latencia muy baja. Con Stream Analytics en IoT Edge, puede analizar los datos de los sensores casi en tiempo real y emitir comandos cuando detecte anomalías para detener una máquina o desencadenar alertas.

### <a name="limited-connectivity-to-the-cloud"></a>Conectividad limitada a la nube

Los sistemas críticos, como un equipo de minería remoto, buques conectados o las perforaciones petrolíferas en mar abierto necesitan analizar y reaccionar ante los datos, incluso cuando la conectividad a la nube sea intermitente. Con Stream Analytics, la lógica de streaming funciona independientemente de la conectividad de red y puede elegir lo que envía a la nube para su posterior procesamiento o almacenamiento.

### <a name="limited-bandwidth"></a>Ancho de banda limitado

El volumen de datos que generan los motores de jet o los automóviles conectados puede ser tan grande que los datos se deben filtrar o procesar con anterioridad antes de enviarlos a la nube. Mediante Azure Stream Analytics puede filtrar o agregar los datos que hay que enviar a la nube.

### <a name="compliance"></a>Cumplimiento

El cumplimiento de las normas puede requerir que se agreguen algunos datos o que se oculte su identidad localmente antes de enviarlos a la nube.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Trabajos de Edge en Azure Stream Analytics

Los trabajos de Stream Analytics en Edge se ejecutan en contenedores implementados en [dispositivos de Azure IoT Edge](../iot-edge/about-iot-edge.md). Los trabajos en Edge constan de dos partes:

* Una parte en la nube que es responsable de la definición del trabajo: los usuarios definen las entradas, la salida, la consultas y otros valores, como los eventos que no funcionan, en la nube.

* Un módulo que se ejecuta en dispositivos IoT. El módulo contiene el motor de Stream Analytics y recibe la definición del trabajo de la nube. 

Stream Analytics usa IoT Hub para implementar los trabajos perimetrales en los dispositivos. Para más información, consulte [Implementación de IoT Edge](../iot-edge/module-deployment-monitoring.md).

:::image type="content" source="media/stream-analytics-edge/stream-analytics-edge-job.png" alt-text="Trabajo en Edge de Azure Stream Analytics":::

## <a name="edge-job-limitations"></a>Limitaciones de los trabajos en Edge

El objetivo es tener paridad entre los trabajos de IoT Edge y los trabajos en la nube. La mayoría de las características del lenguaje de consulta SQL se admiten tanto en el borde como en la nube. Sin embargo, las siguientes características aún no se admiten en los trabajos perimetrales:
* Funciones definidas por el usuario (UDF) en JavaScript. Las UDF están disponibles en [ C# para trabajos de IoT Edge](./stream-analytics-edge-csharp-udf.md) (versión preliminar).
* Agregados definidos por el usuario (UDA).
* Funciones de Azure Machine Learning.
* Formato AVRO para la entrada y salida. En este momento solo se admiten CSV y JSON.
* Los siguientes operadores SQL:
    * PARTITION BY
    * GetMetadataPropertyValue
* Directiva de llegada tardía

### <a name="runtime-and-hardware-requirements"></a>Requisitos de runtime y hardware
Para ejecutar Stream Analytics en IoT Edge, se necesitan dispositivos que puedan ejecutar [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/). 

Stream Analytics y Azure IoT Edge usan contenedores de **Docker** para proporcionar una solución portátil que se ejecuta en varios sistemas operativos host (Windows y Linux).

Stream Analytics en IoT Edge está disponible en forma de imágenes de Windows y Linux, que se ejecutan en arquitecturas x86-64 o ARM (Advanced RISC Machines). 


## <a name="input-and-output"></a>Entrada y salida

Los trabajos de Edge de Stream Analytics pueden obtener entradas y salidas de otros módulos que se ejecutan en dispositivos de IoT Edge. Para conectarse a determinados módulos, y desde ellos, puede establecer la configuración de enrutamiento en el momento de la implementación. Se puede encontrar más información en [la documentación de composición del módulo de IoT Edge](../iot-edge/module-composition.md).

En las entradas y salidas, se admiten los formatos CSV y JSON.

Por cada flujo de entrada y salida que se crea en un trabajo de Stream Analytics, se crea un punto de conexión correspondiente en el módulo implementado. Estos puntos de conexión se pueden utilizar en las rutas de la implementación.

Los tipos de entrada de secuencia admitidos son:
* Centro de Microsoft Edge
* Centro de eventos
* IoT Hub

Los tipos de salida de secuencia que se admiten son:
* Centro de Microsoft Edge
* SQL Database
* Centro de eventos
* Blob Storage/ADLS Gen2

La entrada de referencia admite el tipo de archivo de referencia. Se puede llegar a otras salidas mediante un trabajo de nube de nivel inferior. Por ejemplo, un trabajo de Stream Analytics hospedado en Edge envía una salida al Centro de Microsoft Edge, el cual puede enviar luego una salida a IoT Hub. Puede usar un segundo trabajo de Azure Stream Analytics hospedado en la nube con entrada desde IoT Hub y salida a Power BI u otro tipo de salida.

## <a name="license-and-third-party-notices"></a>Licencia y avisos de terceros
* [Licencia de Azure Stream Analytics en IoT Edge](https://go.microsoft.com/fwlink/?linkid=862827). 
* [Aviso de terceros para Azure Stream Analytics en IoT Edge](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="azure-stream-analytics-module-image-information"></a>Información de la imagen del módulo de Azure Stream Analytics 

Esta información de versión se actualizó por última vez el 21 de septiembre de 2020:

- Imagen: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-amd64`
   - imagen base: mcr.microsoft.com/dotnet/core/runtime:2.1.13-alpine
   - plataforma:
      - arquitectura: amd64
      - so: linux
 
- Imagen: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm32v7`
   - imagen base: mcr.microsoft.com/dotnet/core/runtime:2.1.13-bionic-arm32v7
   - plataforma:
      - arquitectura: arm
      - so: linux
 
- Imagen: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.9-linux-arm64`
   - imagen base: mcr.microsoft.com/dotnet/core/runtime:3.0-bionic-arm64v8
   - plataforma:
      - arquitectura: arm64
      - so: linux
      
      
## <a name="get-help"></a>Obtener ayuda
Para más ayuda, pruebe la [Página de preguntas y respuestas de Microsoft sobre Azure Stream Analytics](/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Pasos siguientes

* [Más información acerca de Azure IoT Edge](../iot-edge/about-iot-edge.md)
* [Tutorial de Stream Analytics en IoT Edge](../iot-edge/tutorial-deploy-stream-analytics.md)
* [Desarrollo de trabajos para dispositivos perimetrales de Stream Analytics mediante herramientas de Visual Studio](./stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Implement CI/CD for Stream Analytics using APIs](stream-analytics-cicd-api.md) (Implementación de CI/CD para Stream Analytics mediante API)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: /stream-analytics-query/stream-analytics-query-language-reference
[stream.analytics.rest.api.reference]: /rest/api/streamanalytics/
