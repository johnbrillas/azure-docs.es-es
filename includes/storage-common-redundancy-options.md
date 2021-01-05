---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/14/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 65729934ea7c4037d6857aec10b14cdddd616368
ms.sourcegitcommit: 7e97ae405c1c6c8ac63850e1b88cf9c9c82372da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97805636"
---
Entre las opciones de redundancia para una cuenta de almacenamiento se incluyen las siguientes:

* Almacenamiento con redundancia local (LRS): una estrategia de redundancia simple y de bajo costo. Los datos se copian de forma sincrónica tres veces dentro de una sola ubicación física en la región primaria.
* Almacenamiento con redundancia de zona (ZRS): redundancia para escenarios que requieren alta disponibilidad. Los datos se copian de forma sincrónica en tres zonas de disponibilidad de Azure en la región primaria.
* Almacenamiento con redundancia geográfica (GRS): redundancia entre regiones para protegerse frente a las interrupciones regionales. Los datos se copian sincrónicamente tres veces en la región primaria y, luego, se replican de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS).
* Almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar): redundancia para escenarios que requieren alta disponibilidad y máxima durabilidad. Los datos se copian sincrónicamente en tres zonas de disponibilidad de Azure en la región primaria y, luego, se copian de forma asincrónica en la región secundaria. Para obtener acceso de lectura a los datos de la región secundaria, habilite el almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS).

Para más información sobre las opciones de redundancia en Azure Storage, consulte [Redundancia de Azure Storage](../articles/storage/common/storage-redundancy.md).
