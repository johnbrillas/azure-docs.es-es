---
title: Conexión a GitHub
description: Uso de GitHub para especificar referencias de entidad de Common Data Model
author: djpmsft
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: daperlov
ms.openlocfilehash: 5d555d7bc4d3aae9c016cacbe17b68c30859d99a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100372287"
---
# <a name="use-github-to-read-common-data-model-entity-references"></a>Uso de GitHub para leer referencias de entidad de Common Data Model

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

El conector de GitHub en Azure Data Factory sirve únicamente para recibir el esquema de referencia de entidades para el formato de [Common Data Model](format-common-data-model.md) en el flujo de datos de asignación.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de GitHub.

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type | La propiedad type debe establecerse en **GitHub**. | sí
| userName | Nombre de usuario de GitHub | sí |
| password | Contraseña de GitHub | sí |

## <a name="next-steps"></a>Pasos siguientes

Cree un [conjunto de datos de origen](data-flow-source.md) en el flujo de datos de asignación.