---
title: Solución de problemas de compresión de archivos en Azure Front Door Estándar/Premium
description: Aprenda a solucionar problemas con la compresión de archivos en Azure Front Door. En este artículo se tratan varias causas posibles.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2020
ms.author: qixwang
ms.openlocfilehash: c912ebde5499ec2f9f68e5f5762af60823faefe1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098268"
---
# <a name="troubleshooting-azure-front-door-standardpremium-file-compression"></a>Solución de problemas de compresión de archivos en Azure Front Door Estándar/Premium

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

Este artículo le ayuda a solucionar problemas de compresión de archivos en Azure Front Door Estándar/Premium.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="symptom"></a>Síntoma

Está habilitada la compresión para la ruta, pero se devuelven archivos sin comprimir.

> [!TIP]
> Para comprobar si los archivos se devuelven comprimidos, debe usar una herramienta como [Fiddler](https://www.telerik.com/fiddler) o las [herramientas de desarrollo](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) del explorador.  Compruebe los encabezados de respuesta HTTP que se devuelven con el contenido CDN en caché.  Si hay un encabezado denominado `Content-Encoding` con un valor de **gzip**, **bzip2**, o **deflate**, el contenido se comprime.
> 
> ![Encabezado content-encoding](../media/troubleshoot-compression/content-header.png)
> 

## <a name="cause"></a>Causa

Hay varias causas posibles, por nombrar algunas:

* El contenido solicitado no es apto para la compresión.
* La compresión no está habilitada para el tipo de archivo solicitado.
* La solicitud HTTP no incluía un encabezado que solicitara un tipo de compresión válido.
* El origen está enviando contenido fragmentado.

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

> [!TIP]
> Al igual que ocurre cuando se implementan puntos de conexión nuevos, los cambios en la configuración de Azure Front Door tardan un tiempo en propagarse por la red.  Normalmente, los cambios se aplican en un plazo de 90 minutos.  Si esta es la primera vez que configura la compresión para su punto de conexión de la red CDN, debe considerar una espera de 1 o 2 horas para asegurarse de que la configuración de la compresión se propagó a los POP. 
> 

### <a name="verify-the-request"></a>Comprobar la solicitud

En primer lugar, debemos comprobar la solicitud. Puede usar las **[herramientas de desarrollo](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)** del explorador para ver las solicitudes que se realizan.

* Compruebe que la solicitud se envía a la dirección URL del punto de conexión, `<endpointname>.z01.azurefd.net`, y no a su origen.
* Compruebe que la solicitud contenga un encabezado **Accept-Encoding** y que el valor para el encabezado contenga **gzip**, **deflate** o **bzip2**.

![Encabezados de solicitud de red CDN](../media/troubleshoot-compression/request-headers.png)

### <a name="verify-compression-settings"></a>Comprobación de la configuración de compresión

Desplácese hasta el punto de conexión en [Azure Portal](https://portal.azure.com) y seleccione el botón **Configurar** en el panel Rutas. Compruebe que la compresión está **habilitada**.

![Configuración de compresión de red CDN](../media/troubleshoot-compression/compression-settings.png)

### <a name="check-the-request-at-the-origin-server-for-a-via-header"></a>Compruebe la solicitud en el servidor de origen para un encabezado **Mediante**

El encabezado **Mediante** HTTP indica al servidor web que un servidor proxy pasará la solicitud.  De forma predeterminada, los servidores web de Microsoft IIS no comprimen las respuestas si la solicitud contiene un encabezado **Mediante**.  Para invalidar este comportamiento, realice lo siguiente:

* **IIS 6**: Establezca HcNoCompressionForProxies = "FALSE" en las propiedades de la metabase de IIS. Para más información, consulte [Compresión de IIS 6](/previous-versions/iis/6.0-sdk/ms525390(v=vs.90)).
* **IIS 7 y posteriores**: Establezca **noCompressionForHttp10** y **noCompressionForProxies** en *False* en la configuración del servidor. Para más información, consulte [Compresión HTTP](https://www.iis.net/configreference/system.webserver/httpcompression).
