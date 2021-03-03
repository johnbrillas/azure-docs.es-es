---
title: Plantillas del Administrador de recursos de Azure
description: Plantillas de Azure Resource Manager para usar las características de Azure Site Recovery.
services: site-recovery
author: rishjai
manager: gaggupta
ms.service: site-recovery
ms.topic: article
ms.date: 02/04/2021
ms.author: rishjai
ms.openlocfilehash: efd5c1ab3e23348373be76c79d1b9689bc5f4941
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101720535"
---
# <a name="azure-resource-manager-templates-for-azure-site-recovery"></a>Plantillas de Azure Resource Manager para Azure Site Recovery

En la tabla siguiente se incluyen vínculos a plantillas de Azure Resource Manager para usar características de Azure Site Recovery.

| Plantilla | Descripción |
|---|---|
|**De Azure a Azure** | |
| [Creación de un almacén de Recovery Services](./quickstart-create-vault-template.md)| Cree un almacén de Recovery Services. El almacén se puede usar con Azure Backup y Azure Site Recovery. |
| [Habilitación de la replicación para máquinas virtuales de Azure](https://aka.ms/asr-arm-enable-replication) | Habilite la replicación en máquinas virtuales de Azure con el almacén existente y la configuración de destino personalizada.|
| [Desencadenamiento de la conmutación por error y la reprotección](https://aka.ms/asr-arm-failover-reprotect) | Desencadene una operación de conmutación por error y reprotección en un conjunto de máquinas virtuales de Azure. |
| [Ejecución de un flujo de recuperación ante desastres de un extremo a otro en máquinas virtuales de Azure](https://aka.ms/asr-arm-e2e-flow) | Inicie un flujo completo de recuperación ante desastres de un extremo a otro (habilite la replicación + la conmutación por error y la reprotección + la conmutación por recuperación y la reprotección) en máquinas virtuales de Azure, lo que también se conoce como flujo de 540 °.|
|   |   |