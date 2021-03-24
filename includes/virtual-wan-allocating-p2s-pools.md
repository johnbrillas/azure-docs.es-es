---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: wellee
ms.service: virtual-wan
ms.topic: include
ms.date: 01/12/2021
ms.author: wellee
ms.custom: include file
ms.openlocfilehash: 67a996dbe1eb7090ea5f9ee9f0880698f4ba74a3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98919079"
---
En esta sección, se describen las instrucciones y los requisitos para asignar espacios de direcciones de cliente en los que la unidad de escalado de puerta de enlace de VPN de punto a sitio que se ha elegido para el centro de conectividad de Virtual WAN es mayor o igual que 40.

### <a name="background"></a>Información previa

Las puertas de enlace de VPN de punto a sitio en el centro de conectividad de Virtual WAN se implementan con varias instancias. Cada instancia de una puerta de enlace de VPN de punto a sitio admite hasta 10 000 conexiones de usuario de punto a sitio simultáneas. Como consecuencia, en el caso de las unidades de escalado superiores a 40, Virtual WAN necesita implementar capacidad adicional, lo cual requiere la asignación de un número mínimo de grupos de direcciones para diferentes unidades de escalado.

Por ejemplo, si se elige una unidad de escalado de 100, se implementarán 5 instancias para la puerta de enlace de VPN de punto a sitio en el centro virtual. Esta implementación admite hasta 50 000 conexiones simultáneas y **al menos** 5 grupos de direcciones diferentes.

**Unidades de escalado disponibles**

| Unidad de escalado | Número máximo de clientes compatibles | Número mínimo de grupos de direcciones |
|--- |--- |--- |
| 40 | 20000 | 2 |
| 60 | 30000 | 3 |
| 80 | 40000 | 4 |
| 100 | 50000 | 5 |
| 120 | 60000 | 6 |
| 140 | 70000 | 7 |
| 160 | 80000 | 8 |
| 180 | 90000 | 9 |
| 200 | 100000 | 10 |

### <a name="specifying-address-pools"></a>Especificación de grupos de direcciones

A continuación se muestran algunas directrices para elegir grupos de direcciones. Tenga en cuenta que las asignaciones de grupos de direcciones VPN de punto a sitio las efectúa automáticamente Virtual WAN.

1. Una instancia de puerta de enlace permite 10 000 conexiones simultáneas como máximo. Por lo tanto, cada grupo de direcciones debe contener al menos 10 000 direcciones IP RFC1918 únicas.
1. Se combinan automáticamente varios intervalos de grupos de direcciones y se asignan a una única instancia de puerta de enlace **única**. Este proceso se lleva a cabo con un enfoque Round Robin para cualquier instancia de puerta de enlace que tenga menos de 10 000 direcciones IP. Por ejemplo, un grupo con 5000 direcciones se puede combinar automáticamente mediante Virtual WAN con otro grupo que tenga 8000 direcciones y se asigne a una sola instancia de puerta de enlace.
1. Un único grupo de direcciones solo se asigna a una única instancia de puerta de enlace mediante Virtual WAN.
1. Los grupos de direcciones deben ser distintos. No pueden superponerse.

> [!NOTE] 
> Si un grupo de direcciones está asociado a una instancia de puerta de enlace que recibiendo mantenimiento, el grupo de direcciones no se puede reasignar a otra instancia.

### <a name="example"></a>Ejemplo 

En el ejemplo siguiente se describe un caso en la que 60 unidades de escalado admiten hasta 30 000 conexiones, pero los grupos de direcciones asignadas dan como resultado menos de 30 000 conexiones simultáneas.

El número total de conexiones simultáneas que se admiten en esta configuración es 28 192. La primera instancia de puerta de enlace admite 10 000 direcciones; la segunda, 8192 conexiones, y la tercera también admite 10 000 direcciones.

| Número de grupo de direcciones | Grupo de direcciones | Conexiones admitidas |
|--- |--- |--- |
| 1 | 10.12.0.0/15 | 10000 |
| 2 | 10.13.0.0/19 | 8192 |
| 3 | 10.14.0.0/15 | 10000|

**Recomendación n.º 1: Asegúrese de que el grupo de direcciones n.º 2 tenga al menos 10 000 direcciones IP distintas (por ejemplo, 10.13.0.0/15)** .

**Recomendación n.º 2: Agregue otro grupo de direcciones más (por ejemplo, el grupo de direcciones n.º 4 10.15.0.0/21, con 2048 direcciones). Los grupos de direcciones 2 y 4 se combinarán automáticamente y permitirán que la instancia de puerta de enlace admita 10 000 conexiones simultáneas.**
