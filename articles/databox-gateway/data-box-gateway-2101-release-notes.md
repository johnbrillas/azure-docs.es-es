---
title: Notas de la versión 2101 de Azure Data Box Gateway | Microsoft Docs
description: Describe los problemas críticos pendientes y las soluciones para la versión 2101 de Azure Data Box Gateway.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/29/2021
ms.author: alkohli
ms.openlocfilehash: 510f2677673363791ab5eb0f2c4dbcd25b697934
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99072523"
---
# <a name="azure-data-box-gateway-2101-release-notes"></a>Notas de la versión 2101 de Azure Data Box Gateway

En las notas de la versión siguientes, se identifican los problemas críticos pendientes y los problemas resueltos de la versión 2101 de Azure Data Box Gateway.

Las notas de la versión se actualizan continuamente. A medida que se descubran incidencias críticas que requieran una solución alternativa, se irán agregando. Antes de implementar Azure Data Box Gateway, le recomendamos que lea detenidamente la información en las notas de la versión.  

Esta versión se corresponde con las versiones de software siguientes:

- **Data Box Gateway 2101 (1.6.1475.2528)** - KB 4603462

> [!NOTE]
> La actualización 2101 solo se puede aplicar a los dispositivos que ejecutan las versiones de disponibilidad general del software o versiones posteriores.

## <a name="whats-new"></a>Novedades

Esta versión contiene la siguiente corrección de errores:

- **Problema de carga**: esta versión corrige un problema de carga en el que los reinicios de carga ocasionados por errores pueden ralentizar la velocidad de finalización de la carga. Este problema puede producirse al cargar un conjunto de datos que se compone principalmente de archivos de gran tamaño en relación con el ancho de banda disponible, en especial, entre otras cosas, cuando está activo el límite de ancho de banda. Este cambio garantiza una oportunidad suficiente para completar la carga antes de reiniciar la carga de un archivo determinado.

Esta versión también contiene las siguientes actualizaciones:

- Todas las actualizaciones acumulativas de Windows y actualizaciones de .NET Framework que se lanzaron hasta octubre de 2020.
- La dirección IP estática para Azure Data Box Gateway se conserva durante las actualizaciones de software.

## <a name="known-issues-in-this-release"></a>Problemas conocidos en esta versión

No se ha notificado ningún problema nuevo para esta versión. Todos los problemas notificados de la versión provienen de versiones anteriores. Para ver una lista de los problemas conocidos, vaya a [Problemas conocidos en la versión de disponibilidad general](data-box-gateway-release-notes.md#known-issues-in-ga-release).

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para implementar Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
