---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 03/18/2021
ms.author: parkerra
ms.openlocfilehash: d91c0aeda2b7ae2f133d2099cbc9d238fd19d287
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "104719774"
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

> [!WARNING]
> El SDK 2.7.0 de ASA es la versión mínima compatible. Si usa Unity 2020, la versión mínima compatible es el SDK 2.9.0 de ASA.

El paquete principal de Azure Spatial Anchors se descargará en la carpeta donde se ejecutó el comando.

Repita este paso para descargar el paquete para cada plataforma (Android/iOS/HoloLens) que le gustaría admitir en el proyecto.

| Plataforma | Nombre del paquete                                    |
|----------|-------------------------------------------------|
| Android  | com.microsoft.azure.spatial-anchors-sdk.android@<version_number> |
| iOS      | com.microsoft.azure.spatial-anchors-sdk.ios@<version_number>     |
| HoloLens | com.microsoft.azure.spatial-anchors-sdk.windows@<version_number> |