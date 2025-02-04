---
title: Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal
titleSuffix: Azure
description: Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: a52e6038b622c004dc0d133394cd4f53600b2935
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "84700049"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-the-azure-portal"></a>Habilitación de Azure Peering Service en un emparejamiento directo mediante Azure Portal

En este artículo se describe cómo habilitar [Peering Service](overview-peering-service.md) en un emparejamiento directo mediante Azure Portal.

Si lo prefiere, puede completar esta guía mediante [PowerShell](howto-peering-service-powershell.md).

## <a name="before-you-begin"></a>Antes de empezar
* Revise los [requisitos previos](prerequisites.md) antes de comenzar la configuración.
* Elija un emparejamiento directo en la suscripción en la que quiera habilitar Peering Service. Si no tiene ninguna, convierta un emparejamiento directo heredado o cree uno:
    * Para convertir un emparejamiento directo heredado, siga las instrucciones que se indican en [Conversión de un emparejamiento directo heredado en un recurso de Azure mediante el portal](howto-legacy-direct-portal.md).
    * Para crear un emparejamiento directo, siga las instrucciones que se indican en [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="enable-peering-service-on-a-direct-peering"></a>Habilitación de Peering Service en un emparejamiento directo

### <a name="view-direct-peering"></a><a name= get></a>Visualización de un emparejamiento directo
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Habilitación del emparejamiento directo para Peering Service

Después de abrir un emparejamiento directo en el paso anterior, habilítelo para Peering Service.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Modificación de una conexión de emparejamiento directo

Para modificar la configuración de la conexión, consulte la sección "Modificación de un emparejamiento directo" del artículo [Creación o modificación de un emparejamiento directo mediante el portal](howto-direct-portal.md).

## <a name="next-steps"></a>Pasos siguientes

* [Creación o modificación de un emparejamiento de Exchange mediante el portal](howto-exchange-portal.md)
* [Conversión de un emparejamiento de Exchange heredado en un recurso de Azure mediante el portal](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Recursos adicionales

Puede consultar las preguntas frecuentes en [Preguntas más frecuentes sobre Peering Service](service-faqs.md).