---
title: Descripción de cómo funciona el proceso de migración automática de alertas clásicas de Azure Monitor
description: Sepa cómo funciona el proceso de migración automática.
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: 92aaffcea7a7c96cd77aade318520b093eed3e14
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045469"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Descripción del proceso de migración automática de reglas de alertas clásicas

Como se [anunció anteriormente](monitoring-classic-retirement.md), se retiran las alertas clásicas de Azure Monitor para los usuarios de la nube pública, aunque se seguirán usando de forma limitada hasta el **31 de mayo de 2021**. Las alertas clásicas para la nube de Azure Government y Azure China 21Vianet se retirarán el **29 de febrero de 2024**.

Hay disponible una [herramienta de migración](alerts-using-migration-tool.md) en Azure Portal para que los clientes puedan desencadenar la migración por sí mismos. En este artículo se explica el proceso de migración automática en la nube pública, que se iniciará después del 31 de mayo de 2021. También se detallan los problemas y las soluciones que podría encontrar.

## <a name="important-things-to-note"></a>Aspectos importantes que tener en cuenta

El proceso de migración convierte las reglas de alertas clásicas a las reglas de alertas nuevas equivalentes y crea grupos de acciones. En la preparación, tenga en cuenta los siguientes aspectos:

- Los formatos de carga de notificación de las nuevas reglas de alertas son diferentes de las cargas de las reglas de alertas clásicas porque admiten más características. Si tiene una regla de alertas clásica con aplicaciones lógicas, runbooks o webhooks, podrían dejar de funcionar según lo previsto después de la migración debido a las diferencias en las cargas. [Obtenga información sobre cómo preparar la migración](alerts-prepare-migration.md).

- Algunas reglas de alertas clásicas no se pueden migrar mediante la herramienta. [Obtenga información sobre las reglas que no se pueden migrar y qué hacer con ellas](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

## <a name="what-will-happen-during-the-automatic-migration-process-in-public-cloud"></a>¿Qué ocurrirá durante el proceso de migración automática en la nube pública?

- A partir del 31 de mayo de 2021, no podrá crear nuevas reglas de alertas clásicas y la migración de alertas clásicas se desencadenará en lotes.
- Las reglas de alertas clásicas que supervisen recursos de destino eliminados o que remitan a [métricas que ya no se admiten](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) se considerarán no válidas.
- Las reglas de alertas clásicas que no sean válidas se retirarán después del 31 de mayo de 2021.
- Una vez iniciada la migración para la suscripción, deberá completarse en una hora. Los clientes pueden supervisar el estado de la migración en la [herramienta de migración de Azure Monitor](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel).
- Los propietarios de la suscripción recibirán un correo electrónico cuando la migración se complete correcta o incorrectamente.

    > [!NOTE]
    > Si no desea esperar a que se inicie el proceso de migración automática, puede desencadenarla voluntariamente con la herramienta de migración.

## <a name="what-if-the-automatic-migration-fails"></a>¿Qué ocurre si se produce un error en la migración automática?

Cuando se produce un error en el proceso de migración automática, los propietarios de la suscripción recibirán un correo electrónico donde se les informará del problema. Se puede usar la herramienta de migración en Azure Monitor para ver todos los detalles del problema. Consulte la [guía de solución de problemas](alerts-understand-migration.md#common-problems-and-remedies) para obtener ayuda con los problemas que se pueden producir durante la migración.

  > [!NOTE]
  > En caso de que se requiera una acción por parte de los clientes, como deshabilitar temporalmente un bloqueo de recursos o cambiar una asignación de directiva, los clientes tendrán que resolver estos problemas. Si no se resuelven, no se puede garantizar la migración correcta de las alertas clásicas.

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para la migración](alerts-prepare-migration.md)
- [Descripción del funcionamiento de la herramienta de migración](alerts-understand-migration.md)