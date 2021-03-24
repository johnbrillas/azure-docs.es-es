---
title: Configuración de las condiciones de coincidencia de un conjunto de reglas de Azure Front Door Estándar/Prémium
description: En este artículo se proporciona una lista de las distintas condiciones de coincidencia disponibles con un conjunto de reglas de Azure Front Door Estándar/Premium.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: conceptual
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 4c65d0e7f80fab59ca7df4849df7117d482352c1
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098231"
---
# <a name="azure-front-door-standardpremium-preview-rule-set-match-conditions"></a>Condiciones de coincidencia de un conjunto de reglas de Azure Front Door Estándar/Prémium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

En este tutorial se muestra cómo crear un conjunto de reglas con el primer conjunto de reglas en Azure Portal. En un [conjunto de reglas](concept-rule-set.md) de Azure Front Door Standard/Premium, una regla consta de cero, o más, condiciones de coincidencia y una acción. En este artículo se muestran descripciones detalladas de las condiciones de coincidencia que puede usar en un conjunto de reglas de Azure Front Door Estándar/Premium.

La primera parte de una regla es una condición de coincidencia o un conjunto de condiciones de coincidencia. Una regla puede contener hasta 10 condiciones de coincidencia. Una condición de coincidencia identifica tipos específicos de solicitudes para los que se realizan las acciones definidas. Si usa varias condiciones de coincidencia, estas se agruparán mediante la lógica AND. Para todas las condiciones de coincidencia que admiten varios valores (que se indican a continuación como "separadas por espacios"), se supone el operador "OR".

Por ejemplo, puede usar una condición de coincidencia para:

* Filtrar las solicitudes en función de una dirección IP específica, un país o una región.
* Filtrar las solicitudes por la información de encabezado.
* Filtrar las solicitudes de los dispositivos móviles o de escritorio.
* Filtrar las solicitudes de nombre de archivo y extensión de archivo de solicitud.
* Filtrar las solicitudes de la dirección URL de la solicitud, el protocolo, la ruta de acceso, la cadena de consulta, los argumentos POST, etc.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Las siguientes condiciones de coincidencia están disponibles para su uso en un conjunto de reglas de Azure Front Door Estándar/Premium:

## <a name="device-type"></a>Tipo de dispositivo

Identifica las solicitudes realizadas desde un dispositivo móvil o un dispositivo de escritorio.  

#### <a name="required-fields"></a>Campos obligatorios

Operator | Valores admitidos
---------|----------------
Es igual a, No es igual a | Móvil, Escritorio

## <a name="post-argument"></a>Argumento POST

Identifica las solicitudes en función de los argumentos definidos para el método POST que se usa en la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Nombre de argumento | Operator | Valor de argumento | Transformación de mayúsculas y minúsculas
--------------|----------|----------------|---------------
String | [Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

## <a name="query-string"></a>Cadena de consulta

Identifica las solicitudes que contienen el parámetro de cadena de consulta especificado. Este parámetro se establece en un valor que coincide con un patrón específico. Los parámetros de cadena de consulta (por ejemplo, **parameter=value**) de la dirección URL de la solicitud determinan si se cumple esta condición. Esta condición de coincidencia identifica un parámetro de cadena de consulta por su nombre y acepta uno o varios valores para el valor del parámetro.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Cadena de consulta | Transformación de caso
---------|--------------|---------------
[Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

## <a name="remote-address"></a>Dirección remota

Identifica las solicitudes en función de la ubicación o la dirección IP del solicitante.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Valores admitidos
---------|-----------------
Coincidencia geográfica | Código de país
Coincidencia de IP | Dirección IP (separada por espacios)
No tiene ninguna coincidencia geográfica | Código de país
No tiene ninguna coincidencia de IP | Dirección IP (separada por espacios)

#### <a name="key-information"></a>Información importante

* Utilice la notación CIDR.
* Para varias direcciones IP y bloques de direcciones IP, se opera el "OR" lógico.
    * **Ejemplo de IPv4**: si agrega dos direcciones IP, *1.2.3.4* y *10.20.30.40*, se compara la condición si las solicitudes que llegan desde las direcciones 1.2.3.4 o 10.20.30.40.
    * **Ejemplo de IPv6**: si agrega dos direcciones IP, *1:2:3:4:5:6:7:8* y *10:20:30:40:50:60:70:80*, se compara la condición si las solicitudes que llegan desde las direcciones 1:2:3:4:5:6:7:8 o 10:20:30:40:50:60:70:80.
* La sintaxis de un bloque de direcciones IP es la dirección IP de base seguida por una barra diagonal y el tamaño del prefijo. Por ejemplo:
    * **Ejemplo IPv4**: *5.5.5.64/26* coincide con las solicitudes que llegan desde la dirección 5.5.5.64 hasta la 5.5.5.127.
    * **Ejemplo IPv6**: *1:2:3:/48* coincide con las solicitudes que lleguen desde la dirección 1:2:3:0:0:0:0:0 hasta la 1:2:3:ffff:ffff:ffff:ffff:ffff.

## <a name="request-body"></a>Cuerpo de la solicitud

Identifica las solicitudes en función del texto específico que aparece en el cuerpo de la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Cuerpo de la solicitud | Transformación de mayúsculas y minúsculas
---------|--------------|---------------
[Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

## <a name="request-header"></a>Encabezado de solicitud

Identifica las solicitudes que usan un encabezado específico en la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Nombre de encabezado | Operator | Valor de encabezado | Transformación de mayúsculas y minúsculas
------------|----------|--------------|---------------
String | [Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

## <a name="request-method"></a>Método de solicitud

Identifica las solicitudes que usan el método de solicitud especificado.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Valores admitidos
---------|----------------
Es igual a, No es igual a | GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE

#### <a name="key-information"></a>Información importante

Solo el método de solicitud GET puede generar contenido almacenado en caché en Azure Front Door. Los demás métodos de solicitud los procesa el proxy mediante la red.

## <a name="request-protocol"></a>Protocolo de solicitud

Identifica las solicitudes que usan el protocolo especificado.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Valores admitidos
---------|----------------
Es igual a, No es igual a | HTTP, HTTPS

## <a name="request-url"></a>URL de la solicitud

Identifica las solicitudes que coinciden con la dirección URL especificada.

#### <a name="required-fields"></a>Campos obligatorios

Operator | URL de la solicitud | Transformación de mayúsculas y minúsculas
---------|-------------|---------------
[Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

#### <a name="key-information"></a>Información importante

Cuando use esta condición de regla, asegúrese de incluir la información del protocolo. Por ejemplo, *https://www.\<yourdomain\>.com*.

## <a name="request-file-extension"></a>Solicitar extensión de archivo

Identifica las solicitudes que incluyen la extensión de archivo especificada en el nombre de archivo de la dirección URL de la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Extensión | Transformación de mayúsculas y minúsculas
---------|-----------|---------------
[Lista de operadores](#operator-list)  | Cadena, Int | Minúsculas, mayúsculas

#### <a name="key-information"></a>Información importante

En el caso de la extensión, no incluya un punto inicial; por ejemplo, utilice *html* en lugar de *.html*.

## <a name="request-file-name"></a>Solicitar nombre de archivo

Identifica las solicitudes que incluyen el nombre de archivo especificado en la dirección URL de la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Nombre de archivo | Transformación de mayúsculas y minúsculas
---------|-----------|---------------
[Lista de operadores](#operator-list)| Cadena, Int | Minúsculas, mayúsculas

## <a name="request-path"></a>Ruta de acceso de solicitud

Identifica las solicitudes que incluyen la ruta de acceso especificada en la dirección URL de la solicitud.

#### <a name="required-fields"></a>Campos obligatorios

Operator | Value | Transformación de caso
---------|-------|---------------
[Lista de operadores](#operator-list) | Cadena, Int | Minúsculas, mayúsculas

## <a name="operator-list"></a><a name = "operator-list"></a>Lista de operadores

En el caso de las reglas que aceptan valores de la lista de operadores estándar, son válidos los siguientes operadores:

* Any
* Equals
* Contains
* Empieza por
* Termina por
* Menor que
* Menor o igual que
* Mayor que
* Mayor o igual que
* Ninguno
* No contiene
* No empieza por
* No termina con
* No es menor que
* No es menor o igual que
* No es mayor que
* No es mayor o igual que
* Expresión regular

En el caso de los operadores numéricos, como *Menor que* o *Mayor o igual que*, la comparación usada se basa en la longitud. El valor de la condición de coincidencia debe ser un entero, igual a la longitud que desee comparar.

## <a name="regular-expression"></a>Expresión regular

Regex no admite las siguientes operaciones:

* Referencias inversas y captura de subexpresiones
* Aserciones arbitrarias de ancho cero
* Referencias de subrutinas y patrones recursivos
* Patrones condicionales
* Verbos de control de retroceso
* Directiva de un solo byte \C
* Directiva de coincidencia de nueva línea \R
* Directiva de inicio de restablecimiento de coincidencia \K
* Llamadas y código insertado
* Agrupación atómica y cuantificadores de posesivos

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el [conjunto de reglas](concept-rule-set.md).
* Aprenda a [configurar el primer conjunto de reglas](how-to-configure-rule-set.md).
* Más información sobre las [acciones del conjunto de reglas](concept-rule-set-actions.md).
