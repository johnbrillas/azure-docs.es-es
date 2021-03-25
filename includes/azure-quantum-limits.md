---
title: archivo de inclusión
description: archivo de inclusión
author: danielstocker
ms.service: azure-quantum
ms.topic: include
ms.date: 01/08/2021
ms.author: dasto
ms.openlocfilehash: 2106a48a583f120f8b4dde4eb32a30f1a1b1d85b
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98947654"
---
### <a name="provider-limits--quota"></a>Límites y cuotas de proveedor

El servicio Azure Quantum admite proveedores de servicios tanto propios como de terceros. Los proveedores de terceros poseen sus límites y cuotas. Los usuarios pueden ver las ofertas y los límites de Azure Portal al configurar proveedores de terceros en la hoja de proveedor. 

A continuación puede encontrar los límites de cuota publicados para el proveedor de soluciones de optimización propio de Microsoft. 

#### <a name="learn--develop-sku"></a>SKU Aprender y desarrollar

| Resource | Límite |
| --- | --- |
| Trabajos simultáneos basados en CPU | Hasta cinco trabajos simultáneos |
| Trabajos simultáneos basados en FPGA | Hasta dos trabajos simultáneos |
| Horas de solucionador basadas en CPU | Veinte horas al mes  |
| Horas de solucionador basadas en FPGA | Una hora al mes  |

Si usa la SKU Aprender y desarrollar, **no puede**  solicitar un aumento de los límites de cuota. En su lugar, debe cambiar a la SKU Rendimiento a escala.

#### <a name="performance-at-scale-sku"></a>SKU Rendimiento a escala

| Resource | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Trabajos simultáneos basados en CPU | Hasta cien trabajos simultáneos | Igual que el límite predeterminado |
| Trabajos simultáneos basados en FPGA | Hasta diez trabajos simultáneos | Igual que el límite predeterminado |
| Horas de solucionador | 1000 horas al mes  | Hasta 50 000 horas al mes |

Si necesita solicitar un aumento del límite, póngase en contacto con el soporte técnico de Azure. 

Para más información, consulte la [página de precios de Azure Quantum](https://aka.ms/AQ/Pricing).
Para información sobre las ofertas de terceros, consulte la página del proveedor pertinente en Azure Portal.
