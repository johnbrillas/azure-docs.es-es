---
title: Vista de tráfico en Azure Traffic Manager
description: En esta introducción, aprenderá cómo funciona la vista de tráfico de Traffic Manager.
services: traffic-manager
documentationcenter: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 01/22/2021
ms.author: duau
ms.custom: ''
ms.openlocfilehash: 66376655c61903761d93ea228c6d72fa05734353
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98743056"
---
# <a name="traffic-manager-traffic-view"></a>Traffic View de Traffic Manager

Traffic Manager proporciona enrutamiento de nivel DNS (Sistema de nombres de dominio). Este servicio permite dirigir a los usuarios finales a puntos de conexión correctos en función del método de enrutamiento que prefiera. Vista de tráfico proporciona a Traffic Manager una visión de las bases de usuarios (en un nivel de granularidad de resolución DNS) y su patrón de tráfico. Cuando se habilita Traffic View, esta información se procesa para proporcionarle detalles sobre lo que actuar. 

Mediante el uso de Traffic View, puede:
- comprender dónde se encuentran las bases de usuarios (hasta una granularidad en el nivel de resolución DNS local).
- ver el volumen de tráfico (en forma de consultas DNS controladas por Azure Traffic Manager) que se origina en estas regiones.
- conocer la latencia representativa que experimentan estos usuarios.
- profundizar en los patrones de tráfico específicos de cada una de estas bases de usuarios para las regiones de Azure en la que tiene puntos de conexión. 

Por ejemplo, puede usar Vista de tráfico para entender qué regiones tienen una gran cantidad de tráfico pero experimentan latencias más altas. Luego puede usar esta información para planear la expansión de la superficie a nuevas regiones de Azure. De este modo, los usuarios tienen una experiencia de menor latencia.

## <a name="how-traffic-view-works"></a>Cómo funciona Traffic View

Vista de tráfico funciona mediante el examen de las consultas entrantes recibidas en los últimos siete días de un perfil. De la información de las consultas entrantes, Vista de tráfico extrae la dirección IP de origen de la resolución DNS que se usa para representar la ubicación de los usuarios. Esta información se agrupa en un nivel de resolución DNS para crear regiones de base de usuarios. Traffic Manager mantiene la información geográfica de las direcciones IP. Luego, examina las regiones de Azure a las que se ha enrutado la consulta y elabora un mapa de flujo de tráfico de los usuarios de esas regiones.
 
En el paso siguiente, Traffic Manager pone en correlación la región de base de usuarios para la asignación de región de Azure con las tablas de latencia de inteligencia de red. Esta tabla se mantiene para diferentes redes de usuarios finales a fin de entender la latencia media que experimentan los usuarios de esas regiones cuando se conectan a regiones de Azure. Todos estos cálculos se combinan por nivel de dirección IP de resolución DNS local antes de presentarse. Puede utilizar la información de varias maneras.

La frecuencia de actualización de datos de la vista Tráfico depende de varias variables de servicio interno. Pero los datos se actualizan cada 24 horas.

>[!NOTE]
>La latencia descrita en Vista de tráfico es una latencia representativa entre el usuario final y las regiones de Azure a las que se habían conectado y no es la latencia de búsqueda DNS. Vista de tráfico hace una estimación de mejor esfuerzo de la latencia entre la resolución DNS local y la región de Azure a la que se enrutó la consulta; si no hay suficientes datos disponibles, la latencia devuelta será nula. 

## <a name="visual-overview"></a>Información general visual

Cuando se va a la sección **Vista de tráfico** de la página de Traffic Manager, se ve un mapa geográfico con una superposición de conclusiones de Vista de tráfico. El mapa proporciona información acerca de la base de usuarios y los puntos de conexión para el perfil de Traffic Manager.

![Vista geográfica de vista de tráfico de Traffic Manager][1]

### <a name="user-base-information"></a>Información de base de usuarios

Las resoluciones DNS locales cuya información de ubicación está disponible se muestran en el mapa. El color de las resoluciones DNS muestra la latencia media experimentada por los usuarios finales que han usado esa resolución DNS para las consultas de Traffic Manager.

Si mantiene el mouse sobre una ubicación de la resolución DNS en el mapa, se muestra:
- la dirección IP de la resolución DNS.
- el volumen de tráfico de consultas DNS visto por Traffic Manager desde él.
- los puntos de conexión a los que se enrutó el tráfico desde la resolución DNS, como una línea entre el punto de conexión y la resolución DNS. 
- la latencia media desde esa ubicación al punto de conexión, representada como el color de la línea que los conecta.

### <a name="endpoint-information"></a>Información de punto de conexión

Las regiones de Azure en las que se ubican los puntos de conexión se muestran como puntos azules en el mapa. Si el punto de conexión es externo y no tiene una región de Azure asignada, se muestra en la parte superior del mapa. Seleccione cualquier punto de conexión para ver las distintas ubicaciones (según la resolución DNS usada) desde donde se ha dirigido el tráfico a ese punto de conexión. Las conexiones se muestran como una línea entre el punto de conexión y la ubicación de la resolución DNS. Están coloreadas según la latencia representativa entre ese par. Puede ver el nombre del punto de conexión y la región de Azure en la que se ejecuta. También se muestra el volumen total de solicitudes que este perfil de Traffic Manager dirige ahí.


## <a name="tabular-listing-and-raw-data-download"></a>Lista tabular y descarga de datos sin procesar

Puede ver los datos de Vista de tráfico en un formato tabular en Azure Portal. Hay una entrada para cada par de dirección IP o punto de conexión de la resolución DNS que muestra:

* La dirección IP de la resolución DNS.
* Nombre.
* La ubicación geográfica de la región de Azure en la que se encuentra el punto de conexión (si está disponible).
* El volumen de solicitudes asociadas a esa resolución DNS para ese punto de conexión.
* La latencia representativa asociada a los usuarios finales que usan ese DNS (si está disponible). 

También puede descargar los datos de Vista de tráfico como un archivo CSV que puede usarse como parte de un flujo de trabajo de análisis de su elección.

## <a name="billing"></a>Facturación

Cuando use Vista de tráfico, se le facturará en función del número de puntos de datos usados para crear las conclusiones presentadas. Actualmente, el único tipo de punto de datos utilizado son las consultas que recibe el perfil de Traffic Manager. Para más detalles sobre los precios, visite la [página de precios de Traffic Manager](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="faqs"></a>Preguntas más frecuentes

* [¿Cómo funciona Traffic View?](./traffic-manager-faqs.md#what-does-traffic-view-do)

* [¿Cuál es el beneficio de usar Traffic View?](./traffic-manager-faqs.md#how-can-i-benefit-from-using-traffic-view)

* [¿En qué se diferencia Traffic View de las métricas de Traffic Manager disponibles mediante Azure Monitor?](./traffic-manager-faqs.md#how-is-traffic-view-different-from-the-traffic-manager-metrics-available-through-azure-monitor)

* [¿Usa Traffic View información de subred de cliente de EDNS?](./traffic-manager-faqs.md#does-traffic-view-use-edns-client-subnet-information)

* [¿Cuántos días de datos usa Traffic View?](./traffic-manager-faqs.md#how-many-days-of-data-does-traffic-view-use)

* [¿Cómo controla Traffic View los puntos de conexión externos?](./traffic-manager-faqs.md#how-does-traffic-view-handle-external-endpoints)

* [¿Es necesario habilitar Traffic View en cada perfil de la suscripción?](./traffic-manager-faqs.md#do-i-need-to-enable-traffic-view-for-each-profile-in-my-subscription)

* [¿Cómo se desactiva Traffic View?](./traffic-manager-faqs.md#how-can-i-turn-off-traffic-view)

* [¿Cómo funciona la facturación de Traffic View?](./traffic-manager-faqs.md#how-does-traffic-view-billing-work)

## <a name="next-steps"></a>Pasos siguientes

- Aprenda [cómo funciona el Administrador de tráfico](traffic-manager-overview.md)
- Aprenda más sobre los [métodos de enrutamiento de tráfico](traffic-manager-routing-methods.md) que admite el Administrador de tráfico.
- Aprenda a [crear un perfil del Administrador de tráfico](./quickstart-create-traffic-manager-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-traffic-view-overview/trafficview.png