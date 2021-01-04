---
title: Preguntas más frecuentes sobre Análisis de vídeos en vivo en IoT Edge en Azure
description: En este tema se proporcionan respuestas a las preguntas más frecuentes sobre Análisis de vídeos en directo en IoT Edge.
ms.topic: conceptual
ms.date: 12/01/2020
ms.openlocfilehash: 521cd0e4f5fc8232a000e10520298a979ba1c14c
ms.sourcegitcommit: cc13f3fc9b8d309986409276b48ffb77953f4458
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97401583"
---
# <a name="frequently-asked-questions-faqs"></a>Preguntas más frecuentes (P+F)

En este tema se proporcionan respuestas a las preguntas más frecuentes sobre Análisis de vídeos en directo en IoT Edge.

## <a name="general"></a>General

### <a name="what-are-the-system-variables-that-can-be-used-in-graph-topology-definition"></a>¿Qué variables del sistema se pueden usar en la definición de la topología de grafos?

|Variable   |Descripción|
|---|---|
|[System.DateTime](/dotnet/framework/data/adonet/sql/linq/system-datetime-methods)|Representa un instante en la hora UTC, normalmente expresado en forma de fecha y hora del día (representación básica: aaaaMMddTHHmmssZ).|
|System.PreciseDateTime|Representa una instancia de fecha y hora UTC en formato compatible con el archivo ISO8601 con milisegundos (representación básica: aaaaMMddTHHmmss.fffZ).|
|System.GraphTopologyName   |Representa una topología de grafos multimedia y contiene el plano técnico de un grafo.|
|System.GraphInstanceName|  Representa una instancia de grafo multimedia, contiene los valores de parámetro y hace referencia a la topología.|

## <a name="configuration-and-deployment"></a>Configuración e implementación

### <a name="can-i-deploy-the-media-edge-module-to-a-windows-10-device"></a>¿Se puede implementar el módulo multimedia de Edge en un dispositivo Windows 10?

Sí. Consulte el artículo sobre los [contenedores Linux en Windows 10](/virtualization/windowscontainers/deploy-containers/linux-containers).

## <a name="capture-from-ip-camera-and-rtsp-settings"></a>Captura con una cámara IP y configuración de RTSP

### <a name="do-i-need-to-use-a-special-sdk-on-my-device-to-send-in-a-video-stream"></a>¿Es necesario usar un SDK especial en el dispositivo para enviarlo en una secuencia de vídeo?

No. Análisis de vídeos en vivo en IoT Edge permite capturar elementos multimedia mediante el protocolo de streaming de vídeo RTSP (que es compatible con la mayoría de las cámaras IP).

### <a name="can-i-push-media-to-live-video-analytics-on-iot-edge-using-rtmp-or-smooth-like-a-media-services-live-event"></a>¿Se puede insertar contenido multimedia en Análisis de vídeos en vivo en IoT Edge mediante RTMP o Smooth (como un evento en directo de Media Services)?

* No. Live Video Analytics solo admite RTSP para capturar vídeo con las cámaras IP.
* Cualquier cámara que admita el streaming con RTSP a través de TCP/HTTP debería funcionar. 

### <a name="can-i-reset-or-update-the-rtsp-source-url-on-a-graph-instance"></a>¿Se puede restablecer o actualizar la dirección URL de origen RTSP en una instancia de grafo?

Sí, cuando la instancia del grafo está en estado inactivo.  

### <a name="is-there-a-rtsp-simulator-available-to-use-during-testing-and-development"></a>¿Hay algún simulador RTSP disponible para usarlo durante las pruebas y el desarrollo?

Sí. Hay un módulo perimetral del [simulador RTSP](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) disponible para su uso en los tutoriales y artículos de inicio rápido para respaldar el proceso de aprendizaje. Este módulo se proporciona siempre que sea posible y puede que no siempre esté disponible. Se recomienda no usar esto durante más de unas horas. Debe invertir en las pruebas con el origen RTSP real antes de planear una implementación en producción.

### <a name="do-you-support-onvif-discovery-of-ip-cameras-at-the-edge"></a>¿Admite la detección de ONVIF de cámaras IP en el perímetro?

No, no se admite la detección de ONVIF de dispositivos en el perímetro.

## <a name="streaming-and-playback"></a>Streaming y reproducción

### <a name="can-assets-recorded-to-ams-from-the-edge-be-played-back-using-media-services-streaming-technologies-like-hls-or-dash"></a>¿Pueden reproducirse los recursos registrados en AMS desde el perímetro con las tecnologías de streaming de Media Services, como HLS o DASH?

Sí. Los recursos grabados se pueden transmitir como cualquier otro recurso de Azure Media Services. Para transmitir el contenido, debe existir un punto de conexión de streaming en ejecución. El uso del proceso de creación del localizador de streaming estándar dará acceso a un manifiesto de HLS o DASH para el streaming a cualquier marco de trabajo para reproductores compatible. Para obtener información detallada sobre la creación de manifiestos de publicación de HLS o DASH, vea [Empaquetado dinámico](../latest/dynamic-packaging-overview.md).

### <a name="can-i-use-the-standard-content-protection-and-drm-features-of-media-services-on-an-archived-asset"></a>¿Se pueden usar las características de DRM y protección de contenido estándar de Media Services en un recurso archivado?

Sí. Todas las características de DRM y protección de contenido de cifrado dinámico están disponibles para usarlas en los recursos grabados desde un grafo multimedia.

### <a name="what-players-can-i-use-to-view-content-from-the-recorded-assets"></a>¿Qué reproductores se pueden usar para ver el contenido de los recursos grabados?

Se admiten todos los reproductores estándar compatibles con Apple HTTP Live Streaming (HLS) versión 3 o versión 4. Además, también se admite cualquier reproductor que sea compatible con la reproducción de MPEG-DASH.

Los reproductores recomendados para las pruebas incluyen:

* [Azure Media Player](../latest/use-azure-media-player.md)
* [HLS.js](https://hls-js.netlify.app/demo/)
* [Video.js](https://videojs.com/)
* [Dash.js](https://github.com/Dash-Industry-Forum/dash.js/wiki)
* [Shaka Player](https://github.com/google/shaka-player)
* [ExoPlayer](https://github.com/google/ExoPlayer)
* [HTTP Live Streaming nativo de Apple](https://developer.apple.com/streaming/)
* Reproductor de vídeo HTML5 integrado en Edge, Chrome o Safari
* Reproductores comerciales compatibles con la reproducción HLS o DASH

### <a name="what-are-the-limits-on-streaming-a-media-graph-asset"></a>¿Cuáles son los límites del streaming de un recurso de grafo multimedia?

El streaming de un recurso grabado o en directo desde un grafo multimedia usa la misma infraestructura de gran escala y el punto de conexión de streaming que Media Services admite para el streaming a petición y en directo para clientes de medios y entretenimiento, OTT y difusión. Esto significa que se puede habilitar rápida y fácilmente Azure CDN, Verizon o Akamai para ofrecer el contenido a una audiencia tan pequeña como algunos espectadores, o hasta millones, en función de su escenario.

El contenido se puede entregar mediante Apple HTTP Live Streaming (HLS) o MPEG-DASH.

## <a name="design-your-ai-model"></a>Diseño del modelo de IA 

### <a name="i-have-multiple-ai-models-wrapped-in-a-docker-container-how-should-i-use-them-with-live-video-analytics"></a>Tengo varios modelos de IA encapsulados en un contenedor de Docker. ¿Cómo debo usarlos con Live Video Analytics? 

Las soluciones son diferentes en función del protocolo de comunicación que use el servidor de inferencia para comunicarse con Live Video Analytics. A continuación encontrará algunas formas de hacerlo.

#### <a name="http-protocol"></a>Protocolo HTTP:

* Contenedor único (lvaExtension único):  

   En el servidor de inferencia, puede usar un solo puerto, pero distintos puntos de conexión para distintos modelos de IA. Por ejemplo, para un ejemplo de Python, puede usar diferentes `route` por modelo, como: 

   ```
   @app.route('/score/face_detection', methods=['POST']) 
   … 
   Your code specific to face detection model… 

   @app.route('/score/vehicle_detection', methods=['POST']) 
   … 
   Your code specific to vehicle detection model 
   … 
   ```

   Y, después, en la implementación de Live Video Analytics, al crear una instancia de los grafos, establezca la dirección URL del servidor de inferencia para cada instancia del modo siguiente: 

   1\.ª instancia: URL del servidor de inferencia = `http://lvaExtension:44000/score/face_detection`<br/>
   2\.ª instancia: URL del servidor de inferencia = `http://lvaExtension:44000/score/vehicle_detection`  
    > [!NOTE]
    > Como alternativa, también puede exponer los modelos de IA en diferentes puertos y llamarlos al crear las instancias de los grafos.  

* Varios contenedores: 

   Cada contenedor se implementa con un nombre distinto. Actualmente, en el conjunto de documentación de Live Video Analytics, le mostramos cómo implementar una extensión con el nombre **lvaExtension**. Ahora puede desarrollar dos contenedores distintos. Cada contenedor tiene la misma interfaz HTTP (es decir, el mismo punto de conexión `/score`). Implemente estos dos contenedores con nombres diferentes y asegúrese de que ambos escuchen en **puertos distintos**. 

   Por ejemplo, un contenedor con el nombre `lvaExtension1` escucha el puerto `44000`, otro contenedor con el nombre `lvaExtension2` escucha el puerto `44001`. 

   En la topología de Live Video Analytics, se crean instancias de dos grafos con diferentes direcciones URL de inferencia, como: 

   Primera instancia: URL del servidor de inferencia = `http://lvaExtension1:44001/score`    
   Segunda instancia: URL del servidor de inferencia = `http://lvaExtension2:44001/score`
   
#### <a name="grpc-protocol"></a>Protocolo gRPC: 

Con el módulo Live Video Analytics 1.0, cuando se usa un protocolo gRPC, la única manera sería si el servidor de gRPC expusiera distintos modelos de IA a través de puertos distintos. En [este ejemplo](https://raw.githubusercontent.com/Azure/live-video-analytics/master/MediaGraph/topologies/grpcExtension/topology.json), hay un único puerto, 44000, que expone todos los modelos de yolo. En teoría, el servidor gRPC de yolo se podría volver a escribir para exponer algunos modelos en 44000, otros en 45000… 

Con el módulo Live Video Analytics 2.0, se agrega una nueva propiedad al nodo de extensión gRPC. Esta propiedad se denomina **extensionConfiguration**, que es una cadena opcional que se puede usar como parte del contrato gRPC. Cuando hay varios modelos de IA empaquetados en un único servidor de inferencia, no es necesario exponer un nodo para cada modelo de IA. En lugar de ello, en el caso de una instancia de grafo, el proveedor de la extensión (usted) puede definir cómo seleccionar los diferentes modelos de IA mediante la propiedad **extensionConfiguration** y, durante la ejecución, Live Video Analytics pasará esta cadena al servidor de inferencia, que puede usarla para invocar el modelo de IA deseado. 

### <a name="i-am-building-a-grpc-server-around-an-ai-model-and-want-to-be-able-to-support-being-used-by-multiple-camerasgraph-instances-how-should-i-build-my-server"></a>Quiero crear un servidor de gRPC en torno a un modelo de IA y poder usarlo en varias cámaras o instancias de grafos. ¿Cómo se debe crear el servidor? 

 En primer lugar, asegúrese de que el servidor pueda controlar más de una solicitud a la vez. O bien asegúrese de que el servidor funcione en subprocesos paralelos. 

Por ejemplo, en uno de los [ejemplos de gRPC de Live Video Analytics](https://github.com/Azure/live-video-analytics/blob/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/lvaextension/server/server.py), hay establecido un número predeterminado de canales paralelos. Consulte: 

```
server = grpc.server(futures.ThreadPoolExecutor(max_workers=3)) 
```

En la creación de instancias del servidor de gRPC anterior, el servidor solo puede abrir tres canales por cámara (por instancia de topología de grafos) a la vez. No debe intentar conectar más de tres instancias al servidor. Si intenta abrir más de tres canales, las solicitudes estarán pendientes hasta que se anule una existente.  

La implementación del servidor de gRPC anterior se usa en nuestros ejemplos de Python. Los desarrolladores pueden implementar sus propios servidores o, en la implementación predeterminada anterior, pueden aumentar el número de trabajos establecido al número de cámaras que se usan para obtener la fuente de vídeo. 

Para configurar y usar varias cámaras, los desarrolladores pueden crear varias instancias de la topología de grafos, donde cada instancia apunte al mismo servidor de inferencia o a otro distinto (por ejemplo, el servidor mencionado en el párrafo anterior). 

### <a name="i-want-to-be-able-to-receive-multiple-frames-from-upstream-before-i-make-an-inferencing-decision-how-can-i-enable-that"></a>Quiero poder recibir varios fotogramas del nivel superior antes de tomar una decisión de inferencia. ¿Cómo se puede habilitar? 

Los [ejemplos predeterminados](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis) actuales funcionan en modo "sin estado". En estos ejemplos no se mantiene el estado de las llamadas anteriores ni siquiera quién llamó (lo que significa que varias instancias de topología pueden llamar al mismo servidor de inferencia, y el servidor no podrá distinguir quién llama ni el estado por autor de la llamada). 

#### <a name="http-protocol"></a>Protocolo HTTP

Al usar el protocolo HTTP: 

Para mantener el estado, cada autor de llamada (instancia de topología de grafos) llamará al servidor de inferencia con el parámetro de consulta HTTP único para el autor de la llamada. Por ejemplo, la dirección URL del servidor de inferencia para  

1\.ª instancia de topología = `http://lvaExtension:44000/score?id=1`<br/>
2\.ª instancia de topología = `http://lvaExtension:44000/score?id=2`

… 

En el lado del servidor, la ruta de puntuación sabrá quién llama. Si id=1, puede mantener el estado por separado para ese autor de llamada (instancia de topología de grafos). Después, puede mantener los fotogramas de vídeo recibidos en un búfer (por ejemplo, una matriz, o un diccionario con una clave DateTime, y el valor es el fotograma) y, a continuación, puede definir el servidor para procesamiento (inferencia) después de que se reciban x fotogramas. 

#### <a name="grpc-protocol"></a>Protocolo gRPC 

Al usar el protocolo gRPC: 

Con una extensión gRPC, cada sesión es para una única fuente de cámara, por lo que no es necesario proporcionar un identificador. Por lo tanto, ahora con la propiedad extensionConfiguration, puede almacenar los fotogramas de vídeo en un búfer y definir el servidor de procesamiento (inferencia) después de que se reciban x fotogramas. 

### <a name="do-all-processmediastreams-on-a-particular-container-run-the-same-ai-model"></a>¿Ejecutan todos los ProcessMediaStreams de un contenedor determinado el mismo modelo de IA? 

No.  

Las llamadas de inicio o detención del usuario final en una instancia de grafos constituyen una sesión o quizás haya una desconexión/reconexión de la cámara. El objetivo es conservar una sesión si la cámara está transmitiendo vídeo. 

* Dos cámaras que envían vídeo para su procesamiento crean dos sesiones. 
* Una cámara que va a un grafo que tiene dos nodos gRPCExtension crea dos sesiones. 

Cada sesión es una conexión dúplex completa entre Live Video Analytics y el servidor gRPC, y cada sesión puede tener un modelo o una canalización diferentes. 

> [!NOTE]
> En el caso de una desconexión/reconexión de la cámara (donde la cámara se desconecta durante un período que supera los límites de tolerancia), Live Video Analytics abrirá una nueva sesión con el servidor gRPC. No es necesario que el servidor haga el seguimiento del estado en estas sesiones. 

Live Video Analytics también ha agregado compatibilidad con varias extensiones de gRPC para una sola cámara en una instancia de grafos. Podrá usar estas extensiones de gRPC para llevar a cabo el procesamiento de IA secuencialmente o en paralelo, o incluso puede tener una combinación de ambos. 

> [!NOTE]
> El hecho de que varias extensiones se ejecuten en paralelo afectará a los recursos de hardware, por lo que deberá tener esto en cuenta al elegir el hardware que se adapte a sus necesidades informáticas. 

### <a name="what-is-the-max--of-simultaneous-processmediastreams"></a>¿Cuál es el número máximo de ProcessMediaStreams simultáneos? 

Live Video Analytics no aplica ningún límite.  

### <a name="how-should-i-decide-if-my-inferencing-server-should-use-cpu-or-gpu-or-any-other-hardware-accelerator"></a>¿Cómo debo decidir si el servidor de inferencia debe usar la CPU o GPU o cualquier otro acelerador de hardware? 

Esto depende completamente de la complejidad del desarrollo del modelo de IA y de cómo el desarrollador quiera usar los aceleradores de hardware y CPU. Al desarrollar el modelo de IA, los desarrolladores pueden especificar qué recursos debe usar el modelo para realizar qué acciones. 

### <a name="how-do-i-store-images-with-bounding-boxes-post-processing"></a>¿Cómo se almacenan imágenes con cuadros de límite después del procesamiento? 

En la actualidad, las coordenadas del cuadro de límite solo se proporcionan como mensajes de inferencia. Los desarrolladores pueden crear un secuenciador MJPEG personalizado que pueda usar estos mensajes y superponer los cuadros de límite sobre los fotogramas de vídeo.  

## <a name="grpc-compatibility"></a>Compatibilidad con gRPC 

### <a name="how-will-i-know-what-the-mandatory-fields-for-the-media-stream-descriptor-are"></a>¿Cómo sabré cuáles son los campos obligatorios para el descriptor de flujo multimedia? 

A todo valor de campo que no se proporcione se le asignará un valor predeterminado [según lo especificado por gRPC](https://developers.google.com/protocol-buffers/docs/proto3#default).  

Live Video Analytics usa la versión **proto3** del lenguaje de búfer de protocolo. Todos los datos del búfer de protocolo que usan los contratos de Live Video Analytics están disponibles en los archivos de búfer de protocolo [definidos aquí](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc). 

### <a name="how-should-i-ensure-that-i-am-using-the-latest-protocol-buffer-files"></a>¿Cómo puedo asegurarme de que estoy usando los archivos de búfer de protocolo más recientes? 

Los archivos de búfer de protocolo más recientes se pueden [obtener aquí](https://github.com/Azure/live-video-analytics/tree/master/contracts/grpc). Cada vez que se actualicen los archivos de contrato, aparecerán en esta ubicación. Si bien no hay ningún plan inmediato para actualizar los archivos de protocolo, busque el nombre del paquete en la parte superior de los archivos para conocer la versión. Debería decir esto: 

```
microsoft.azure.media.live_video_analytics.extensibility.grpc.v1 
```

Todas las actualizaciones de estos archivos harán aumentar la porción "v-valor" al final del nombre. 

> [!NOTE]
> Dado que Live Video Analytics usa la versión de proto3 del lenguaje, los campos son opcionales, y esto lo hace compatible con versiones anteriores y posteriores. 

### <a name="what-grpc-features-are-available-for-me-to-use-with-live-video-analytics-which-features-are-mandatory-and-which-ones-are-optional"></a>¿Qué características de gRPC se pueden usar con Live Video Analytics? ¿Qué características son obligatorias y cuáles son opcionales? 

Se pueden usar todas las características de gRPC del lado servidor siempre que se cumpla el contrato protobuf. 

## <a name="monitoring-and-metrics"></a>Supervisión y métricas

### <a name="can-i-monitor-the-media-graph-on-the-edge-using-event-grid"></a>¿Se puede supervisar el grafo multimedia en el perímetro mediante Event Grid?

Sí. Puede consumir las métricas de Prometheus y publicarlas en la cuadrícula de eventos. 

### <a name="can-i-use-azure-monitor-to-view-the-health-metrics-and-performance-of-my-media-graphs-in-the-cloud-or-on-the-edge"></a>¿Se puede usar Azure Monitor para ver el estado, las métricas y el rendimiento de los grafos multimedia en la nube o en el perímetro?

Sí. Esta configuración es compatible. Obtenga más información sobre el [uso de métricas de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

### <a name="are-there-any-tools-to-make-it-easier-to-monitor-the-media-services-iot-edge-module"></a>¿Hay alguna herramienta que facilite la supervisión del módulo de IoT Edge de Media Services?

Visual Studio Code admite la extensión "Azure IoT Tools" que permite supervisar fácilmente los puntos de conexión del módulo LVAEdge. Puede usar esta herramienta para empezar a supervisar rápidamente el punto de conexión integrado de IoT Hub para "eventos" y ver los mensajes de inferencia que se enrutan desde el dispositivo perimetral hasta la nube. 

Además, puede usar esta extensión para editar el módulo gemelo para el módulo LVAEdge con el fin de modificar la configuración del grafo multimedia.

Para más información, vea el artículo [Supervisión y registro](monitoring-logging.md).

## <a name="billing-and-availability"></a>Facturación y disponibilidad

### <a name="how-is-live-video-analytics-on-iot-edge-billed"></a>¿Cómo se factura Live Video Analytics on IoT Edge?

Vea la [página de precios](https://azure.microsoft.com/pricing/details/media-services/) para obtener más detalles.

## <a name="next-steps"></a>Pasos siguientes

[Inicio rápido: Introducción: Live Video Analytics on IoT Edge](get-started-detect-motion-emit-events-quickstart.md)
