---
title: 'Azure Front Door: Conjunto de reglas'
description: En este artículo se ofrece una introducción sobre la característica Conjunto de reglas de Azure Front Door Estándar/Prémium.
services: front-door
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 8e6ceebc9e92dabe66baeb9aeff0ae9692e2bdad
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098217"
---
# <a name="what-is-a-rule-set-for-azure-front-door-standardpremium-preview"></a>¿Qué es un conjunto de reglas de Azure Front Door Estándar/Premium (versión preliminar)?

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

Un conjunto de reglas es un motor de reglas personalizado que agrupa una combinación de reglas en un único conjunto que se puede asociar a varias rutas. El conjunto de reglas le permite personalizar el modo en que las solicitudes se procesan en el perímetro y cómo las controla Azure Front Door.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="common-supported-scenarios"></a>Escenarios admitidos habituales

* Implemente encabezados de seguridad para evitar vulnerabilidades basadas en el explorador como HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy y X-Frame-Options, además de encabezados Access-Control-Allow-Origin para escenarios de uso compartido de recursos entre orígenes (CORS). Los atributos basados en seguridad también se pueden definir con cookies.

* Enrute las solicitudes a la versión móvil o de escritorio de la aplicación en función del tipo de dispositivo cliente.

* Use las funcionalidades de redireccionamiento para devolver los redireccionamientos 301, 302, 307 y 308 al cliente para dirigirlos a nuevos nombres de host, rutas de acceso, cadenas de consulta o protocolos.

* Modifique de forma dinámica la configuración de almacenamiento en caché de la ruta en función de las solicitudes entrantes.

* Vuelva a escribir la ruta de acceso de la dirección URL de la solicitud y reenvíe la solicitud al origen adecuado del grupo de orígenes configurado.

* Agregue, modifique o elimine el encabezado de solicitud o respuesta para ocultar la información confidencial o capturar información importante a través de los encabezados.

* Admita variables de servidor para cambiar dinámicamente los encabezados de solicitud o respuesta, o las rutas de acceso de reescritura de URL y las cadenas de consulta, por ejemplo, al cargar una nueva página o cuando se publica un formulario. La variable de servidor se admite actualmente solo en las **[acciones del conjunto de reglas](concept-rule-set-actions.md)** .

## <a name="architecture"></a>Architecture

El conjunto de reglas controla las solicitudes en el perímetro. Cuando llega una solicitud al punto de conexión de Azure Front Door Estándar/Premium, se ejecuta primero el firewall de aplicaciones web, seguido de los valores configurados de ruta. Estos valores incluyen el conjunto de reglas asociado a la ruta. Los conjuntos de reglas se procesan de arriba a abajo en la ruta. Lo mismo se aplica a las reglas de un conjunto de reglas. Para que todas las acciones de cada regla se ejecuten, deben cumplirse todas las condiciones de coincidencia dentro de una regla. Si una solicitud no coincide con ninguna de las condiciones de la configuración del conjunto de reglas, solo se ejecutarán las configuraciones de la ruta.

Si la opción **Detener la evaluación de las reglas restantes** se activa, no se ejecutará ninguno de los conjuntos de reglas restantes asociados a la ruta.  

### <a name="example"></a>Ejemplo

En el diagrama siguiente, las directivas del firewall de aplicaciones web se ejecutan en primer lugar. Un conjunto de reglas se configura para anexar un encabezado de respuesta. El encabezado cambia la duración máxima del control de la caché si se cumple la condición de coincidencia.

:::image type="content" source="../media/concept-rule-set/front-door-rule-set-architecture-1.png" alt-text="Diagrama que muestra la arquitectura del conjunto de reglas." lightbox="../media/concept-rule-set/front-door-rule-set-architecture-1-expanded.png":::

## <a name="terminology"></a>Terminología

Con un conjunto de reglas de Azure Front Door, puede crear una combinación de configuraciones del conjunto de reglas, cada una de las cuales se compone de un conjunto de reglas. A continuación se describen ciertos términos útiles que se utilizarán al configurar el conjunto de reglas.

Para más información sobre el límite de cuota, consulte [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](../../azure-resource-manager/management/azure-subscription-service-limits.md).

* *Conjunto de reglas*: conjunto de reglas que se asocia a una o varias [rutas](concept-route.md). Cada configuración se limita a 25 reglas. Puede crear hasta 10 configuraciones.

* *Regla del conjunto de reglas*: una regla compuesta de un máximo de 10 condiciones de coincidencia y 5 acciones. Las reglas son locales para un conjunto de reglas y no se pueden exportar para usarlas en conjuntos de reglas. Los usuarios pueden crear la misma regla en varios conjuntos de reglas.

* *Condición de coincidencia*: Se pueden usar varias condiciones de coincidencia para analizar las solicitudes entrantes. Una regla puede contener hasta 10 condiciones de coincidencia. Las condiciones de coincidencia se evalúan con un operador **AND**. *Se admiten las expresiones regulares en las condiciones*. Se puede encontrar una lista completa de las condiciones de coincidencia en [Condiciones de un conjunto de reglas](concept-rule-set-match-conditions.md).

* *Acción*: las acciones determinan cómo Azure Front Door controla las solicitudes entrantes en función de las condiciones de coincidencia. Puede modificar los comportamientos de almacenamiento en caché, modificar encabezados de solicitud o de respuesta, realizar la reescritura de direcciones URL y la redirección de direcciones URL. *Se admiten las variables de servidor en la acción*. Una regla puede contener hasta 10 condiciones de coincidencia. En [Acciones de un conjunto de reglas](concept-rule-set-actions.md) encontrará una lista completa de las acciones.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear una instancia de Front Door Estándar o Premium](create-front-door-portal.md).
* Aprenda a configurar el primer [conjunto de reglas](how-to-configure-rule-set.md).
 
