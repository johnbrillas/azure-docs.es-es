---
title: Notificación de expiración y actualización automática del entorno de ejecución de integración autohospedado
description: Obtenga información sobre la notificación de expiración y actualización automática del entorno de ejecución de integración autohospedado
ms.service: data-factory
ms.topic: conceptual
author: lrtoyou1223
ms.author: lle
ms.custom: seo-lt-2019
ms.date: 12/25/2020
ms.openlocfilehash: 972015f0f42a8a869de0edcc8f0e921ae7278cd1
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100376265"
---
# <a name="self-hosted-integration-runtime-auto-update-and-expire-notification"></a>Notificación de expiración y actualización automática del entorno de ejecución de integración autohospedado

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

En este artículo se describe cómo permitir la actualización automática del entorno de ejecución de integración autohospedado a la versión más reciente y cómo ADF administra las versiones del entorno de ejecución de integración autohospedado.

## <a name="self-hosted-integration-runtime-auto-update"></a>Actualización automática del entorno de ejecución de integración autohospedado
Por lo general, al instalar un entorno de ejecución de integración autohospedado en el equipo local o en una máquina virtual de Azure, tiene dos opciones para administrar la versión de dicho entorno de ejecución de integración autohospedado: actualización automática o mantenimiento manual. Normalmente, ADF lanza dos nuevas versiones del entorno de ejecución de integración autohospedado cada mes, que incluyen la nueva versión de actualización de características, la corrección de errores o mejoras. Por lo tanto, se recomienda a los usuarios que actualicen a la versión más reciente para obtener las características y mejoras más recientes.

La manera más cómoda de hacerlo consiste en habilitar la actualización automática al crear o editar el entorno de ejecución de integración autohospedado. A continuación, se actualizará automáticamente a la versión más reciente. También puede programar la actualización en la franja horaria que resulte más adecuada.

![Habilitación de la actualización automática](media/create-self-hosted-integration-runtime/shir-auto-update.png)

Puede comprobar el valor de datetime de la última actualización en el cliente del entorno de ejecución de integración autohospedado.

![Captura de pantalla de la comprobación de la hora de actualización](media/create-self-hosted-integration-runtime/shir-auto-update-2.png)

> [!NOTE]
> Para garantizar la estabilidad del entorno de ejecución de integración autohospedado, aunque lancemos dos versiones, solo se actualizará automáticamente una vez al mes. Por lo tanto, en ocasiones verá que la versión de la actualización automática es la anterior a la versión más reciente real. Si quiere obtener la versión más reciente, puede ir al [Centro de descarga](https://www.microsoft.com/download/details.aspx?id=39717).

## <a name="self-hosted-integration-runtime-expire-notification"></a>Notificación de expiración del entorno de ejecución de integración autohospedado
Si quiere controlar manualmente la versión del entorno de ejecución de integración autohospedado, puede deshabilitar la configuración de actualización automática e instalarla manualmente. Cada versión del entorno de ejecución de integración autohospedado expirará en un año. El mensaje de expiración se muestra en el portal de ADF y el cliente del entorno de ejecución de integración autohospedado durante **90 días** antes de la expiración.

## <a name="next-steps"></a>Pasos siguientes

- Revise los [conceptos del entorno de ejecución de integración en Azure Data Factory](./concepts-integration-runtime.md).

- Aprenda a [crear un entorno de ejecución de integración autohospedado en Azure Portal](./create-self-hosted-integration-runtime.md).
