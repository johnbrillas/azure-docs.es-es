---
title: Recuperación de metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)
titleSuffix: Azure Load Balancer
description: Aprenda a recuperar los metadatos del equilibrador de carga con Azure Instance Metadata Service.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: how-to
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: 5196b03ccd513e4afd93b8b8fcf18f7c2580024a
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2021
ms.locfileid: "100519225"
---
# <a name="retrieve-load-balancer-metadata-using-the-azure-instance-metadata-service-imds"></a>Recuperación de metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)

## <a name="prerequisites"></a>Prerrequisitos

* Use la [última versión de la API](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#supported-api-versions) para su solicitud.

## <a name="sample-request-and-response"></a>Solicitud y respuesta de ejemplo
> [!IMPORTANT]
> En este ejemplo se omiten los servidores proxy. **Debe** omitir los servidores proxy al consultar IMDS. Para más información, vea [Servidores proxy](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#proxies).
### <a name="windows"></a>[Windows](#tab/windows/)

```powershell
Invoke-RestMethod -Headers @{"Metadata"="true"} -Method GET -NoProxy -Uri "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01" | ConvertTo-Json
```

### <a name="linux"></a>[Linux](#tab/linux/)

```bash
curl -H "Metadata:true" --noproxy "*" "http://169.254.169.254:80/metadata/loadbalancer?api-version=2020-10-01"
```

---
### <a name="sample-response"></a>Respuesta de muestra

```json
{
   "loadbalancer": {
    "publicIpAddresses":[
      {
         "frontendIpAddress":"51.0.0.1",
         "privateIpAddress":"10.1.0.4"
      }
   ],
   "inboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frontendIpAddress":"2603:10e1:100:2::1:1",
         "protocol":"tcp",
         "frontendPort":80,
         "backendPort":443,
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
   ],
   "outboundRules":[
      {
         "frontendIpAddress":"50.0.0.1",
         "privateIpAddress":"10.1.0.4"
      },
      {
         "frotendIpAddress":"2603:10e1:100:2::1:1",
         "privateIpAddress":"ace:cab:deca:deed::1"
      }
    ]
   }
}

```

## <a name="next-steps"></a>Pasos siguientes
[Códigos de error comunes y pasos para solucionar problemas](troubleshoot-load-balancer-imds.md)

Más información sobre [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md)

[Recuperación de todos los metadatos para una instancia](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#access-azure-instance-metadata-service)

[Implementación de un equilibrador de carga estándar](quickstart-load-balancer-standard-public-portal.md)

