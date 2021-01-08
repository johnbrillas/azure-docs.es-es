---
title: archivo de inclusión
description: archivo de inclusión
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 11/24/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: ce906ad62b51956cb85f854846740fa09e06895d
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97665208"
---
Event Hubs organiza las secuencias de eventos en una o varias particiones. A medida que llegan eventos más recientes, se agregan al final de esta secuencia. Una partición puede considerarse como un "registro de confirmación".

Las particiones almacenan datos de eventos que contienen el cuerpo del evento, un contenedor de propiedades definido por el usuario que describe el evento y metadatos, como su desplazamiento en la partición, su número en la secuencia de la transmisión y la marca de tiempo del servicio en el momento que se aceptó.

![Diagrama que muestra la secuencia de eventos, de más antiguo o más reciente.](./media/event-hubs-partitions/partition.png)

Event Hubs está diseñado para ayudar en el procesamiento de grandes volúmenes de eventos y la creación de particiones ayuda a hacerlo de dos maneras:

En primer lugar, aunque Event Hubs sea un servicio PaaS, hay una realidad física subyacente, y el mantenimiento de un registro que conserva el orden de los eventos requiere que estos eventos se mantengan juntos en el almacenamiento subyacente y sus réplicas, y esto crea un límite máximo de rendimiento para este tipo de registro. La creación de particiones permite que se usen varios registros paralelos para el mismo centro de eventos y, por lo tanto, multiplicar la capacidad de rendimiento de E/S sin procesar disponible.

En segundo lugar, sus propias aplicaciones deben poder mantener el procesamiento del volumen de eventos que se envía a un centro de eventos. Esto puede ser bastante complejo y requiere una capacidad de procesamiento en paralelo importante con escalabilidad horizontal. El razonamiento para la creación de particiones es el mismo que el anterior: la capacidad de un único proceso para controlar los eventos es limitada y, por tanto, se necesitan varios procesos, y las particiones son el modo en que la solución alimenta esos procesos y, además, garantiza que cada evento tiene un claro propietario de procesamiento. 

Event Hubs retiene los eventos durante el tiempo de retención configurado, que se aplica a todas las particiones. Los eventos se eliminan automáticamente cuando se alcanza el período de retención. Si especifica un período de retención de un día, el evento dejará de estar disponible exactamente 24 horas después de que se haya aceptado. No se pueden eliminar eventos explícitamente. 

El tiempo de retención permitido es de hasta 7 días para Event Hubs Estándar y hasta 90 días para Event Hubs dedicado. Si necesita archivar eventos más allá del período de retención permitido, puede hacer que se [almacenen automáticamente en Azure Storage o Azure Data Lake mediante la activación de la característica Capture de Event Hubs](../articles/event-hubs/event-hubs-capture-overview.md) y, si necesita buscar o analizar estos archivos, puede [importarlos fácilmente en Azure Synapse](../articles/event-hubs/store-captured-data-data-warehouse.md) u otros almacenes y plataformas de análisis similares. 

La razón del límite de Event Hubs en la retención de datos basada en el tiempo es evitar que grandes volúmenes de datos históricos del cliente se mantengan en un almacén que solo está indexado por una marca de tiempo y solo permite el acceso secuencial. La filosofía arquitectónica aquí es que los datos históricos necesitan una indexación más enriquecida y un acceso más directo que la interfaz de eventos en tiempo real que proporcionan Event Hubs o Kafka. Los motores de flujos de eventos no son adecuados para desempeñar el papel de los lagos de datos o los archivos a largo plazo para la creación de orígenes de eventos. 

Dado que las particiones son independientes y contienen sus propias secuencias de datos, a menudo crecen a velocidades diferentes. En Event Hubs, eso no supone ningún problema que requiera intervención administrativa, como sería, por ejemplo, en Apache Kafka, pero la distribución desigual conducirá a una carga desigual en los procesadores de eventos de nivel inferior.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

El número de particiones se especifica en el momento de la creación y debe estar comprendido entre 1 y 32 en Event Hubs Estándar. El número de particiones puede llegar hasta 2000 particiones por unidad de capacidad en Event Hubs dedicado. 

Se recomienda elegir al menos tantas particiones como [unidades de procesamiento (TU)](../articles/event-hubs/event-hubs-faq.md#what-are-event-hubs-throughput-units) sostenidas que espera necesitar durante la carga máxima de la aplicación para ese centro de eventos específico. Debe hacer los cálculos con una sola partición que tenga una capacidad de procesamiento de 1 TU (1 MB de entrada, 2 MB de salida). Puede escalar las unidades de procesamiento del espacio de nombres o las unidades de capacidad del clúster independientemente del número de particiones. Un centro de eventos con 32 particiones y un centro de eventos con 1 partición incurren en el mismo costo cuando el espacio de nombres se establece en una capacidad de 1 TU. 

Las aplicaciones controlan la asignación de eventos a las particiones mediante una de estas tres maneras:

- Al especificar una clave de partición, que se asigna de forma coherente (mediante una función hash) a una de las particiones disponibles. 
- Cuando no se especifica una clave de partición, lo que permite al agente elegir aleatoriamente una partición para un evento determinado.
- Mediante el envío explícito de eventos a una partición específica.

La especificación de una clave de partición permite mantener los eventos relacionados en la misma partición y en el orden exacto en el que se enviaron. La clave de partición es una cadena que se deriva del contexto de la aplicación e identifica la interrelación de los eventos.

Una secuencia de eventos identificados por una clave de partición es un *flujo*. Una partición es un almacén de registros multiplexado para muchos de estos flujos. 

Se puede aumentar el número de particiones de un centro de eventos después de crear el centro de eventos, pero la distribución de secuencias entre particiones cambiará cuando se realice como la asignación de claves de partición a cambios de particiones, por lo que debe intentar evitar estos cambios si el orden relativo de los eventos es importante en la aplicación.

Establecer el número de particiones en el valor máximo permitido es tentador, pero siempre debe tener en cuenta que los flujos de eventos deben estar estructurados de manera que se puedan aprovechar las ventajas de tener varias particiones. Si necesita conservar el orden absoluto en todos los eventos o solo en un puñado de subsecuencias, es posible que no pueda aprovechar las ventajas de tener muchas particiones. Además, muchas particiones hacen que el procesamiento sea más complejo. 

Aunque el envío directo a las particiones es posible, no se recomienda. En su lugar, puede usar las construcciones de nivel superior que se presentan en la sección [Publicador de eventos](../articles/event-hubs/event-hubs-features.md#event-publishers). 

Para más información acerca de particiones y el equilibrio entre disponibilidad y confiabilidad, consulte los artículos [Guía de programación de Event Hubs](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) y [Disponibilidad y coherencia en Event Hubs](../articles/event-hubs/event-hubs-availability-and-consistency.md).
