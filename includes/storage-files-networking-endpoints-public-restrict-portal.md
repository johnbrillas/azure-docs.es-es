---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 01/25/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 3c76988557bfa338bcfb606f568036422a536c1d
ms.sourcegitcommit: 95c2cbdd2582fa81d0bfe55edd32778ed31e0fe8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98798582"
---
Vaya a la cuenta de almacenamiento para la que desea restringir el punto de conexión público a redes virtuales específicas. En la tabla de contenido de la cuenta de almacenamiento, seleccione la entrada **Redes**. 

En la parte superior de la página, seleccione el botón de radio **Redes seleccionadas**. Esta acción anulará la ocultación de una serie de opciones para controlar la restricción del punto de conexión público. Haga clic en **+ Add existing virtual network** (+ Agregar red virtual existente) para seleccionar la red virtual específica a la que se debe permitir el acceso a la cuenta de almacenamiento a través del punto de conexión público. Para ello, es necesario seleccionar una red virtual y una subred para esa red virtual. 

Active **Allow trusted Microsoft services to access this service account** (Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de servicio) para permitir que servicios de confianza de terceros, como Azure File Sync, accedan a la cuenta de almacenamiento.

[![Captura de pantalla de la hoja Redes con una red virtual específica a la que se permite el acceso a la cuenta de almacenamiento mediante el punto de conexión público](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png)](media/storage-files-networking-endpoints-public-restrict-portal/restrict-public-endpoint-0.png#lightbox)