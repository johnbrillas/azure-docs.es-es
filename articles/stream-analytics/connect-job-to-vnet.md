---
title: Conexión de trabajos de Stream Analytics a recursos en una instancia de Azure Virtual Network (VNet)
description: En este artículo se describe cómo conectar un trabajo de Azure Stream Analytics con recursos que se encuentran en una VNet.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/23/2020
ms.custom: devx-track-js
ms.openlocfilehash: a2d26689a3036f05d7c8e7f417fbbb447402aedc
ms.sourcegitcommit: 799f0f187f96b45ae561923d002abad40e1eebd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/24/2020
ms.locfileid: "97762867"
---
# <a name="connect-stream-analytics-jobs-to-resources-in-an-azure-virtual-network-vnet"></a>Conexión de trabajos de Stream Analytics a recursos en una instancia de Azure Virtual Network (VNet)

Los trabajos de Stream Analytics realizan conexiones salientes a los recursos de Azure de entrada y salida para procesar datos en tiempo real y generar resultados. Estos recursos de entrada y salida (por ejemplo, Azure Event Hubs y Azure SQL Database) pueden estar detrás de un firewall de Azure o de una instancia de Azure Virtual Network (VNet). El servicio de Stream Analytics funciona desde redes que no se pueden incluir directamente en las reglas de red.

Sin embargo, hay dos formas de conectar de forma segura los trabajos de Stream Analytics a los recursos de entrada y salida en estos escenarios.
* Uso de puntos de conexión privados de los clústeres de Stream Analytics.
* Uso del modo de autenticación de identidad administrada junto con la configuración de red "Permitir servicios de confianza".

El trabajo de Stream Analytics no acepta ninguna conexión entrante.

## <a name="private-endpoints-in-stream-analytics-clusters"></a>Puntos de conexión privados de los clústeres de Stream Analytics.
El [clúster de Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/cluster-overview) es un clúster de proceso dedicado de un solo inquilino en el que se pueden ejecutar los trabajos de Stream Analytics. Puede crear puntos de conexión privados administrados en el clúster de Stream Analytics, lo que permite que los trabajos que se ejecutan en el clúster realicen una conexión de salida segura a los recursos de entrada y salida.

La creación de puntos de conexión privados en el clúster de Stream Analytics es una [operación de dos pasos](https://docs.microsoft.com/azure/stream-analytics/private-endpoints). Esta opción es más adecuada para cargas de trabajo de streaming de tamaño medio a grande, ya que el tamaño mínimo de un clúster de Stream Analytics es de 36 SU (aunque los 36 SU se pueden compartir en distintos trabajos en varias suscripciones o en entornos como los de desarrollo, prueba y producción).

## <a name="managed-identity-authentication-with-allow-trusted-services-configuration"></a>Autenticación de identidad administrada con la configuración "Permitir servicios de confianza"
Algunos servicios de Azure ofrecen la opción de configuración de redes **Permitir servicios de Microsoft de confianza**, que cuando está habilitada, permite que los trabajos de Stream Analytics se conecten de forma segura a su recurso mediante autenticación sólida. Esta opción permite conectar los trabajos a los recursos de entrada y salida sin necesidad de un clúster de Stream Analytics ni puntos de conexión privados. La configuración del trabajo para usar esta técnica es una operación de dos pasos:
* Use el modo de autenticación de identidad administrada al configurar la entrada o salida en el trabajo de Stream Analytics.
* Conceda a los trabajos específicos de Stream Analytics acceso explícito a los recursos de destino mediante la asignación de un rol de Azure a la identidad administrada asignada por el sistema del trabajo. 

Al habilitar **Permitir servicios de Microsoft de confianza**, no se concede acceso global a ningún trabajo. Esto le proporciona control total sobre los trabajos específicos de Stream Analytics que pueden tener acceso a los recursos de forma segura. 

Los trabajos pueden conectarse a los siguientes servicios de Azure mediante esta técnica:
1. [Blob Storage o Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/stream-analytics/blob-output-managed-identity): puede ser la entrada o la salida de streaming o la cuenta de almacenamiento del trabajo.
2. [Azure Event Hubs](https://docs.microsoft.com/azure/stream-analytics/event-hubs-managed-identity): puede ser la entrada o la salida de streaming del trabajo.

Si los trabajos necesitan conectarse a otros tipos de entrada o salida, la única opción es usar puntos de conexión privados en clústeres de Stream Analytics.

## <a name="next-steps"></a>Pasos siguientes

* [Creación y eliminación de puntos de conexión privados en un clúster de Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/private-endpoints)
* [Conexión a Event Hubs en una VNet mediante la autenticación de identidad administrada](https://docs.microsoft.com/azure/stream-analytics/event-hubs-managed-identity)
* [Conexión a Blob Storage y ADLS Gen2 en una VNet mediante la autenticación de identidad administrada](https://docs.microsoft.com/azure/stream-analytics/blob-output-managed-identity)
