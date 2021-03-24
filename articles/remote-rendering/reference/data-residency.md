---
title: Residencia de datos
description: Se describe la residencia de datos cuando se usa Azure Remote Rendering.
author: rapete
ms.author: rapete
ms.date: 02/04/2021
ms.topic: reference
ms.openlocfilehash: f20b3bb3de877ac627f5f6909c98cb9e1553f2a9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99577458"
---
# <a name="azure-remote-rendering-data-residency"></a>Residencia de datos de Azure Remote Rendering 
En este artículo se explica el concepto de residencia de datos y cómo se aplica a Azure Remote Rendering. 

## <a name="data-residency"></a>Residencia de datos 
Azure Remote Rendering usa contenedores de blobs de Azure proporcionados por el usuario para el almacenamiento de modelos. Cuando el modelo no está en uso, permanece en la región de Azure Blob Storage proporcionada por el usuario. Cuando el modelo se utiliza para la representación, los datos se copian en la región que elige el usuario al iniciar la sesión de representación.

## <a name="next-steps"></a>Pasos siguientes
Si quiere aprender a configurar un contenedor de Azure Blob Storage para Azure Remote Rendering, consulte [Conversión de un modelo para la representación](../quickstarts/convert-model.md).
