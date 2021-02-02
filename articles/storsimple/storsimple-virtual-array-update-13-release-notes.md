---
title: Notas de la versión de Microsoft Azure StorSimple Virtual Array Update 1.3 | Microsoft Docs
description: Se describen los problemas críticos por resolver y las soluciones para Azure StorSimple Virtual Array que ejecuta Update 1.3.
ms.service: storsimple
author: v-dalc
ms.topic: article
ms.date: 01/22/2021
ms.author: alkohli
ms.openlocfilehash: 02611bdf9689d2f62f661f558fd547ea46bd4d36
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744131"
---
# <a name="storsimple-virtual-array-update-13-release-notes"></a>Notas de la versión de StorSimple Virtual Array Update 1.3

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de las actualizaciones de la matriz virtual de Microsoft Azure StorSimple.

Las notas de la versión se actualizan continuamente. A medida que se descubren problemas críticos que requieren una solución alternativa, se van agregando. Antes de implementar la matriz virtual de StorSimple, le recomendamos que lea detenidamente la información que encontrará en las notas de la versión.

Update 1.3 se corresponde con la versión 10.0.10319.0 del software.

> [!IMPORTANT]
> - Update 1.3 es una actualización crítica. Se recomienda encarecidamente instalarla lo antes posible.
> - Solo puede instalar Update 1.3 en dispositivos que ejecuten la Update 1.2.
> - Las actualizaciones causan interrupciones y reinician el dispositivo. Si hay procesos de E/S en curso, el dispositivo incurrirá en tiempo de inactividad. Para instrucciones detalladas con los paquetes que se usan para aplicar esta actualización, vaya a [Descarga de Update 1.3](#download-update-13).

## <a name="whats-new-in-update-13"></a>Novedades de Update 1.3

Esta actualización contiene las siguientes mejoras:

- Seguridad de la capa de transporte (TLS) 1.2 es una actualización obligatoria y debe instalarse inmediatamente. A partir de esta versión, TLS 1.2 se convierte en el protocolo estándar para todas las comunicaciones de Azure Portal.
- Las correcciones de errores de la recolección de elementos no utilizados mejoran el rendimiento del ciclo de recolección de elementos no utilizados cuando la cuenta de almacenamiento y el dispositivo se encuentran en dos regiones lejanas.
- Corrección de errores de copia de seguridad debido a tiempos de espera de blobs.
- Parches de seguridad del sistema operativo/.NET Framework actualizados:
  - [KB4540725](\\winsehotfix.segroup.winse.corp.microsoft.com\hotfixes\Windows6.3\RTM\KB4540725\V1.001\free\NEU\X64): Actualización de la pila de servicio (marzo de 2020)
  - [KB4565541](\\winsehotfix.segroup.winse.corp.microsoft.com\hotfixes\Windows6.3\RTM\KB4565541\V1.014\free\NEU\X64): Paquete acumulativo de actualizaciones de julio de 2020
  - [KB4565622](\\winsehotfix.segroup.winse.corp.microsoft.com\hotfixes\Partner\DOTNET47x\KB4565622\V1.000\free\NEU\x64): Actualización de .NET Framework de julio de 2020

## <a name="download-update-13"></a>Descarga de Update 1.3

Para descargar esta actualización, vaya al servidor de [Catálogo de Microsoft Update](https://www.catalog.update.microsoft.com/Home.aspx) y descargue el paquete KB4575898. Este paquete contiene los siguientes paquetes:

- **KB4540725**, que contiene las actualizaciones acumulativas de Windows desde 2012 R2 hasta marzo de 2020. Para más información sobre qué se incluye en este paquete acumulativo, vaya a [Paquete acumulativo mensual (marzo)](https://support.microsoft.com/help/4540725).
- **KB4565541**, que contiene las actualizaciones acumulativas de Windows desde 2012 R2 hasta julio de 2020. Para más información sobre qué se incluye en este paquete acumulativo, vaya a [Paquete acumulativo mensual (febrero)](https://support.microsoft.com/help/4565541).
- **KB4565622**, que contiene las actualizaciones acumulativas de .NET Framework hasta julio de 2020. Para más información sobre qué se incluye en este paquete acumulativo, vaya a [Paquete acumulativo mensual (febrero)](https://support.microsoft.com/help/4565622).
- **KB3011067**, que contiene las actualizaciones de software del dispositivo.

Descargue KB4575898 y siga estas instrucciones para [aplicar la actualización a través de la interfaz de usuario local](./storsimple-virtual-array-install-update-11.md#use-the-local-web-ui).

## <a name="known-issues-in-update-13"></a>Problemas conocidos de Update 1.3
No se notificaron nuevos problemas de la versión en Update 1.3. Todos los problemas notificados de la versión provienen de las versiones anteriores. Para ver el resumen de los problemas conocidos incluidos desde las versiones anteriores, vaya a [Problemas conocidos de Update 1.2](./storsimple-virtual-array-update-12-release-notes.md#known-issues-in-update-12).

## <a name="next-steps"></a>Pasos siguientes
Descargar KB4575898 y [aplicar la actualización a través de la interfaz de usuario local](./storsimple-virtual-array-install-update-1.md#use-the-local-web-ui).

## <a name="references"></a>Referencias
¿Busca una nota de la versión anterior? Vaya a:

- [Notas de la versión de StorSimple Virtual Array Update 1.2](./storsimple-virtual-array-update-12-release-notes.md)
- [Notas de la versión de StorSimple Virtual Array Update 1.0](./storsimple-virtual-array-update-1-release-notes.md)
- [Notas de la versión de StorSimple Virtual Array Update 0.6](./storsimple-virtual-array-update-06-release-notes.md)
- [Notas de la versión de StorSimple Virtual Array Update 0.5](./storsimple-virtual-array-update-05-release-notes.md)
- [Notas de la versión de StorSimple Virtual Array Update 0.4](./storsimple-virtual-array-update-04-release-notes.md)
- [Notas de la versión de StorSimple Virtual Array Update 0.3](./storsimple-ova-update-03-release-notes.md)
- [Notas de la versión de la matriz virtual de StorSimple Update 0.1 y 0.2](./storsimple-ova-update-01-release-notes.md)
- [Notas de la versión de disponibilidad general de la matriz Virtual de StorSimple](https://review.docs.microsoft.com/en-us/azure/storsimple/storsimple-ova-pp-release-notes)