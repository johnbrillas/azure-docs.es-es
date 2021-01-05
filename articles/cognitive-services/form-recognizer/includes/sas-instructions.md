---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 12/11/2020
ms.author: pafarley
ms.openlocfilehash: 3f4272f5445c8530e97726bd4fdca6bf662719c2
ms.sourcegitcommit: 02ed9acd4390b86c8432cad29075e2204f6b1bc3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97807814"
---
Para recuperar la dirección URL de la firma de acceso compartido para los datos de entrenamiento del modelo personalizado, vaya al recurso de almacenamiento en Azure Portal y seleccione la pestaña **Explorador de Storage**. Vaya al contenedor, haga clic con el botón derecho y seleccione **Obtener firma de acceso compartido**. Es importante obtener la firma de acceso compartido para el contenedor, no para la propia cuenta de almacenamiento. Asegúrese de que los permisos de **lectura** y **enumeración** están marcados y haga clic en **Create** (Crear). A continuación, copie el valor de la sección **URL** en una ubicación temporal. Debe tener el formato `https://<storage account>.blob.core.windows.net/<container name>?<SAS value>`.