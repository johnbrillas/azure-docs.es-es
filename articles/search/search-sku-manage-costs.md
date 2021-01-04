---
title: Cálculo de capacidad y costos
titleSuffix: Azure Cognitive Search
description: Revise las instrucciones para estimar la capacidad y administrar los costos del servicio de búsqueda, incluida la infraestructura y las herramientas de Azure, así como las prácticas recomendadas para usar los recursos de manera más eficaz.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.openlocfilehash: d48ae71a979a2d0f1457b0cefa8a98a02710dd96
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577578"
---
# <a name="how-to-estimate-capacity-and-costs-of-an-azure-cognitive-search-service"></a>Cómo calcular la capacidad y los costos de un servicio de Azure Cognitive Search

Como parte del planeamiento de la capacidad de Azure Cognitive Search, las siguientes instrucciones pueden ayudarle a reducir costos o a administrarlos de forma más eficaz:

+ Cree todos los recursos en la misma región, o en el menor número de regiones posible, para minimizar o eliminar los cargos de ancho de banda.

+ Consolide todos los servicios en un grupo de recursos, como Azure Cognitive Search, Cognitive Services y cualquier otro servicio de Azure que se use en la solución. En Azure Portal, busque el grupo de recursos y use los comandos de **Administración de costos** para obtener información sobre el gasto real y previsto.

+ Considere Azure Web App como aplicación de front-end para que las solicitudes y las respuestas permanezcan dentro del límite del centro de datos.

+ Escale verticalmente las operaciones que consumen muchos recursos, como la indización, y vuelva a ajustar hacia abajo para las cargas de trabajo de consulta normales. Comience con la configuración mínima de Azure Cognitive Search (una SU compuesta de una partición y una réplica) y luego supervise la actividad de los usuarios para identificar patrones de uso que indiquen la necesidad de más capacidad. Si hay un patrón predecible, es posible que pueda sincronizar la escala con la actividad (necesitaría escribir código para automatizar esto).

Los eventos facturables, la fórmula de facturación y la tarifa facturable se explican en [Elección de un plan de tarifa](search-sku-tier.md). Asimismo, puede consultar [Facturación y administración de costos](../cost-management-billing/cost-management-billing-overview.md) para ver las herramientas y características integradas relacionadas con los gastos.

No es posible quitar un servicio de búsqueda de forma temporal. Los recursos dedicados siempre están operativos y se asignan para su uso exclusivo mientras el servicio esté vigente. La eliminación de un servicio es permanente y también elimina los datos asociados.

En lo que respecta al propio servicio, la única manera de reducir la factura es reducir las réplicas y las particiones a un nivel que aún proporcione rendimiento aceptable y [cumplimiento del SLA](https://azure.microsoft.com/support/legal/sla/search/v1_0/), o crear un servicio en un nivel inferior (las tarifas por hora de S1 son inferiores a las de S2 o S3). Supongamos que aprovisiona su servicio con la menor de las previsiones de carga; si se supera el servicio, puede crear un segundo servicio con un nivel mayor, regenerar los índices en el segundo servicio y eliminar el primero.

## <a name="how-to-evaluate-capacity-requirements"></a>Cómo evaluar los requisitos de capacidad

En Azure Cognitive Search, la capacidad se estructura como *réplicas* y *particiones*.

+ Las réplicas son instancias del servicio de búsqueda. Cada réplica hospeda una copia de carga equilibrada de un índice. Por ejemplo, un servicio con seis réplicas tiene seis copias de todos los índices cargados en el servicio.

+ Las particiones almacenan índices y dividen automáticamente los datos utilizables en búsquedas. Dos particiones dividen el índice por la mitad; tres particiones lo dividen en tercios y así sucesivamente. En términos de capacidad, el *tamaño de partición* es la principal característica diferenciadora entre niveles.

> [!NOTE]
> Todos los niveles Estándar y Almacenamiento optimizado admiten [combinaciones flexibles de réplicas y particiones](search-capacity-planning.md#chart) para que pueda [optimizar el sistema para velocidad o para almacenamiento](search-performance-optimization.md) con solo cambiar el equilibrio. El nivel Básico ofrece hasta tres réplicas para alta disponibilidad, pero solo tiene una partición. El nivel Gratis no proporciona recursos dedicados: los recursos de cálculo se comparten entre varios suscriptores.

### <a name="evaluating-capacity"></a>Evaluar la capacidad

La capacidad y los costos de ejecución del servicio están relacionados. Los planes de tarifa imponen límites en dos niveles: almacenamiento y contenido (número de índices, por ejemplo). Debe tener en cuenta estos dos aspectos, porque el límite real será el que se alcance primero.

Los requisitos empresariales suelen imponer el número de índices que se necesitará. Por ejemplo, podría necesitar un índice global para un repositorio de documentos de gran tamaño. O podría necesitar varios índices en función de la región, la aplicación o el nicho de negocio.

Para determinar el tamaño de un índice, tendrá que [crear uno](search-what-is-an-index.md). Su tamaño se basará en los datos importados y en la configuración del índice, por ejemplo, si habilita los proveedores de sugerencias, el filtrado y la ordenación.

Para la búsqueda de texto completo, la estructura de datos principal es una estructura de [índice invertido](https://en.wikipedia.org/wiki/Inverted_index), que tiene características diferentes de los datos de origen. Para un índice invertido, el tamaño y la complejidad vienen determinados por el contenido, y no necesariamente por la cantidad de datos que se incorporan. Un origen de datos de gran tamaño con mucha redundancia podría dar lugar a un índice más pequeño que un conjunto de datos más pequeño que incluya contenido muy variable. Así que es poco probable deducir el tamaño del índice en función del tamaño del conjunto de datos original.

> [!NOTE] 
> Aunque el cálculo de las necesidades futuras de índices y almacenamiento se hace a partir de suposiciones, vale la pena hacerlo. Si la capacidad de un nivel resulta demasiado baja, tendrá que aprovisionar un nuevo servicio en un nivel superior y luego [volver a cargar los índices](search-howto-reindex.md). No existe ninguna actualización local de un servicio de un nivel a otro.
>

## <a name="estimate-with-the-free-tier"></a>Estimación con el nivel Gratis

Un enfoque para calcular la capacidad es empezar con el nivel Gratis. Recuerde que el servicio Gratis ofrece hasta 3 índices, 50 MB de almacenamiento y 2 minutos de tiempo de indexación. Puede resultar complicado calcular un tamaño de índice proyectado con estas restricciones, pero estos son los pasos:

+ [Cree un servicio gratuito](search-create-service-portal.md).
+ Prepare un conjunto de datos pequeño y representativo.
+ [Genere un índice inicial en el portal](search-get-started-portal.md) y anote su tamaño. Las características y los atributos afectan al almacenamiento. Por ejemplo, al agregar proveedores de sugerencias (consultas al escribir), aumentan los requisitos de almacenamiento. Con el mismo conjunto de datos, puede intentar crear varias versiones de un índice, con atributos diferentes en cada campo, para ver cómo varían los requisitos de almacenamiento. Para obtener más información, vea ["Implicaciones de almacenamiento" en Creación de un índice básico](search-what-is-an-index.md#index-size).

Con una estimación aproximada, podría doblar esa cantidad a fin de presupuestar dos índices (desarrollo y producción) y luego elegir el nivel en consecuencia.

## <a name="estimate-with-a-billable-tier"></a>Estimación con un nivel facturable

Los recursos dedicados pueden adaptarse a mayores tiempos de muestreo y procesamiento para realizar estimaciones más realistas de cantidad de índices, tamaño y volúmenes de consulta durante el desarrollo. Algunos clientes pasan directamente a un nivel facturable y luego reevalúan a medida que el proyecto de desarrollo madura.

1. [Revise los límites del servicio en cada nivel](./search-limits-quotas-capacity.md#index-limits) para determinar si los niveles más bajos pueden admitir la cantidad de índices que necesita. En los niveles Básico, S1 y S2, los límites de índices son 15, 50 y 200 respectivamente. El nivel Almacenamiento optimizado tiene un límite de 10 índices porque se ha diseñado para admitir un número reducido de índices muy grandes.

1. [Cree un servicio en un nivel facturable](search-create-service-portal.md):

    + Comience por abajo, en Básico o S1, si no está seguro de la carga proyectada.
    + Empiece por arriba, con un nivel S2 o incluso S3, si sabe que va a tener cargas de consulta e indexación a gran escala.
    + Empiece con Almacenamiento optimizado, en L1 o L2, si va a indexar una gran cantidad de datos y la carga de consultas es relativamente baja, como con una aplicación empresarial interna.

1. [Genere un índice inicial](search-what-is-an-index.md) para determinar cómo se traducen los datos de origen a un índice. Esta es la única manera de calcular el tamaño del índice.

1. [Supervise el almacenamiento, los límites del servicio, el volumen de consultas y la latencia](search-monitor-usage.md) en el portal. El portal muestra las consultas por segundo, las consultas limitadas y la latencia de búsqueda. Todos estos valores pueden ayudarle a decidir si ha seleccionado el nivel correcto. 

El tamaño y la cantidad de índices son igualmente importantes para el análisis. Esto es así porque se alcanzan los límites máximos al usar por completo el almacenamiento (particiones) o al agotar todos los recursos (índices, indexadores, etc.), lo que ocurra primero. El portal le ayuda a realizar un seguimiento de ambos, mostrando el uso actual y los límites máximos en paralelo en la página Información general.

> [!NOTE]
> Los requisitos de almacenamiento pueden parecer excesivos si los documentos contienen datos extraños. Lo ideal es que los documentos contengan solo los datos que necesita para la experiencia de búsqueda. No es posible realizar búsquedas en datos binarios, así que deben almacenarse por separado (quizás en Azure Blob Storage o en una tabla de Azure). Después hay que agregar un campo en el índice para mantener una referencia de URL a los datos externos. El tamaño máximo de un documento individual es 16 MB (o menos si va a cargar de forma masiva varios documentos en una sola solicitud). Para más información, vea [Límites de servicio en Azure Cognitive Search](search-limits-quotas-capacity.md).
>

**Consideraciones del volumen de consultas**

Las consultas por segundo (QPS) son una medida importante durante la optimización del rendimiento, pero solo suele ser una consideración de nivel si espera un volumen elevado de consultas desde el principio.

El nivel Estándar puede proporcionar un equilibrio entre réplicas y particiones. Puede aumentar el tiempo de respuesta de las consultas mediante la adición de réplicas para el equilibrio de carga o agregar particiones para el procesamiento paralelo. Luego puede ajustar el rendimiento después de aprovisionar el servicio.

Si prevé grandes volúmenes de consultas sostenidos desde el principio, considere la opción de los niveles Estándar superiores, respaldados por un hardware más potente. Luego puede desconectar particiones y réplicas, o incluso cambiar a un servicio de nivel inferior, si no llega a tener los volúmenes de consultas previstos. Para más información sobre cómo calcular el rendimiento de las consultas, consulte [Rendimiento y optimización de Azure Cognitive Search](search-performance-optimization.md).

El nivel Almacenamiento optimizado es útil para cargas de trabajo de datos de gran tamaño, ya que admite más almacenamiento de índice general disponible para cuando los requisitos de latencia de consulta sean menos importantes. Tendrá que seguir usando réplicas adicionales para el equilibrio de carga y particiones adicionales para el procesamiento paralelo. Luego puede ajustar el rendimiento después de aprovisionar el servicio.

**Contratos de nivel de servicio**

El nivel Gratis y las características en vista previa no ofrecen [contratos de nivel de servicio (SLA)](https://azure.microsoft.com/support/legal/sla/search/v1_0/). Para todos los niveles facturables, los SLA tomarán efecto cuando se aprovisione suficiente redundancia para el servicio. Necesitará dos o más réplicas para los SLA de consulta (lectura). Necesitará tres o más réplicas para los SLA de consulta e indización (lectura-escritura). El número de particiones no afecta a los SLA.

## <a name="tips-for-tier-evaluation"></a>Sugerencias para la evaluación de nivel

+ Cree métricas sobre las consultas y recopile datos sobre los patrones de uso (consultas durante el horario laboral, indexación durante las horas de menor uso). Use estos datos para respaldar las decisiones de aprovisionamiento del servicio. Aunque no resulta práctico en una cadencia horaria o diaria, puede ajustar dinámicamente las particiones y los recursos para adaptar los cambios planeados en el volumen de consultas. También puede adaptar los cambios no planeados pero sostenidos si los niveles se mantienen lo suficiente para garantizar que se puedan realizar acciones.

+ Recuerde que la única desventaja de un aprovisionamiento inferior es que es posible que deba anular un servicio si los requisitos reales son mayores que los que había previsto. Para evitar la interrupción del servicio, se crea un nuevo servicio en un nivel superior y se ejecuta en paralelo hasta que todas las solicitudes y las aplicaciones se dirijan al nuevo punto de conexión.

## <a name="next-steps"></a>Pasos siguientes

Comience con un nivel Gratis y cree un índice inicial usando un subconjunto de los datos para comprender sus características. La estructura de datos de Azure Cognitive Search es una estructura de índice invertido. El tamaño y la complejidad de un índice invertido vienen determinados por el contenido. Recuerde que un contenido de gran redundancia tiende a producir un índice más pequeño que el contenido de alta irregularidad. Por tanto, son las características del contenido más que el tamaño del conjunto de datos lo que determina los requisitos de almacenamiento de índices.

Una vez que tenga un cálculo inicial del tamaño del índice, [aprovisione un servicio facturable](search-create-service-portal.md) en uno de los niveles que hemos descrito en este artículo: Básico, Estándar o Almacenamiento optimizado. Rebaje cualquier restricción artificial sobre el tamaño de los datos y [recompile el índice](search-howto-reindex.md) para que incluya todos los datos sobre los que quiera realizar búsquedas.

[Asigne particiones y réplicas](search-capacity-planning.md) según sea necesario para obtener el rendimiento y la escala que necesite.

Si la capacidad y el rendimiento son válidos, ya estará listo. En caso contrario, vuelva a crear un servicio de búsqueda en otro nivel que se adapte más estrechamente a sus necesidades.

> [!NOTE]
> Si tiene alguna duda, puede publicarla en [StackOverflow](https://stackoverflow.com/questions/tagged/azure-search) o [ponerse en contacto con el Soporte técnico de Azure](https://azure.microsoft.com/support/options/).