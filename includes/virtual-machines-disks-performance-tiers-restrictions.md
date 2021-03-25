---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2021
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 7151c110fd50f7485aa0b130832aace4f3143ad9
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101751138"
---
- Esta característica es compatible actualmente solo con SSD Premium.
- Debe desasignar la máquina virtual o desconectar el disco de una máquina virtual en ejecución antes de que pueda cambiar el nivel del disco.
- Los niveles de rendimiento P60, P70 y P80 solo pueden usarlos aquellos discos cuyo tamaño supere los 4096 GiB.
- El nivel de rendimiento de un disco solo se puede cambiar a un nivel inferior una vez cada 12 horas.

## <a name="change-performance-tier-without-downtime-preview"></a>Cambio del nivel de rendimiento sin que se produzca tiempo de inactividad (versión preliminar)

Normalmente, habría que desasignar la máquina virtual o desconectar el disco para cambiar el nivel de rendimiento. Pero si habilita esta característica de la versión preliminar, no hay que desasignar la máquina virtual ni desconectar el disco para cambiar el nivel. Puede registrarse para la versión preliminar [en esta página](https://aka.ms/liveperftiersignup).

La versión preliminar tiene las limitaciones siguientes:
- Solo está disponible en la región EastUS2EUAP.
- Actualmente no está disponible para discos compartidos.
- Se deben usar plantillas de Azure Resource Manager con la API `2020-12-01` para cambiar los niveles de rendimiento sin que se produzca tiempo de inactividad.