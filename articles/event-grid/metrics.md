---
title: Métricas compatibles con Azure Event Grid
description: En este artículo se proporcionan las métricas de Azure Monitor admitidas por el servicio Azure Event Grid.
ms.topic: conceptual
ms.date: 03/17/2021
ms.openlocfilehash: 321e318f9dab87fde20b33a6a3a906b020ada622
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104588739"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Métricas compatibles con Azure Event Grid
En este artículo se proporcionan listas de métricas de Event Grid clasificadas por espacios de nombres. 

## <a name="system-topics"></a>Temas del sistema

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="custom-topics"></a>Temas personalizados

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|

## <a name="domains"></a>Dominios

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, Error, ErrorType|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Topic, EventSubscriptionName, DomainEventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|Topic, ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Tema|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|

## <a name="event-subscriptions"></a>Suscripciones a eventos

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|Sin dimensiones|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|Sin dimensiones|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|Sin dimensiones|


## <a name="extension-topics"></a>Temas de extensión

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|

## <a name="partner-namespaces"></a>Espacios de nombres de los asociados

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|PublishSuccessLatencyInMs|Sí|Latencia de publicación correcta|Milisegundos|Total|Latencia de publicación correcta en milisegundos|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="partner-topics"></a>Temas de asociados

|Métrica|¿Se puede exportar con la configuración de diagnóstico?|Nombre de métrica para mostrar|Unidad|Tipo de agregación|Descripción|Dimensions|
|---|---|---|---|---|---|---|
|AdvancedFilterEvaluationCount|Sí|Evaluaciones de filtros avanzados|Count|Total|Total de filtros avanzados evaluados en las suscripciones de eventos de este tema.|EventSubscriptionName|
|DeadLetteredCount|Sí|Eventos en la cola de mensajes fallidos|Count|Total|Número total de eventos en la cola de eventos fallidos que coinciden con esta suscripción de eventos|DeadLetterReason, EventSubscriptionName|
|DeliveryAttemptFailCount|No|Eventos de error en la entrega|Count|Total|Número total de eventos que no se han podido entregar a esta suscripción de eventos|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Sí|Eventos entregados|Count|Total|Número total de eventos que se han entregado a esta suscripción de eventos|EventSubscriptionName|
|DestinationProcessingDurationInMs|No|Duración del procesamiento de destino|Milisegundos|Average|Duración del procesamiento de destino en milisegundos|EventSubscriptionName|
|DroppedEventCount|Sí|Eventos quitados|Count|Total|Número total de eventos quitados que coinciden con esta suscripción de eventos|DropReason, EventSubscriptionName|
|MatchedEventCount|Sí|Eventos coincidentes|Count|Total|Número total de eventos que coinciden con esta suscripción de eventos|EventSubscriptionName|
|PublishFailCount|Sí|Eventos no publicados|Count|Total|Número total de eventos que no se han podido publicar en este tema|ErrorType, Error|
|PublishSuccessCount|Sí|Eventos publicados|Count|Total|Número total de eventos publicados en este tema|Sin dimensiones|
|UnmatchedEventCount|Sí|Eventos no coincidentes|Count|Total|Número total de eventos que no coinciden con ninguna de las suscripciones de eventos de este tema|Sin dimensiones|


## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo siguiente: [Registros de diagnóstico](diagnostic-logs.md)
