---
title: Habilitación de la supervisión en Cloud Services (soporte extendido) mediante Azure Portal
description: Habilitación de la supervisión de instancias de Cloud Services (soporte extendido) mediante Azure Portal
ms.topic: how-to
ms.service: cloud-services-extended-support
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.date: 10/13/2020
ms.custom: ''
ms.openlocfilehash: 4591253e1a305632b7f41afe692f297d71366382
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98744116"
---
# <a name="enable-monitoring-for-cloud-services-extended-support-using-the-azure-portal"></a>Habilitación de la supervisión para Cloud Services (soporte extendido) mediante Azure Portal

En este artículo se explica cómo habilitar alertas en implementaciones existentes de Cloud Services (soporte extendido). 

## <a name="add-monitoring-rules"></a>Adición de reglas de supervisión
1. Inicie sesión en [Azure Portal](https://portal.azure.com). 
2. Seleccione la implementación de Cloud Services (soporte extendido) para la que quiere habilitar las alertas. 
3. Seleccione la hoja **Alertas**. 

    :::image type="content" source="media/enable-alerts-1.png" alt-text="Imagen que muestra la selección de la pestaña Alertas en Azure Portal.":::

4. Seleccione el icono **Nueva alerta**.
     :::image type="content" source="media/enable-alerts-2.png" alt-text="Imagen que muestra la selección de la opción Agregar nueva alerta.":::

5. Escriba las condiciones deseadas y las acciones necesarias en función de las métricas que le interese supervisar. Puede definir las reglas basadas en métricas individuales o en el registro de actividad. 

     :::image type="content" source="media/enable-alerts-3.png" alt-text="Imagen que muestra dónde agregar condiciones a las alertas.":::

     :::image type="content" source="media/enable-alerts-4.png" alt-text="Imagen que muestra la configuración de la lógica de señal.":::

     :::image type="content" source="media/enable-alerts-5.png" alt-text="Imagen que muestra la configuración de la lógica del grupo de acciones.":::

6. Cuando haya terminado de configurar las alertas, guarde los cambios y, en función de las métricas configuradas, comenzará a ver que la hoja **Alertas** se rellena con el tiempo.

## <a name="next-steps"></a>Pasos siguientes 
- Vea las [preguntas más frecuentes](faq.md) sobre Cloud Services (soporte extendido).
- Implemente una instancia de Cloud Services (soporte extendido) mediante [Azure Portal](deploy-portal.md), [PowerShell](deploy-powershell.md), una [plantilla](deploy-template.md) o [Visual Studio](deploy-visual-studio.md).