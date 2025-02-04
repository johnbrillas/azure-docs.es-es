---
title: Operaciones de Synchronization Service Manager de Azure AD Connect | Microsoft Docs
description: Conozca la pestaña Operaciones de Synchronization Service Manager para Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 97a26565-618f-4313-8711-5925eeb47cdc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 64ab5901ad54e37fc05d7da757ce7f31d42fe3b6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "85357110"
---
# <a name="using-the-sync-service-manager-operations-tab"></a>Uso de la pestaña Operaciones de Synchronization Service Manager

![Sync Service Manager](./media/how-to-connect-sync-service-manager-ui-operations/operations.png)

En la pestaña Operaciones se muestran los resultados de las operaciones más recientes. Esta pestaña es clave para entender y solucionar los problemas.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Información visible en la pestaña Operaciones
La mitad superior muestra todas las ejecuciones en orden cronológico. De forma predeterminada, el registro de operaciones conservará información sobre los últimos 7 días, pero puede cambiar este parámetro con el [programador](how-to-connect-sync-feature-scheduler.md). Debe buscar las ejecuciones que no muestran un estado de operación correcta. Para cambiar la ordenación, haga clic en los encabezados.

En la columna **Estado** se encuentra la información más importante, puesto que muestra el problema más grave de una ejecución. A continuación tiene un resumen rápido de los estados más comunes que debe analizar por orden de prioridad (donde * indica varias cadenas de error posibles).

| Status | Comentario |
| --- | --- |
| stopped-\* |No se ha podido completar la ejecución. Por ejemplo, si el sistema remoto está inactivo y no se puede conectar a él. |
| stopped-error-limit |Se han generado más de 5000 errores. La ejecución se ha detenido automáticamente debido al elevado número de errores. |
| completed-\*-errors |Se completa la ejecución, pero hay errores (menos de 5000) que deben investigarse. |
| completed-\*-warnings |La ejecución se ha completado, pero algunos datos no tienen el estado esperado. Si se producen errores, es posible que se trate únicamente de un síntoma. Le recomendamos que primero resuelva los errores y que luego investigue las advertencias. |
| success |No hay ningún problema. |

Cuando seleccione una fila, la parte inferior se actualizará para mostrar los detalles de la ejecución. En el extremo izquierdo de la parte inferior, es posible que aparezca una lista con la información **Paso #** . Solo aparecerá si tiene varios dominios en el bosque; cada dominio estará representado por un paso. El nombre de dominio puede encontrarse bajo el encabezado **Partición**. En **Synchronization Statistics**(Estadísticas de sincronización) puede encontrar más información sobre el número de cambios que se han procesado. Puede hacer clic en los vínculos para obtener una lista de los objetos modificados. Si hay objetos con errores, estos se mostrarán en **Errores de sincronización**.

Para más información, consulte la [solución de problemas de un objeto que no se sincroniza](tshoot-connect-object-not-syncing.md).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre la configuración de la [Sincronización de Azure AD Connect](how-to-connect-sync-whatis.md) .

Obtenga más información sobre la [Integración de las identidades locales con Azure Active Directory](whatis-hybrid-identity.md).
