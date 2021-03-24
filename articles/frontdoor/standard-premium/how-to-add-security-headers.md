---
title: Configuración de encabezados de seguridad con el conjunto de reglas de Azure Front Door Estándar/Premium (versión preliminar)
description: En este artículo se proporcionan instrucciones sobre cómo usar el conjunto de reglas para configurar encabezados de seguridad.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: c08ba57f43969bb2f0ee9c66b6cb4e92879ed258
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098203"
---
# <a name="configure-security-headers-with-azure-front-door-standardpremium-preview-rule-set"></a>Configuración de encabezados de seguridad con el conjunto de reglas de Azure Front Door Estándar/Premium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

En este artículo se indica cómo implementar encabezados de seguridad para evitar vulnerabilidades basadas en el explorador como HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy o X-Frame-Options. Los atributos basados en seguridad también se pueden definir con cookies.

En el ejemplo siguiente se muestra cómo agregar un encabezado Content-Security-Policy a todas las solicitudes entrantes que coinciden con la ruta de acceso definida en la ruta. En este caso, solo se permiten la ejecución de scripts de nuestro sitio de confianza, **https://apiphany.portal.azure-api.net** , en nuestra aplicación.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

* Antes de configurar los encabezados de seguridad, primero debe crear una puerta principal. Para más información, consulte [Inicio rápido: Cree una instancia de Front Door](create-front-door-portal.md).
* Revise cómo [configurar un conjunto de reglas](how-to-configure-rule-set.md) si no ha usado antes la característica Conjunto de reglas.

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Adición de un encabezado Content-Security-Policy en Azure Portal

1. Vaya al perfil de Azure Front Door Estándar/Premium y seleccione **Conjunto de reglas** en **Configuración**.

1. Seleccione **Agregar** para agregar un nuevo conjunto de reglas. Asigne un **nombre** al conjunto de reglas y, a continuación, proporcione un **nombre** para la regla. Seleccione **Agregar una acción** y, a continuación, seleccione **Encabezado de respuesta**.

1. Establezca el operador en **Anexar** para agregar este encabezado como respuesta a todas las solicitudes entrantes para esta ruta.

1. Agregue el nombre del encabezado: **Content-Security-Policy** y defina los valores que debe aceptar este encabezado. En este escenario, hemos elegido *"script-src 'self' https://apiphany.portal.azure-api.net"* .

1. Una vez que haya agregado todas las reglas que le gustaría a su configuración, no olvide asociar el conjunto de reglas con una ruta. Este paso es *necesario* para permitir que el conjunto de reglas tome medidas. 

> [!NOTE]
> En este escenario, no hemos agregado [condiciones de coincidencia](concept-rule-set-match-conditions.md) a la regla. Esta regla se aplicará a todas las solicitudes entrantes que coincidan con la ruta de acceso definida en la ruta asociada. Si desea que solo se aplique a un subconjunto de esas solicitudes, asegúrese de agregar las **condiciones de coincidencia** específicas a esta regla.

## <a name="clean-up-resources"></a>Limpieza de recursos

### <a name="deleting-a-rule"></a>Eliminación de una regla

En los pasos anteriores, configuró el encabezado Content-Security-Policy con un conjunto de reglas. Si ya no desea una regla, puede seleccionar el nombre del conjunto de reglas y, a continuación, seleccionar Eliminar regla. 

### <a name="deleting-a-rule-set"></a>Eliminación de un conjunto de reglas

Si desea eliminar un conjunto de reglas, asegúrese de que lo desasocia de todas las rutas antes de eliminarlo. Para obtener instrucciones detalladas sobre cómo eliminar un conjunto de reglas, consulte [Configuración del conjunto de reglas](how-to-configure-rule-set.md).

## <a name="next-steps"></a>Pasos siguientes

Para aprender a configurar un firewall de aplicaciones web para Front Door, consulte [Firewall de aplicaciones web y Front Door](../../web-application-firewall/afds/afds-overview.md).
