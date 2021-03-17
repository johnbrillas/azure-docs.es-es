---
title: Importación del entorno de Postman para llamadas REST de Azure Media Services
description: En este tema se proporciona una definición del entorno de Postman para llamadas REST de Azure Media Services.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2021
ms.author: inhenkel
ms.openlocfilehash: 5b1705fb2b3b1b69f79158747827d764f1bef4f0
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "103007856"
---
# <a name="import-the-postman-environment"></a>Importación del entorno de Postman

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Este artículo contiene una definición de las variables de entorno de **Postman** que se usan en la [colección de Postman](postman-collection.md) que contiene solicitudes HTTP agrupadas que llaman a las API de REST de Media Services. El tutorial [Configuración de Postman para llamadas API de REST de Media Services](media-rest-apis-with-postman.md) usa los archivos de colección y de entorno.

> [!NOTE]
> El valor de `AzureADSTSEndpoint ` = `https://login.microsoftonline.com/{{TenantId}}/oauth2/token`. Para obtener el identificador de inquilino, puede mantener el mouse sobre el nombre de usuario en el portal (en la esquina superior derecha) y estará en el "directorio: Microsoft ({{TENANTID}}).

```
{
  "id": "2dbce3ce-74c2-2ceb-0461-c4c2323f5b09",
  "name": "AzureMedia",
  "values": [
    {
      "enabled": true,
      "key": "TenantID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AzureADSTSEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "RESTAPIEndpoint",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientID",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "ClientSecret",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "AccessToken",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAssetId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastAccessPolicyId",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "UploadURL",
      "value": "",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "MediaFileName",
      "value": "BigBuckBunny.mp4",
      "type": "text"
    },
    {
      "enabled": true,
      "key": "LastChannelId",
      "value": "",
      "type": "text"
    }
  ],
  "_postman_variable_scope": "environment",
  "_postman_exported_using": "Postman/5.5.0"
}
```
