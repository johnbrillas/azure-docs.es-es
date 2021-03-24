---
title: Implementación de Azure SQL Edge en las turbinas de un parque eólico Contoso
description: En este tutorial usará Azure SQL Edge para la detección de estelas en las turbinas de un parque eólico Contoso.
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: tutorial
author: VasiyaKrishnan
ms.author: vakrishn
ms.reviewer: sstein
ms.date: 12/18/2020
ms.openlocfilehash: e966d756744210dc8f6739b96501dd91f0d8d1b5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "97723480"
---
# <a name="using-azure-sql-edge-to-build-smarter-renewable-resources"></a>Uso de Azure SQL Edge para crear recursos renovables más inteligentes

Esta demostración de Azure SQL Edge se basa en una energía renovable de Contoso, un parque eólico de turbinas que usa SQL DB Edge para el procesamiento de datos en el generador. 

Esta demostración le guiará por la resolución de una alerta que se genera debido a la detección de turbulencias por viento en el dispositivo. Entrenará un modelo y lo implementará en SQL DB Edge, que corregirá la estela de viento detectada y, en última instancia, optimizará la salida de potencia.

Azure SQL Edge: vídeo de demostración de energía renovable en el canal 9:
> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Azure-SQL-Edge-Demo-Renewable-Energy/player]

## <a name="setting-up-the-demo-on-your-local-computer"></a>Configuración de la demostración en la máquina local
Se usará GIT para copiar todos los archivos de la demostración en la máquina local. 

1. Instale GIT desde [aquí](https://git-scm.com/download).
2. Abra un símbolo del sistema y vaya a una carpeta donde descargar el repositorio. 
3. Emita el comando https://github.com/microsoft/sql-server-samples.git:
4. Vaya a **"sql-server-samples\samples\demos\azure-sql-edge-demos\Wind Turbine Demo"** en la ubicación donde se ha clonado el repositorio.
5. Siga las instrucciones de README.md para configurar el entorno de demostración y ejecutar la demostración.