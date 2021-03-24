---
title: Conozca los términos y condiciones de Azure BareMetal Infrastructure
description: Conozca los términos y condiciones de Azure BareMetal Infrastructure.
ms.topic: conceptual
ms.date: 1/4/2021
ms.openlocfilehash: b22a6cecb2647df3878cb8fd4ade93d9a7d963fd
ms.sourcegitcommit: 2c1b93301174fccea00798df08e08872f53f669c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2021
ms.locfileid: "104770893"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Conozca los términos y condiciones de BareMetal Infrastructure

En este artículo, trataremos algunos términos y condiciones importantes de BareMetal.

- **Revisión**: Hay una revisión de marca original conocida como Revisión 3 (Rev 3) y dos revisiones de marca diferentes para los sellos de instancias de BareMetal. Cada sello difiere en la arquitectura y la proximidad de los hosts de máquina virtual de Azure:
    - **Revisión 4** (Rev 4): un nuevo diseño que proporciona una mayor proximidad de los hosts de máquina virtual (VM) de Azure y reduce la latencia entre las VM de Azure y las unidades de instancia de BareMetal. 
    - **Revisión 4.2** (Rev 4.2): la infraestructura BareMetal Infrastructure cuyo nombre se ha cambiado más reciente que usa la arquitectura Rev 4 existente. Rev 4 proporciona mayor proximidad a los hosts de máquina virtual (VM) de Azure. Presenta mejoras significativas en la latencia de red entre las máquinas virtuales de Azure y las unidades de instancia de BareMetal implementadas en los sellos o filas de Rev 4. Puede tener acceso a sus instancias de BareMetal y administrarlas a través de Azure Portal.    

- **Demarcación**: Define el tamaño de la implementación interna de Microsoft de instancias de BareMetal. Para poder implementar unidades de instancias, se debe implementar una demarcación de instancias de BareMetal que conste de bastidores de proceso, red y almacenamiento en una ubicación del centro de datos. Este tipo de implementación se denomina demarcación de instancias de BareMetal o de la revisión 4.2.

- **Tenant**: un cliente implementado en la demarcación de instancias de BareMetal se aísla en un *inquilino.* Un inquilino se aísla en el nivel de redes, almacenamiento y proceso de otros inquilinos. Las unidades de almacenamiento y proceso asignadas a los distintos inquilinos no pueden verse ni comunicarse entre sí en el nivel de la demarcación de instancias de BareMetal. Un cliente puede elegir que las implementaciones se realicen en diferentes inquilinos. Aún así, no hay ninguna comunicación entre los inquilinos en el nivel de la demarcación de instancias de BareMetal.

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la [infraestructura de BareMetal](concepts-baremetal-infrastructure-overview.md) o sobre cómo [identificar las unidades de instancia de BareMetal e interactuar con ellas](connect-baremetal-infrastructure.md). 