---
title: Uso de Azure Front Door Estándar y Premium con uso compartido de recursos entre orígenes
description: Aprenda a usar Azure Front Door (AFD) con uso compartido de recursos entre orígenes (CORS).
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ee8f19aca62d2e331fcf59551d47c2dac93783b1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098113"
---
# <a name="using-azure-front-door-standardpremium-with-cross-origin-resource-sharing-cors"></a>Uso de Azure Front Door Estándar y Premium con uso compartido de recursos entre orígenes (CORS)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

## <a name="what-is-cors"></a>¿Qué es CORS?

CORS (Uso compartido de recursos entre orígenes) es una característica de HTTP que permite que una aplicación web que se ejecuta en un dominio tenga acceso a recursos de otro dominio. Para reducir la posibilidad de ataques de scripting entre sitios, todos los exploradores web modernos implementan una restricción de seguridad que se conoce como [directiva del mismo origen](https://www.w3.org/Security/wiki/Same_Origin_Policy). Esto impide que una página web llame a las API de un dominio distinto.  CORS aporta un modo seguro de permitir que un origen (el dominio de origen) llame a las API de otro origen.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Funcionamiento

Hay dos tipos de solicitudes de CORS, *solicitudes sencillas* y *solicitudes complejas.*

### <a name="for-simple-requests"></a>Para solicitudes sencillas:

1. El explorador envía la solicitud de CORS con otro encabezado de solicitud HTTP **Origin** (Origen). El valor de este encabezado es el origen del que proviene la página primaria, que se define como la combinación de *protocolo,* *dominio,* y *puerto.*  Cuando una página de https\://www.contoso.com intenta acceder a datos de un usuario en el origen fabrikam.com, se envía el siguiente encabezado de solicitud a fabrikam.com:

   `Origin: https://www.contoso.com`

2. El servidor puede responder con lo siguiente:

   * Un encabezado **Access-Control-Allow-Origin** en la respuesta, que indica cuál de los sitios de origen se permite. Por ejemplo:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Un código de error HTTP como el 403, si el servidor no permite la solicitud entre orígenes después de comprobar el encabezado Origin (Origen)

   * Un encabezado **Access-Control-Allow-Origin** con un carácter comodín que permite todos los dominios:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Para solicitudes complejas:

Una solicitud compleja es una solicitud de CORS en la que es necesario que el explorador envíe una *solicitud preparatoria* (es decir, un sondeo preliminar) antes de enviar la solicitud de CORS real. La solicitud preparatoria solicita el permiso del servidor si la solicitud de CORS original puede continuar y es una solicitud `OPTIONS` a la misma dirección URL.

> [!TIP]
> Para obtener más detalles sobre los flujos de CORS y los errores comunes, consulte la [guía de CORS para las API de REST](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/).
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Escenarios de origen único o carácter comodín
En Azure Front Door, CORS funcionará automáticamente sin ninguna configuración adicional cuando el encabezado **Access-Control-Allow-Origin** esté establecido en un carácter comodín (*) o en un solo origen.  Azure Front Door almacenará en caché la primera respuesta y las solicitudes siguientes usarán el mismo encabezado.

Si ya se han enviado solicitudes a Azure Front Door antes de establecer CORS en el origen, tendrá que purgar el contenido del punto de conexión para volver a cargarlo con el encabezado **Access-Control-Allow-Origin**.

## <a name="multiple-origin-scenarios"></a>Escenarios de varios orígenes
Si necesita permitir una lista específica de orígenes admitidos para CORS, el proceso puede resultar un poco más complicado. Encontramos el primer problema cuando la red CDN copia en caché el encabezado **Access-Control-Allow-Origin** el primer origen de CORS.  Cuando otro origen de CORS realice otra solicitud, la red CDN enviará el encabezado **Access-Control-Allow-Origin** almacenado en caché, que no coincidirá. Existen diversas formas de corregir este problema.

### <a name="azure-front-door-rule-set"></a>Conjunto de reglas de Azure Front Door

En Azure Front Door, puede crear una regla en el [conjunto de reglas](concept-rule-set.md) de Azure Front Door para comprobar el encabezado **Origin** (Origen) de la solicitud. Si se trata de un origen válido, la regla establecerá el encabezado **Access-Control-Allow-Origin** en el valor deseado. En este caso, el encabezado **Access-Control-Allow-Origin** del servidor de origen del archivo se ignora y el motor de reglas de AFD administra totalmente los orígenes de CORS admitidos.

:::image type="content" source="../media/troubleshooting-cross-origin-resource-sharing/cross-origin-resource.png" alt-text="Captura de pantalla del ejemplo de reglas con el conjunto de reglas.":::

> [!TIP]
> Puede agregar acciones adicionales a su rol para modificar encabezados de respuesta adicionales, como **Access-Control-Allow-Methods**.
> 

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear una instancia de Front Door](create-front-door-portal.md).
