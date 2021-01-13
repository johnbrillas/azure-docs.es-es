---
title: archivo de inclusión
description: archivo de inclusión
services: data-factory
author: lrtoyou1223
ms.service: data-factory
ms.topic: include
ms.date: 10/09/2019
ms.author: lle
ms.openlocfilehash: 4ef7bc7ed1ab255bffdfec10c07bc040019d9f1e
ms.sourcegitcommit: aacbf77e4e40266e497b6073679642d97d110cda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98121682"
---
| Nombres de dominio                  | Puertos de salida | Descripción                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.core.windows.net`          | 443            | Lo usa el entorno de ejecución de integración autohospedado para conectarse a la cuenta de Azure Storage cuando se utiliza la característica Copia almacenada provisionalmente. |
| `*.database.windows.net`      | 1433           | Solo es obligatorio cuando se copia desde o en Azure SQL Database o Azure Synapse Analytics. En caso contrario, es opcional. Use la característica de copia almacenada provisionalmente para copiar datos en SQL Database o Azure Synapse Analytics sin abrir el puerto 1433. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | Solo es necesario cuando se copia desde o en Azure Data Lake Store, sino es opcional. |
