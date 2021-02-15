---
title: Traslado de una máquina virtual de Azure Automanage entre regiones
description: Obtenga información sobre cómo trasladar una máquina virtual administrada automáticamente entre regiones
author: asinn826
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: how-to
ms.date: 02/05/2021
ms.author: alsin
ms.custom: subject-moving-resources
ms.openlocfilehash: 7e880992143bf79a5a99cc9830957f83167b6b46
ms.sourcegitcommit: 8245325f9170371e08bbc66da7a6c292bbbd94cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2021
ms.locfileid: "99808060"
---
# <a name="move-an-azure-automanage-virtual-machine-to-a-different-region"></a>Traslado de una máquina virtual de Azure Automanage a otra región
En este artículo se describe cómo mantener Automanage habilitado en una máquina virtual (VM) cuando se mueve a otra región. Puede mover sus máquinas virtuales a otra región por varios motivos. Por ejemplo, para aprovechar una nueva región de Azure, para cumplir los requisitos de gobernanza y las directivas internas o como respuesta a los requisitos de planeamiento de capacidad. Actualmente, es posible que las máquinas virtuales que mueve se administren automáticamente y que desee que permanezcan así tras el traslado.

## <a name="prerequisites"></a>Prerrequisitos
* Asegúrese de que la región de destino sea [compatible con Automanage](./automanage-virtual-machines.md#prerequisites).
* Asegúrese de que la región del área de trabajo de Log Analytics, la región de la cuenta de Automation y la región de destino son todas regiones compatibles con las asignaciones de regiones [aquí](https://docs.microsoft.com/azure/automation/how-to/region-mappings).

## <a name="prepare-your-automanaged-vms-for-moving"></a>Preparación de las máquinas virtuales administradas automáticamente para su traslado
Deshabilite Automanage en las máquinas virtuales administradas automáticamente. Puede hacerlo seleccionando las máquinas virtuales en la hoja Automanage y haciendo clic en **Deshabilitar administración automática** en la hoja Automanage.

## <a name="move-your-automanaged-vms-and-re-enable-automanage"></a>Traslado de las máquinas administradas automáticamente y rehabilitación de Automanage
Para obtener detalles sobre cómo mover las máquinas virtuales, consulte este [artículo](https://docs.microsoft.com/azure/resource-mover/tutorial-move-region-virtual-machines).

Una vez que haya migrado las máquinas virtuales entre regiones, puede rehabilitar Automanage en ellas una vez más. [Aquí](./automanage-virtual-machines.md#enabling-automanage-for-vms-in-azure-portal) encontrará detalles al respecto.

## <a name="next-steps"></a>Pasos siguientes
* [Más información sobre Azure Automanage](./automanage-virtual-machines.md)
* [Consulta de las preguntas más frecuentes sobre Azure Automanage](./faq.md)