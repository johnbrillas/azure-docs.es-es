---
title: Códigos de error comunes de Azure Instance Metadata Service (IMDS)
titleSuffix: Azure Load Balancer
description: Información general sobre los códigos de error comunes y los métodos de mitigación correspondientes de Azure Instance Metadata Service (IMDS)
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: troubleshooting
ms.date: 02/12/2021
ms.author: allensu
ms.openlocfilehash: e932e211996a05b2740613381735a7de3492e5bf
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100519191"
---
# <a name="error-codes-common-error-codes-when-using-imds-to-retrieve-load-balancer-information"></a>Códigos de error: Códigos de error comunes al usar IMDS para recuperar información del equilibrador de carga

En este artículo se describen los errores de implementación comunes y cómo resolverlos al usar Azure Instance Metadata Service (IMDS).

## <a name="error-codes"></a>Códigos de error

| Código de error | Mensaje de error | Detalles y mitigación |
| --- | ---------- | ----------------- |
| 400 | Falta el parámetro obligatorio: "\<ParameterName>". Corrija la solicitud y vuelva a intentarlo. | El código de error indica que falta un parámetro. </br> Para más información sobre cómo agregar el parámetro que falta, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service de Azure (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).
| 400 | No se permite el valor del parámetro o no se permite el valor del parámetro "\<ParameterValue>" para el parámetro "ParameterName". Corrija la solicitud y vuelva a intentarlo. | El código de error indica que el formato de la solicitud no está configurado correctamente. </br> Para más información, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response) para corregir el cuerpo de la solicitud y emitir un reintento. |
| 400 | Solicitud inesperada. Compruebe los parámetros de consulta y vuelva a intentarlo. | El código de error indica que el formato de la solicitud no está configurado correctamente. </br> Para más información, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response) para corregir el cuerpo de la solicitud y emitir un reintento. |
| 404 | No se encuentran metadatos del equilibrador de carga. Compruebe si la máquina virtual usa un equilibrador de carga de SKU no básica e inténtelo de nuevo más tarde. | El código de error indica que la máquina virtual no está asociada a un equilibrador de carga o que el equilibrador de carga es una SKU básica en lugar de estándar. </br> Para más información, consulte [Inicio rápido: Cree un equilibrador de carga público para equilibrar la carga de las máquinas virtuales mediante Azure Portal](quickstart-load-balancer-standard-public-portal.md?tabs=option-1-create-load-balancer-standard) para implementar un equilibrador de carga estándar.|
| 404 | API no encontrada: ruta de acceso = "\<UrlPath>", método = "\<Method>" | El código de error indica un error de configuración de la ruta de acceso. </br> Para más información, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response) para corregir el cuerpo de la solicitud y emitir un reintento.|
| 405 | El método HTTP no está permitido: ruta de acceso = "\<UrlPath>", método = "\<Method>" | El código de error indica un verbo HTTP no admitido. </br> Para más información, consulte [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#http-verbs) para ver los verbos admitidos. |
| 429 | Demasiadas solicitudes | El código de error indica un límite de frecuencia. </br> Para más información sobre la limitación de frecuencia, consulte [Azure Instance Metadata Service (IMDS)](../virtual-machines/windows/instance-metadata-service.md?tabs=windows#rate-limiting).|
| 400 | El cuerpo de la solicitud es mayor que MaxBodyLength: ... | El código de error indica una solicitud mayor que MaxBodyLength. </br> Para más información sobre la longitud del cuerpo, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | La longitud de la clave del parámetro es mayor que MaxParameterKeyLength: ... | El código de error indica una longitud de clave de parámetro mayor que MaxParameterKeyLength. </br> Para más información sobre la longitud de la clave, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response). |
| 400 | La longitud del valor del parámetro es mayor que MaxParameterValueLength: ... | El código de error indica una longitud de clave del parámetro mayor que MaxParameterValueLength. </br> Para más información sobre la longitud del valor, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 400 | La longitud del valor del encabezado del parámetro es mayor que MaxHeaderValueLength: ... | El código de error indica que la longitud del valor del encabezado del parámetro es mayor que MaxHeaderValueLength. </br> Para más información sobre la longitud del valor, consulte [Cómo recuperar metadatos del equilibrador de carga mediante Azure Instance Metadata Service (IMDS)](howto-load-balancer-imds.md#sample-request-and-response).|
| 404 | La API de metadatos de Load Balancer no está disponible en este momento. Inténtelo de nuevo más tarde. | El código de error indica que la API podría estar en proceso de aprovisionamiento. Intente la solicitud más tarde. |
| 404 | /metadata/loadbalancer no está disponible actualmente | El código de error indica que la API está en proceso de habilitación. Intente la solicitud más tarde. |
| 503 | Servicio interno no disponible. Inténtelo de nuevo más tarde.  | El código de error indica que la API no está disponible temporalmente. Intente la solicitud más tarde. |
|  |  |

## <a name="next-steps"></a>Pasos siguientes

Más información sobre [Azure Instance Metadata Service](../virtual-machines/windows/instance-metadata-service.md).

