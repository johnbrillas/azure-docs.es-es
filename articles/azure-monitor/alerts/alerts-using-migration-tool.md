---
title: Migración de las reglas de alertas de Azure Monitor
description: Obtenga información sobre cómo usar la herramienta de migración voluntaria para migrar las reglas de alertas clásicas.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: fa487bec49ab9faa0f7c3dce752a30e4440fb873
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102037683"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Uso de la herramienta de migración voluntaria para migrar las reglas de alertas clásicas

Como se [anunció anteriormente](monitoring-classic-retirement.md), se retiran las alertas clásicas de Azure Monitor para los usuarios de la nube pública, aunque se seguirán usando de forma limitada hasta el **31 de mayo de 2021**. Las alertas clásicas para la nube de Azure Government y Azure China 21Vianet se retirarán el **29 de febrero de 2024**.

En Azure Portal, hay una herramienta de migración para los clientes que usan reglas de alertas clásicas y que quieren desencadenar la migración ellos mismos. En este artículo se explica cómo usar la herramienta de migración.

## <a name="before-you-migrate"></a>Antes de la migración

El proceso de migración convierte las reglas de alertas clásicas a las reglas de alertas nuevas equivalentes y crea grupos de acciones. En la preparación, tenga en cuenta los siguientes aspectos:

- El formato de carga de notificación y las API para crear y administrar nuevas reglas de alertas son diferentes de las reglas de alerta clásicas porque admiten más características. [Obtenga información sobre cómo preparar la migración](alerts-prepare-migration.md).

- No se pueden migrar algunas reglas de alertas clásicas mediante la herramienta. [Obtenga información sobre las reglas que no se pueden migrar y qué hacer con ellas](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

    > [!NOTE]
    > El proceso de migración no afecta a la evaluación de las reglas de alertas clásicas. Seguirán ejecutándose y enviando alertas hasta que se hayan migrado y las nuevas reglas de alertas surtan efecto.

## <a name="how-to-use-the-migration-tool"></a>Cómo usar la herramienta de migración

Para desencadenar la migración de las reglas de alertas clásicas en Azure Portal, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), seleccione **Monitor**.

1. Seleccione **Alertas** y, a continuación, seleccione **Administrar reglas de alertas** o **Ver alertas clásicas**.

1. Seleccione **Migrar a las nuevas reglas** para ir a la página de inicio de la migración. Esta página muestra una lista de todas las suscripciones y su estado de migración:

    ![Captura de pantalla que muestra la página Migrar reglas de alertas.](media/alerts-using-migration-tool/migration-landing.png "Migración de reglas")

    Todas las suscripciones que se pueden migrar mediante la herramienta están marcadas como **Listo para migrar**.

    > [!NOTE]
    > La herramienta de migración se está implementando por fases en todas las suscripciones que usan reglas de alertas clásicas. En las primeras fases del lanzamiento, es posible que vea algunas suscripciones marcadas como no listas para la migración.

1. Seleccione una o varias suscripciones y, a continuación, seleccione **Obtener una vista previa de la migración**.

    La página resultante muestra los detalles de las reglas de alertas clásicas que se migrarán para una suscripción cada vez. También puede seleccionar **Descargar los detalles de la migración de esta suscripción** para obtener los detalles en un formato CSV.

    ![Captura de pantalla que muestra la página Migrar reglas de alertas con un vínculo para descargar los detalles de la migración de esta suscripción y donde puede especificar el correo electrónico para la notificación de la migración.](media/alerts-using-migration-tool/migration-preview.png "Vista previa de la migración")

1. Especifique una o varias direcciones de correo electrónico para recibir una notificación del estado de la migración. Recibirá un correo electrónico una vez completada la migración o si es necesaria alguna acción por parte del usuario.

1. Seleccione **Iniciar migración**. Lea la información que se muestra en el cuadro de diálogo de confirmación y confirme que está listo para iniciar el proceso de migración.

    > [!IMPORTANT]
    > Después de iniciar la migración de una suscripción, no podrá editar o crear reglas de alertas clásicas para esa suscripción. Esta restricción garantiza que no se pierde ningún cambio en las reglas de alertas clásicas durante la migración a las nuevas reglas. Aunque no podrá cambiar las reglas de alertas clásicas, estas aún seguirán en ejecución y proporcionarán alertas hasta que se hayan migrado. Una vez completada la migración de su suscripción, ya no podrá usar más las reglas de alertas clásicas.

    ![Captura de pantalla muestra un aviso de confirmación para la migración, incluida información importante con vínculos para obtener más información antes de continuar.](media/alerts-using-migration-tool/migration-confirm.png "Configuración del inicio de la migración")

1. Recibirá un correo electrónico en las direcciones que proporcionó anteriormente cuando se complete la migración o si es necesaria alguna acción por su parte. También puede comprobar periódicamente el estado en la página de inicio de la migración en el portal.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>¿Por qué mi suscripción aparece como no lista para la migración?

La herramienta de migración se está implementando en los clientes por fases. En las primeras fases, la mayoría o todas las suscripciones pueden estar marcadas como **No está lista para la migración**. 

Cuando una suscripción pasa a estar lista para la migración, el propietario de la suscripción recibirá un mensaje de correo electrónico que indica que la herramienta está disponible. Esté atento de este mensaje.

### <a name="who-can-trigger-the-migration"></a>¿Quién puede desencadenar la migración?

Pueden desencadenar la migración los usuarios que tengan el rol de colaborador de supervisión asignado en el nivel de suscripción. [Más información sobre el control de acceso basado en rol de Azure para el proceso de migración](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>¿Cuánto tiempo tardará la migración?

La migración se completa en menos de una hora para la mayoría de las suscripciones. Puede realizar un seguimiento del estado de la migración en la página de inicio de la migración. Durante la migración, se garantiza que las alertas todavía se ejecutan en el sistema de alertas clásicas o en el nuevo.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>¿Qué puedo hacer si surge algún problema durante la migración?

Consulte la [guía de solución de problemas](alerts-understand-migration.md#common-problems-and-remedies) para obtener ayuda con los problemas que se pueden producir durante la migración. Si es necesaria alguna acción por parte del usuario para completar la migración, se le notificará en las direcciones de correo electrónico que proporcionó al configurar la herramienta.

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para la migración](alerts-prepare-migration.md)
- [Descripción del funcionamiento de la herramienta de migración](alerts-understand-migration.md)