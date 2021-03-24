---
title: Recuperación de información del equilibrador de carga mediante Azure Instance Metadata Service
titleSuffix: Azure Load Balancer
description: Comience a aprender a usar Azure Instance Metadata Service para recuperar información del equilibrador de carga.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 0d7d08eb5e77e744fb6ce0abefc550bc79de4c8c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100519089"
---
# <a name="retrieve-load-balancer-information-by-using-the-azure-instance-metadata-service"></a>Recuperación de información del equilibrador de carga mediante Azure Instance Metadata Service

IMDS (Azure Instance Metadata Service) proporciona información sobre las instancias de máquina virtual que se ejecutan actualmente. El servicio es una API REST que está disponible en una dirección IP conocida y no enrutable (169.254.169.254). 

Al colocar instancias de máquina virtual o de conjunto de máquinas virtuales detrás de Azure Standard Load Balancer, use IMDS para recuperar los metadatos relacionados con el equilibrador de carga y las instancias.

Los metadatos incluyen la siguiente información de las máquinas virtuales o de los conjuntos de escalado de máquinas virtuales:

* Dirección IP públicas de SKU estándar.
* Configuraciones de reglas de entrada del equilibrador de carga de cada IP privada de la interfaz de red.
* Configuraciones de reglas de salida del equilibrador de carga de cada IP privada de la interfaz de red.

## <a name="access-the-load-balancer-metadata-using-the-imds"></a>Acceso a los metadatos del equilibrador de carga mediante IMDS

Para más información sobre cómo acceder a los metadatos del equilibrador de carga, consulte [Uso de Azure Instance Metadata Service para acceder a la información del equilibrador de carga](howto-load-balancer-imds.md).

## <a name="troubleshoot-common-error-codes"></a>Solución de códigos de error comunes

Para más información sobre los códigos de error comunes y sus métodos de mitigación, consulte [Solución de problemas de códigos de error comunes al usar IMDS](troubleshoot-load-balancer-imds.md). 

## <a name="support"></a>Soporte técnico

Si después de varios intentos no puede recuperar una respuesta de metadatos, puede crear una incidencia de soporte técnico en Azure Portal.

## <a name="next-steps"></a>Pasos siguientes
Más información sobre [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md).

[Implementación de un equilibrador de carga estándar](quickstart-load-balancer-standard-public-portal.md)

