---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 2/3/21
ms.author: parkerra
ms.openlocfilehash: f6c2780ccbb914228a9870cb3b5fe4b0e3d0b214
ms.sourcegitcommit: f82e290076298b25a85e979a101753f9f16b720c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99569604"
---
Para descargar los paquetes necesarios, debe tener <a href="https://www.npmjs.com/get-npm" target="_blank">NPM</a> instalado.

Ejecute el siguiente comando reemplazando `<version_number>` por la versión de Azure Spatial Anchors que desea descargar en la carpeta actual:

```bash
npm pack com.microsoft.azure.spatial-anchors-sdk.core@<version_number> --registry https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM
```

> [!NOTE]
> Para enumerar las versiones disponibles del paquete de Azure Spatial Anchors, ejecute lo siguiente:
>
> ```bash
> npm view com.microsoft.azure.spatial-anchors-sdk.core --registry https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM versions
> ```

El paquete principal de Azure Spatial Anchors se descargará en la carpeta donde se ejecutó el comando.

Repita este paso para descargar el paquete para cada plataforma (Android/iOS/HoloLens) que le gustaría admitir en el proyecto.

| Plataforma | Nombre del paquete                                    |
|----------|-------------------------------------------------|
| Android  | com.microsoft.azure.spatial-anchors-sdk.android@<version_number> |
| iOS      | com.microsoft.azure.spatial-anchors-sdk.ios@<version_number>     |
| HoloLens | com.microsoft.azure.spatial-anchors-sdk.windows@<version_number> |