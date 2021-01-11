---
title: Conozca los términos y condiciones de Azure BareMetal Infrastructure
description: Conozca los términos y condiciones de Azure BareMetal Infrastructure.
ms.topic: conceptual
ms.date: 12/31/2020
ms.openlocfilehash: f11bc4cfdd463623010ed7b6677235344ec7cd62
ms.sourcegitcommit: 42922af070f7edf3639a79b1a60565d90bb801c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/31/2020
ms.locfileid: "97829061"
---
# <a name="know-the-terms-for-baremetal-infrastructure"></a>Conozca los términos y condiciones de BareMetal Infrastructure

En este artículo, trataremos algunos términos y condiciones importantes de BareMetal.

- **Revisión**: Hay dos revisiones de demarcación diferentes para las demarcaciones de instancias de BareMetal. Cada versión difiere en la arquitectura y la proximidad de los hosts de máquina virtual de Azure:
    - **Revisión 3** (Rev 3): es el diseño original.
    - **Revisión 4** (Rev 4): es un nuevo diseño que proporciona una mayor proximidad de los hosts de máquina virtual (VM) de Azure y reduce la latencia entre las VM de Azure y las unidades de instancia de BareMetal. 
    - **Revisión 4.2** (Rev 4.2): es la infraestructura BareMetal Infrastructure cuyo nombre se ha cambiado más reciente que usa la arquitectura Rev 4 existente. Puede tener acceso a sus instancias de BareMetal y administrarlas a través de Azure Portal.  

- **Demarcación**: Define el tamaño de la implementación interna de Microsoft de instancias de BareMetal. Para poder implementar unidades de instancias, se debe implementar una demarcación de instancias de BareMetal que conste de bastidores de proceso, red y almacenamiento en una ubicación del centro de datos. Este tipo de implementación se denomina demarcación de instancias de BareMetal o de la revisión 4.2.

- **Tenant**: un cliente implementado en la demarcación de instancias de BareMetal se aísla en un *inquilino.* Un inquilino se aísla en el nivel de redes, almacenamiento y proceso de otros inquilinos. Las unidades de almacenamiento y proceso asignadas a los distintos inquilinos no pueden verse ni comunicarse entre sí en el nivel de la demarcación de instancias de BareMetal. Un cliente puede elegir que las implementaciones se realicen en diferentes inquilinos. Aún así, no hay ninguna comunicación entre los inquilinos en el nivel de la demarcación de instancias de BareMetal.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre cómo identificar e interactuar con las unidades de instancia de BareMetal a través de [Azure Portal](workloads/sap/baremetal-infrastructure-portal.md).


 