---
title: 'Aplicaciones y tareas de replicación de eventos: Azure Event Hubs | Microsoft Docs'
description: En este artículo se proporciona información general sobre la compilación de aplicaciones y tareas de replicación de eventos con Azure Functions.
ms.topic: article
ms.date: 12/01/2020
ms.openlocfilehash: a65815c7da400af8b5b6d46358e6bca6edbd7543
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97663552"
---
# <a name="event-replication-tasks-and-applications-with-azure-functions"></a>Aplicaciones y tareas de replicación de eventos con Azure Functions

> [!TIP]
> Para todas las tareas de replicación con estado en las que necesite tener en cuenta las cargas de los eventos y transformarlas, agregarlas, enriquecerlas o reducirlas, use [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) en lugar de Azure Functions.

Como se explica en el artículo de [federación entre regiones y replicación de eventos](event-hubs-federation-overview.md), la replicación sin estado de secuencias de eventos entre pares de Event Hubs y entre Event Hubs y otros orígenes y destinos de secuencias de eventos se apoya en Azure Functions.

[Azure Functions](../azure-functions/functions-overview.md) es un entorno de ejecución escalable y confiable para configurar y ejecutar aplicaciones sin servidor, incluidas las tareas de federación y replicación de eventos.

En esta página de información general, obtendrá información sobre las funciones integradas de Azure Functions para estas aplicaciones, sobre los bloques de código que puede adaptar y modificar para las tareas de transformación y sobre cómo configurar una aplicación de Azure Functions de modo que se integre idealmente con Event Hubs y otros servicios de mensajería de Azure. Para gran parte de la información, este artículo apuntará a la documentación de Azure Functions.

[!INCLUDE [messaging-replicator-functions](../../includes/messaging-replicator-functions.md)]









