---
title: Cómo corregir manualmente los problemas de sincronización de ServiceNow
description: Restablecimiento de la conexión a ServiceNow para que las alertas en Microsoft Azure puedan volver a llamar a ServiceNow
ms.subservice: alerts
ms.topic: conceptual
author: nolavime
ms.author: nolavime
ms.date: 01/17/2021
ms.openlocfilehash: 1b2e914057aa72934524cc4977e3d2292394d0b7
ms.sourcegitcommit: aaa65bd769eb2e234e42cfb07d7d459a2cc273ab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98882076"
---
# <a name="how-to-manually-fix-sync-problems"></a>Cómo corregir manualmente los problemas de sincronización

Azure Monitor puede conectarse a otros proveedores de Administración de servicios de TI (ITSM) de terceros. ServiceNow es uno de estos proveedores.

Por motivos de seguridad, puede que tenga que actualizar el token de autenticación que se usa para la conexión con ServiceNow.
Use el proceso de sincronización siguiente para volver a activar la conexión y actualizar el token:

1. Busque la solución en el banner de búsqueda superior y, luego, seleccione las soluciones apropiadas.

    ![Captura de pantalla que muestra el banner de búsqueda principal y dónde seleccionar las soluciones pertinentes.](media/itsmc-resync-servicenow/solution-search-8-bit.png)

1. En la pantalla de la solución, elija "Seleccionar todo" en el filtro de suscripción y, a continuación, filtre por "ServiceDesk".

    ![Captura de pantalla que muestra dónde elegir "Seleccionar todo" y dónde filtrar según ServiceDesk.](media/itsmc-resync-servicenow/solutions-list-8-bit.png)

1. Seleccione la solución de la conexión de ITSM.
1. Seleccione la conexión de ITSM en el banner izquierdo.

    ![Captura de pantalla que muestra dónde seleccionar las conexiones de ITSM.](media/itsmc-resync-servicenow/itsm-connector-8-bit.png)

1. Seleccione cada conector en la lista. 
    1. Haga clic en el nombre del conector para configurarlo.
    1. Elimine los conectores que ya no se estén usando.

    1. Actualice los campos de acuerdo con [estas definiciones](./itsmc-connections.md) según el tipo de asociado.

    1. Haga clic en Sincronizar

       ![Captura de pantalla que resalta el botón Sincronizar.](media/itsmc-resync-servicenow/resync-8-bit-2.png)

    1. Haga clic en Guardar

        ![Nueva conexión](media/itsmc-resync-servicenow/save-8-bit.png)

f.    Revise las notificaciones para ver si se inició el proceso.
