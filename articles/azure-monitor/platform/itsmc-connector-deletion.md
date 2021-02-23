---
title: Eliminación del Conector ITSM y la acción asociada con él
description: En este artículo se explica cómo eliminar el Conector ITSM y los grupos de acciones asociadas con él.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 1fbaa48d104642984e604bc66a3aa914fbfed44c
ms.sourcegitcommit: 7ec45b7325e36debadb960bae4cf33164176bc24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100530899"
---
# <a name="deletion-of-unused-itsm-connectors"></a>Eliminación de conectores ITSM sin utilizar

El proceso de eliminación de un conector sin utilizar contiene dos fases:

1. Eliminación de las acciones asociadas: deben eliminarse todas las acciones asociadas al Conector ITSM. Esto se debe hacer para que no haya acciones sin conector que puedan causar errores en la suscripción.

2. Eliminación del Conector ITSM sin usar.

## <a name="deletion-of-the-associated-actions"></a>Eliminación de las acciones asociadas

1. Para encontrar el grupo de acciones, debe ir a "Monitor". ![Captura de pantalla de la selección de Monitor](media/itsmc-connector-deletion/itsmc-monitor-selection.png)

2. Seleccione "Alertas". ![Captura de pantalla de la selección de Alertas](media/itsmc-connector-deletion/itsmc-alert-selection.png)
3. Seleccione "Administrar acciones". ![Captura de pantalla de la selección de Administrar acciones](media/itsmc-connector-deletion/itsmc-actions-selection.png)
4. Seleccione todos los conectores ITSM que están conectados a Cherwell. ![Captura de pantalla de los conectores ITSM que están conectados a Cherwell](media/itsmc-connector-deletion/itsmc-actions-screen.png)
5. Elimine el grupo de acciones. ![Captura de pantalla de la eliminación del grupo de acciones](media/itsmc-connector-deletion/itsmc-action-deletion.png)

## <a name="deletion-of-the-unused-itsm-connector"></a>Eliminación del Conector ITSM sin usar

1. Busque y seleccione "ServiceDesk" LA en la barra de búsqueda superior. ![Captura de pantalla de la búsqueda y selección de "ServiceDesk" LA](media/itsmc-connector-deletion/itsmc-connector-selection.png)
2. Seleccione "Conexiones de ITSM" y seleccione el conector de Cherwell. ![Captura de pantalla de los conectores ITSM de Cherwell](media/itsmc-connector-deletion/itsmc-cherwell-connector.png)
3. Seleccione "Eliminar". ![Captura de pantalla de la eliminación del conector ITSM](media/itsmc-connector-deletion/itsmc-connector-deletion.png)

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas del Conector de Administración de servicios de TI](./itsmc-resync-servicenow.md)
