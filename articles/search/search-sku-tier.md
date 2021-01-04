---
title: Elija un plan de tarifa.
titleSuffix: Azure Cognitive Search
description: 'Azure Cognitive Search puede aprovisionarse en estos niveles: Gratis, Básico o Estándar; este último está disponible en varias configuraciones de recursos y niveles de capacidad.'
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/15/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 062bd41b0803cbb08f74fbcbcebb89bbddeb0d45
ms.sourcegitcommit: 77ab078e255034bd1a8db499eec6fe9b093a8e4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559810"
---
# <a name="choose-a-pricing-tier-for-azure-cognitive-search"></a>Selección de un plan de tarifa de Azure Cognitive Search

Al [crear un servicio de búsqueda](search-create-service-portal.md), debe elegir un plan de tarifa que es fijo para el período de vigencia del servicio. El plan que seleccione determina:

+ Cantidad de índices y otros objetos que puede crear (límites máximos)
+ Tamaño y velocidad de particiones (almacenamiento físico)
+ Tarifa facturable, un costo mensual fijo, pero también un costo incremental si agrega particiones o réplicas

Además, algunas [características Premium](#premium-features) tienen requisitos de plan.

## <a name="tier-descriptions"></a>Descripciones de los planes

Los planes incluyen **Gratis**, **Básico**, **Estándar** y **Almacenamiento optimizado**. Estándar y Almacenamiento optimizado están disponibles con varias configuraciones y capacidades.

En la siguiente captura de pantalla de Azure Portal se muestran los niveles disponibles, menos los precios (que puede encontrar en el portal y en la [página de precios](https://azure.microsoft.com/pricing/details/search/). 

![Planes de tarifa de Azure Cognitive Search](media/search-sku-tier/tiers.png "Planes de tarifa de Azure Cognitive Search")

El plan de tarifa **Gratis** crea un servicio de búsqueda limitado para proyectos más pequeños, como la ejecución de tutoriales y ejemplos de código. Internamente, las réplicas y las particiones se comparten entre varios suscriptores. No se puede escalar el servicio gratis, ni se pueden ejecutar grandes cargas de trabajo.

**Básico** y **Estándar** son los niveles facturables usados con mayor frecuencia, mientras que el valor predeterminado es **Estándar**. Con recursos dedicados bajo su control, puede implementar proyectos más grandes, optimizar el rendimiento y aumentar la capacidad.

Algunos niveles están optimizados para determinados tipos de trabajo. Por ejemplo, **Estándar 3 Alta densidad (S3 HD)** es un *modo de hospedaje* para S3, donde el hardware subyacente está optimizado para un gran número de índices más pequeños y está pensado para escenarios de varios inquilinos. S3 HD tiene el mismo cargo por unidad que S3, pero el hardware está optimizado para lecturas de archivos rápidas en un gran número de índices más pequeños.

Los niveles **Almacenamiento optimizado** ofrecen mayor capacidad de almacenamiento a un precio menor por TB que los niveles Estándar. El principal inconveniente es una mayor latencia de consulta, lo cual debe validar para sus requisitos de aplicación específica. Para saber más sobre las consideraciones de rendimiento de este nivel, vea [Estrategias de implementación y procedimientos recomendados para optimizar el rendimiento en Azure Search](search-performance-optimization.md).

Encontrará más información sobre los distintos niveles en la [página de precios](https://azure.microsoft.com/pricing/details/search/) del artículo [Límites de servicio en Azure Cognitive Search](search-limits-quotas-capacity.md) y en la página del portal cuando vaya a aprovisionar un servicio.

<a name="premium-features"></a>

## <a name="feature-availability-by-tier"></a>Disponibilidad de características por nivel

La mayoría de las características están disponibles en todos los niveles, incluido el nivel gratis. En algunos casos, el nivel que elija afectará a su capacidad para implementar una característica. En la tabla siguiente se describen las restricciones de características relacionadas con el nivel de servicio.

| Característica | Limitaciones |
|---------|-------------|
| [Indizadores](search-indexer-overview.md) | Los indizadores no están disponibles en S3 HD.  |
| [Enriquecimiento con IA](search-security-manage-encryption-keys.md) | Se ejecuta en el nivel Gratis, pero no se recomienda. |
| [Claves de cifrado administradas por el cliente](search-security-manage-encryption-keys.md) | No disponibles en el nivel Gratis. |
| [Acceso al firewall de IP](service-configure-firewall.md) | No disponibles en el nivel Gratis. |
| [Punto de conexión privado (integración con Azure Private Link)](service-create-private-endpoint.md) | En el caso de las conexiones entrantes a un servicio de búsqueda, no está disponible en el nivel Gratis. En el caso de las conexiones salientes de indexadores a otros recursos de Azure, no es´ta disponible en el nivel Gratis ni S3 HD. Para los indexadores que usan conjuntos de aptitudes, no está disponible en los niveles Gratis, Básico, S1 o S3 HD.|

Es posible que las características que consumen muchos recursos no funcionen bien a menos que se le proporcione capacidad suficiente. Por ejemplo, el [enriquecimiento con IA](cognitive-search-concept-intro.md) contiene funciones de ejecución prolongada que agotan el tiempo de espera en un servicio Gratis, a menos que el conjunto de datos sea pequeño.

## <a name="billable-events"></a>Eventos facturables

Una solución basada en Azure Cognitive Search puede incurrir en costos de las siguientes maneras:

+ [Costo del propio servicio](#service-costs), que se ejecuta de forma ininterrumpida con una configuración mínima (una partición y una réplica) a la tasa base. Puede considerar esto como el costo fijo de ejecutar el servicio.

+ Adición de capacidad (réplicas o particiones), donde los costos aumentan en incrementos de la tasa facturable

+ Cargos de ancho de banda (transferencia de datos salientes)

+ Servicios complementarios necesarios para funcionalidades o características específicas:

  + Enriquecimiento con inteligencia artificial (requiere [Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/))
  + almacén de conocimiento (requiere [Azure Storage](https://azure.microsoft.com/pricing/details/storage/))
  + enriquecimiento incremental (requiere [Azure Storage](https://azure.microsoft.com/pricing/details/storage/), se aplica al enriquecimiento con inteligencia artificial)
  + claves administradas por el cliente y cifrado doble (requiere [Azure Key Vault](https://azure.microsoft.com/pricing/details/key-vault/))
  + puntos de conexión privados para un modelo de acceso sin Internet (requiere [Azure Private Link](https://azure.microsoft.com/pricing/details/private-link/))

### <a name="service-costs"></a>Costos de servicio

A diferencia de las máquinas virtuales u otros recursos que se pueden "pausar" para evitar cargos, un servicio de Azure Cognitive Search siempre está disponible en el hardware dedicado para su uso exclusivo. Así, la creación de un servicio es un evento facturable que se inicia cuando se crea el servicio y termina cuando se elimina este. 

El cargo mínimo es la primera unidad de búsqueda (una réplica x una partición) a la tasa facturable. Este valor mínimo es fijo durante la vigencia del servicio porque el servicio no se puede ejecutar en ninguna otra configuración menor que esta. Más allá del mínimo, puede agregar réplicas y particiones por separado. Los aumentos incrementales de la capacidad mediante réplicas y particiones aumentan la factura en función de la siguiente fórmula: [(réplicas x particiones x tarifa)](#search-units), donde la tarifa que se le cobra depende del plan de tarifa que seleccione.

Cuando esté calculando el costo de una solución de búsqueda, tenga en cuenta que los precios y la capacidad no son lineales (cuanto más se duplica la capacidad, más se duplica el costo). Para ver un ejemplo de cómo funciona la fórmula, eche un vistazo a [Cómo asignar particiones y réplicas](search-capacity-planning.md#how-to-allocate-replicas-and-partitions).

### <a name="bandwidth-charges"></a>Cargos de ancho de banda

El uso de [indexadores](search-indexer-overview.md) puede afectar a la facturación si el origen de datos de Azure está en una región diferente de Azure Cognitive Search. En este escenario, se trata de un costo para mover datos salientes desde el origen de datos de Azure a Azure Cognitive Search. 

Puede eliminar por completo los cargos de salida de datos si crea el servicio Azure Cognitive Search en la misma región que los datos. Para saber más, eche un vistazo a la [página de precios de ancho de banda](https://azure.microsoft.com/pricing/details/bandwidth/):

+ Microsoft no cobra por los datos de entrada a ningún servicio de Azure.
+ No hay ningún cargo por los datos de salida de Azure Cognitive Search. Por ejemplo, si el servicio de búsqueda está en la región Oeste de EE. UU. y una aplicación web de Azure está en la región Este de EE. UU., Microsoft no cobrará por las cargas de respuesta de la consulta que se originen en el oeste de EE. UU.
+ En soluciones de varios servicios, no hay ningún cargo por los datos que atraviesan la conexión cuando todos los servicios están en la misma región.

Si hay servicios en regiones diferentes, se aplican cargos por los datos de salida. Estos cargos realmente no forman parte de la factura de Azure Cognitive Search. Se mencionan aquí porque si usa datos o indexadores enriquecidos con inteligencia artificial para extraer datos de distintas regiones, verá los costos reflejados en su factura general.

### <a name="ai-enrichment-with-cognitive-services"></a>Enriquecimientos de inteligencia artificial con Cognitive Services

Para el [enriquecimiento de inteligencia artificial](cognitive-search-concept-intro.md), debe planear [asociar un recurso facturable de Azure Cognitive Services](cognitive-search-attach-cognitive-services.md), en la misma región que Azure Cognitive Search, en el plan de tarifa S0 para el procesamiento de pago por uso. No hay ningún costo fijo por asociar Cognitive Services. Solo se paga por el procesamiento que necesita.

| Operación | Impacto en la facturación |
|-----------|----------------|
| Descifrado de documentos, extracción de texto | Gratuito |
| Descifrado de documentos, extracción de imágenes | Se factura según el número de imágenes extraídas de los documentos. En una [configuración de indexador](/rest/api/searchservice/create-indexer#indexer-parameters), **imageAction** es el parámetro que desencadena la extracción de imágenes. Si **imageAction** está establecido en "none" (valor predeterminado), no se le cobrará por la extracción de imágenes. La tarifa por extracción de imágenes está documentada en la página de [detalles de precios](https://azure.microsoft.com/pricing/details/search/) de Azure Cognitive Search.|
| [Aptitudes cognitivas integradas](cognitive-search-predefined-skills.md) | Se facturan a la misma tarifa que si se hubiera realizado la tarea mediante Cognitive Services directamente. |
| Aptitudes personalizadas | Una aptitud personalizada es funcionalidad que proporciona el usuario. El costo de usar una aptitud personalizada depende completamente de si el código personalizado está llamando a otros servicios de uso medido. |

La característica [enriquecimiento incremental (versión preliminar)](cognitive-search-incremental-indexing-conceptual.md) le posibilita proporcionar una memoria caché que permite que el indexador sea más eficaz al ejecutar solo las aptitudes cognitivas necesarias si modifica el conjunto de aptitudes en el futuro, lo que le ahorra tiempo y dinero.

<a name="search-units"></a>

## <a name="billing-formula-r-x-p--su"></a>Fórmula de facturación (R x P = SU)

El concepto de facturación más importante que se debe entender para las operaciones de Azure Cognitive Search es la *unidad de búsqueda* (SU). Dado que Azure Cognitive Search depende de las réplicas y las particiones para la indexación y las consultas, no tiene sentido facturar solo por una o la otra. En su lugar, la facturación se basa en una composición de ambas.

Una SU es el producto de las *réplicas* y las *particiones* usadas por un servicio: **(R x P = SU)** .

Cada servicio se inicia como mínimo con una SU (una réplica multiplicada por una partición). El máximo para cualquier servicio es 36 unidades de búsqueda. Este valor máximo se puede conseguir de varias maneras: por ejemplo, 6 particiones por 6 réplicas, o 3 particiones por 12 réplicas. Es habitual usar menos capacidad total (por ejemplo, un servicio de 3 réplicas y 3 particiones facturado como 9 SU). Vea el gráfico [Combinaciones de particiones y réplicas](search-capacity-planning.md#chart) para ver las combinaciones válidas.

La tarifa de facturación es de una hora por unidad. Cada nivel tiene una tarifa mayor de forma progresiva. Los niveles superiores incluyen particiones mayores y más rápidas, y esto contribuye a un total superior para ese nivel. Puede ver las tarifas de cada nivel en la página [detalles de precios](https://azure.microsoft.com/pricing/details/search/).

La mayoría de los clientes solo ponen en línea una parte de la capacidad total, manteniendo el resto en la reserva. Para la facturación, el número de particiones y réplicas que tiene en línea, calculado mediante la fórmula de SU, determina lo que se paga por hora.

## <a name="next-steps"></a>Pasos siguientes

Cost Management es una parte fundamental del planeamiento de capacidad. Como próximo paso, continúe con el artículo siguiente para obtener instrucciones sobre cómo calcular la capacidad y administrar los costos.

> [!div class="nextstepaction"]
> [Administración de costos y estimación de la capacidad en Azure Cognitive Search](search-sku-manage-costs.md)