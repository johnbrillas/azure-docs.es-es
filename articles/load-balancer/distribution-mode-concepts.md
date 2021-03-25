---
title: Modos de distribución de Azure Load Balancer
description: Comience a aprender los distintos modos de distribución de Azure Load Balancer.
author: asudbring
ms.author: allensu
ms.service: load-balancer
ms.topic: article
ms.date: 02/04/2021
ms.custom: template-concept
ms.openlocfilehash: fcea2e962722773f59e73df898e0188c3f6454b6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99551148"
---
# <a name="azure-load-balancer-distribution-modes"></a>Modos de distribución de Azure Load Balancer

Azure Load Balancer admite dos modos de distribución para enrutar las conexiones a la aplicación de carga equilibrada:

* Por hash
* Modo de afinidad de IP de origen

## <a name="hash-based"></a>Por hash

El modo de distribución predeterminado para Azure Load Balancer es un hash de tupla de cinco elementos. 

La tupla consta de:
* **IP de origen**
* **Puerto de origen**
* **IP de destino**
* **Puerto de destino**
* **Tipo de protocolo**

El hash se utiliza para asignar el tráfico a los servidores disponibles. El algoritmo solo proporciona adherencia dentro de una sesión de transporte. Los paquetes que se encuentran en la misma sesión se dirigen a la misma dirección IP del centro de datos tras el punto de conexión con equilibrio de carga. Cuando el cliente inicia una nueva sesión desde la misma IP de origen, el puerto de origen cambia y provoca que el tráfico vaya hacia otro punto de conexión del centro de datos.

![Modo de distribución basada en un hash de tupla de cinco elementos](./media/distribution-mode-concepts/load-balancer-distribution.png)

El modo basado en hash tiene un tipo de configuración:

* **Ninguno (basado en hash)**: especifica que cualquier máquina virtual puede controlar las solicitudes sucesivas del mismo cliente.

## <a name="source-ip-affinity"></a>Modo de afinidad de IP de origen

Este modo de distribución también se conoce como afinidad de la sesión o afinidad de IP del cliente. El modo utiliza un hash de tupla de dos elementos (IP de origen e IP de destino) o de tres elementos (IP de origen, IP de destino y tipo de protocolo) para asignar el tráfico a los servidores disponibles. 

Mediante el uso de la afinidad de la IP de origen, las conexiones que se han iniciado desde el mismo equipo cliente van al mismo punto de conexión del centro de datos.

En la figura siguiente, se ilustra la configuración de tupla de dos elementos. Observe cómo la tupla de dos elementos se ejecuta a través del equilibrador de carga en la máquina virtual 1 (VM1). A continuación, VM2 y VM3 realizan una copia de seguridad de VM1.

![Modo de distribución de afinidad de la sesión de una tupla de dos elementos](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

El modo de afinidad de IP de origen tiene dos tipos de configuración:

* **IP de cliente (dos tuplas de afinidad de IP de origen)**: especifica que la misma máquina virtual controlará las solicitudes sucesivas de la misma dirección IP de cliente.
* **IP de cliente y protocolo (tres tuplas de afinidad de IP de origen)**: especifica que la misma máquina virtual controlará las solicitudes sucesivas de la combinación de la misma dirección IP de cliente y protocolo.

## <a name="use-cases"></a>Casos de uso

La afinidad de IP de origen con la IP y el protocolo de cliente (afinidad de IP de origen de 3 tuplas) resuelve una incompatibilidad entre Azure Load Balancer y Puerta de enlace de Escritorio remoto. 

Otro escenario de caso de uso es la carga de elementos multimedia. La carga de datos tiene lugar a través de UDP, pero el plano de control se consigue mediante TCP:

* Un cliente inicia una sesión TCP en la dirección pública de carga equilibrada y se le dirige a una DIP específica. El canal se mantiene activo para supervisar el estado de conexión.
* Se inicia una nueva sesión UDP desde el mismo equipo cliente al mismo punto de conexión público de carga equilibrada. La conexión se dirige al mismo punto de conexión DIP que la conexión TCP anterior. La carga de elementos multimedia se puede ejecutar a alto rendimiento al tiempo que mantiene un canal de control a través de TCP.

> [!NOTE]
> Cuando un conjunto de carga equilibrada cambia al quitar o agregar una máquina virtual, la distribución de las solicitudes de cliente se vuelve a calcular. No puede depender de nuevas conexiones desde clientes existentes para terminar en el mismo servidor. Además, el uso del modo de distribución de afinidad de IP de origen puede ocasionar una distribución desigual del tráfico. Los clientes que se ejecutan detrás de servidores proxy pueden considerarse como una sola aplicación cliente.


## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo configurar el modo de distribución de Azure Load Balancer, consulte [Configuración del modo de distribución de Azure Load Balancer](load-balancer-distribution-mode.md).

