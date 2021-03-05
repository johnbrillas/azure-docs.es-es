---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 01/04/2021
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 5658b68081fae8dab528030f7fc1cd6fe4935730
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "102178391"
---
- La expansión a petición no se puede habilitar en un disco SSD prémium que tenga menos de 512 GiB. Los SSD prémium inferiores a 512 GiB siempre usarán la expansión basada en crédito.
- La expansión a petición solo se admite en SSD prémium. Si un SSD prémium con la expansión a petición habilitada se cambia a otro tipo de disco, se deshabilitará la expansión de disco.
- La expansión a petición no se deshabilita automáticamente cuando se cambia el nivel de rendimiento. Si desea cambiar el nivel de rendimiento pero no desea mantener la expansión de disco, debe deshabilitarla.
- La expansión a petición solo se puede habilitar cuando se separa el disco de una máquina virtual o cuando se detiene la máquina virtual. La expansión a petición se puede deshabilitar 12 horas después de que se haya habilitado.