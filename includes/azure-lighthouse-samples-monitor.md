---
title: archivo de inclusión
description: archivo de inclusión
services: lighthouse
author: JnHs
ms.service: lighthouse
ms.topic: include
ms.date: 12/11/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 0056e18b6cb3aad2a4504bbe20b3b3421793489e
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2020
ms.locfileid: "97356284"
---
En estos ejemplos se muestra cómo usar Azure Monitor para crear alertas para las suscripciones que se han incorporado a la administración de recursos delegados de Azure.

| **Plantilla** | **Descripción** |
|---------|---------|
| [monitor-delegation-changes](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/monitor-delegation-changes) | Consulta el último día de actividad en un inquilino de administración y [notifica sobre cualquier delegación agregada o eliminada](../articles/lighthouse/how-to/monitor-delegation-changes.md) (o intentos de delegaciones que no se realizaron correctamente).|
| [alert-using-actiongroup](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/alert-using-actiongroup) | Esta plantilla crea una alerta de Azure y se conecta a un grupo de acciones existente.|
| [multiple-loganalytics-alerts](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/multiple-loganalytics-alerts) | Crea varias alertas de registro basadas en consultas de Kusto.|
| [delegation-alert-for-customer](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/delegation-alert-for-customer) | Implementa una alerta en un inquilino cuando un usuario delega una suscripción en un inquilino de administración.|
| [workbook-activitylogs-by-domain](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/workbook-activitylogs-by-domain) | Muestra los registros de actividad de Azure entre suscripciones con una opción para filtrarlos por nombre de dominio. |
