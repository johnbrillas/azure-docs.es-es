---
title: Métodos de enrutamiento del tráfico de Azure Traffic Manager
description: Este artículo lo ayudará a entender los distintos métodos de enrutamiento de tráfico que emplea Traffic Manager.
services: traffic-manager
author: duongau
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/21/2021
ms.author: duau
ms.openlocfilehash: eeebded128f636ecba2e4e0dab1bc2f0632aaa6a
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98730979"
---
# <a name="traffic-manager-routing-methods"></a>Métodos de enrutamiento del Administrador de tráfico

Azure Traffic Manager admite seis métodos de enrutamiento de tráfico para determinar cómo enrutar el tráfico de red a los diversos puntos de conexión del servicio. Para cualquier perfil, Traffic Manager aplica el método de enrutamiento de tráfico asociado a cada consulta de DNS que recibe. El método de enrutamiento de tráfico determina qué punto de conexión se devuelve en la respuesta de DNS.

En Traffic Manager, los métodos de enrutamiento de tráfico disponibles son los siguientes:

* **[Prioridad](#priority-traffic-routing-method):** Seleccione el enrutamiento **Prioridad** cuando desee tener un punto de conexión de servicio principal para todo el tráfico. Puede proporcionar varios puntos de conexión de reserva en caso de que el principal o uno de los de reserva no esté disponible.
* **[Ponderado](#weighted):** seleccione el enrutamiento **Ponderado** cuando desee distribuir el tráfico a través de un conjunto de puntos de conexión en función de su ponderación. Establezca el mismo valor de ponderación para distribuir el tráfico uniformemente entre todos los puntos de conexión.
* **[Rendimiento](#performance):** seleccione el enrutamiento **Rendimiento** cuando tenga puntos de conexión en diferentes ubicaciones geográficas y quiera que los usuarios finales utilicen el punto de conexión "más cercano" según la latencia de red más baja.
* **[Geográfico](#geographic):** seleccione el enrutamiento **Geográfico** para dirigir a los usuarios a puntos de conexión concretos (Azure, Externo o Anidado) en función de la ubicación geográfica de la que partan sus consultas de DNS. Con este método de enrutamiento, podrá satisfacer los requisitos de escenarios como los mandatos de soberanía de datos, la localización del contenido y la experiencia del usuario, y la medición del tráfico de distintas regiones.
* **[Multivalor](#multivalue):** seleccione **Multivalor** para los perfiles de Traffic Manager que solo pueden tener direcciones IPv4/IPv6 como puntos de conexión. Cuando se recibe una consulta para este perfil, se devuelven todos los puntos de conexión correctos.
* **[Subred](#subnet):** seleccione el método de enrutamiento de tráfico **Subred** para asignar conjuntos de intervalos de direcciones IP de usuario final a un punto de conexión específico. Al recibirse una solicitud, se devuelve el punto de conexión asignado a esa dirección IP de origen de la solicitud. 


Todos los perfiles de Traffic Manager tienen características de seguimiento de estado y conmutación automática por error de puntos de conexión. Para más información, consulte [Acerca de la supervisión de Traffic Manager](traffic-manager-monitoring.md). Dentro de un perfil de Traffic Manager, solo puede configurar un método de enrutamiento de tráfico a la vez. El método de enrutamiento de tráfico para el perfil se puede cambiar en cualquier momento. Los cambios se aplicarán cuando transcurra un minuto sin tiempo de inactividad. Los métodos de enrutamiento de tráfico se pueden combinar mediante perfiles anidados de Traffic Manager. El anidamiento de perfiles permite configuraciones de enrutamiento de tráfico sofisticadas que satisfacen las necesidades de aplicaciones más grandes y complejas. Para más información, consulte [Nested Traffic Manager profiles](traffic-manager-nested-profiles.md)(Perfiles anidados de Administrador de tráfico).

## <a name="priority-traffic-routing-method"></a>Método de enrutamiento del tráfico prioritario

En ocasiones, una organización desea proporcionar confiabilidad en sus servicios. Para ello, implementan uno o más servicios de copia de seguridad en caso de que el principal deje de funcionar. El método de enrutamiento de tráfico de "Prioridad" permite que los clientes de Azure implementen fácilmente este patrón de conmutación por error.

![Método de enrutamiento de tráfico de "prioridad" de Azure Traffic Manager](media/traffic-manager-routing-methods/priority.png)

El perfil de Traffic Manager contiene una lista de puntos de conexión de servicio ordenados por prioridad. De forma predeterminada, Traffic Manager envía todo el tráfico al punto de conexión principal (prioridad más alta). Si el punto de conexión principal no está disponible, Traffic Manager enruta el tráfico al segundo punto de conexión. Si los puntos de conexión principal y secundario no están disponibles, el tráfico pasa al tercero, y así sucesivamente. La disponibilidad del punto de conexión se basa en el estado configurado (habilitado o deshabilitado) y en la supervisión continuada del punto de conexión.

### <a name="configuring-endpoints"></a>Configuración de puntos de conexión

Con Azure Resource Manager, el usuario configura explícitamente la prioridad del punto de conexión mediante la propiedad de prioridad de cada punto de conexión. Esta propiedad es un valor comprendido entre 1 y 1000. Un valor inferior representa una prioridad más alta. Los puntos de conexión no pueden compartir valores de prioridad. La configuración de esta propiedad es opcional. Cuando se omite, se usa una prioridad predeterminada basada en el orden del punto de conexión.

## <a name="weighted-traffic-routing-method"></a><a name = "weighted"></a>Método de enrutamiento de tráfico Ponderado
El método de enrutamiento de tráfico "Ponderado" le permite distribuir el tráfico uniformemente o utilizar una ponderación predefinida.

![Método de enrutamiento de tráfico "ponderado" de Azure Traffic Manager](media/traffic-manager-routing-methods/weighted.png)

En el método de enrutamiento de tráfico ponderado, a cada punto de conexión se le asigna un peso en la configuración de perfiles de Traffic Manager. El peso es un entero comprendido entre 1 y 1000. Este parámetro es opcional. Si se omite, Traffic Manager usará un peso predeterminado de '1'. Cuanto mayor sea la ponderación, mayor será la prioridad.

Para cada consulta de DNS recibida, Traffic Manager elegirá aleatoriamente un punto de conexión disponible. La probabilidad de elegir un punto de conexión se basa en los pesos asignados a todos los puntos de conexión disponibles. Usar el mismo peso en todos los puntos de conexión dará como resultado una distribución de tráfico uniforme. El uso de pesos mayores o menores en puntos de conexión específicos hace que esos puntos de conexión se devuelvan con mayor o menor frecuencia en las respuestas de DNS.

El método ponderado permite algunos escenarios útiles:

* Actualización gradual de aplicaciones: asigne un porcentaje de tráfico para redirigirlo a un nuevo punto de conexión y aumentar gradualmente el tráfico hasta el 100 %.
* Migración de aplicaciones a Azure: cree un perfil con puntos de conexión de Azure y externos. Ajuste el peso de los puntos de conexión para dar preferencia a los nuevos puntos de conexión.
* Ampliación en la nube para aumentar la capacidad: expanda rápidamente una implementación local en la nube colocándola detrás de un perfil de Traffic Manager. Cuando necesite capacidad adicional en la nube, puede agregar o habilitar más extremos y especificar la porción de tráfico que va a cada extremo.

Puede configurar las ponderaciones mediante Azure Portal, Azure PowerShell, la CLI o las API REST.

Conviene recordar que los clientes almacenan en caché las respuestas DNS. También las almacenan en caché los servidores DNS recursivos que los clientes usan para resolver nombres DNS. Este almacenamiento en caché puede tener un impacto en las distribuciones de tráfico ponderadas. Cuando el número de clientes y servidores DNS recursivos es grande, la distribución del tráfico funciona según lo previsto. Sin embargo, si el número de clientes o servidores DNS recursivos es pequeño, el almacenamiento en caché puede sesgar considerablemente la distribución del tráfico.

Entre los casos de uso comunes se incluye:

* Entornos de desarrollo y pruebas
* Comunicaciones de aplicación a aplicación
* Las aplicaciones dirigidas a una base de usuarios estrecha que comparten una infraestructura DNS recursiva común (por ejemplo, los empleados de la empresa que se conectan a través de un proxy)

Estos efectos de almacenamiento en caché de DNS son comunes a todos los sistemas de enrutamiento de tráfico basados en DNS, no solo al Administrador de tráfico de Azure. En algunos casos, borrar explícitamente la caché DNS puede proporcionar una solución provisional. Si eso no funciona, puede resultar más adecuado el uso de un método de enrutamiento de tráfico alternativo.

## <a name="performance-traffic-routing-method"></a><a name = "performance"></a>Método de enrutamiento de tráfico Rendimiento

La implementación de puntos de conexión en dos o más ubicaciones del planeta puede mejorar la capacidad de respuesta de las aplicaciones. Con el método de enrutamiento de tráfico "Rendimiento", puede enrutar el tráfico a la ubicación más cercana.

![Método de enrutamiento de tráfico de "rendimiento" de Azure Traffic Manager](media/traffic-manager-routing-methods/performance.png)

El punto de conexión "más cercano" no es necesariamente el más cercano según la medición de la distancia geográfica. En vez de eso, el método de enrutamiento de tráfico de "rendimiento" determina cuál es el punto de conexión más cercano midiendo la latencia de red. Traffic Manager mantiene una tabla de latencia de Internet que realiza un seguimiento del tiempo que se invierte en la ida y la vuelta entre intervalos de direcciones IP y cada centro de datos de Azure.

Traffic Manager busca la dirección IP de origen de la solicitud de DNS entrante en la tabla de latencia de Internet. Traffic Manager elige entonces un punto de conexión disponible en el centro de datos de Azure que tiene la latencia más baja para ese intervalo de direcciones IP. A continuación, Traffic Manager devuelve ese punto de conexión en la respuesta DNS.

Según se explica en [Cómo funciona Traffic Manager](traffic-manager-how-it-works.md), Traffic Manager no recibe consultas de DNS directamente desde los clientes. En su lugar, las consultas de DNS proceden del servicio DNS recursivo que se ha configurado para que lo usen los clientes. Por lo tanto, la dirección IP usada para determinar el punto de conexión "más cercano" no es la dirección IP del cliente, sino la dirección IP de su servicio DNS recursivo. Esta dirección IP constituye un buen proxy para el cliente.


Para explicar los cambios en la red de Internet mundial y la incorporación de nuevas regiones de Azure, Traffic Manager actualiza regularmente la tabla de latencia de Internet. Sin embargo, el rendimiento de la aplicación varía en función de las variaciones en tiempo real de la carga en Internet. El enrutamiento de tráfico de rendimiento no supervisa la carga en un punto de conexión de servicio determinado. Si un punto de conexión no está disponible, Traffic Manager no lo incluirá en las respuestas a las consultas de DNS.


Puntos a tener en cuenta:

* Si el perfil contiene varios puntos de conexión en la misma región de Azure, Traffic Manager distribuirá el tráfico uniformemente entre los puntos de conexión disponibles en esa región. Si prefiere una distribución de tráfico diferente dentro de una región, puede usar los [perfiles anidados de Traffic Manager](traffic-manager-nested-profiles.md).
* Si se degradan todos los puntos de conexión habilitados en la región de Azure más cercana, Traffic Manager mueve el tráfico a los puntos de conexión en la siguiente región de Azure más cercana. Si desea definir una secuencia preferida de conmutación por error, use los [perfiles anidados de Traffic Manager](traffic-manager-nested-profiles.md).
* Al usar el método de enrutamiento de tráfico de rendimiento con puntos de conexión externos o anidados, deberá especificar la ubicación de esos puntos de conexión. Elija la región de Azure más cercana a su implementación. Estas ubicaciones son los valores compatibles con la tabla de latencia de Internet.
* El algoritmo que elige el punto de conexión es determinista. Las consultas de DNS repetidas del mismo cliente se dirigen al mismo punto de conexión. Normalmente, los clientes utilizan servidores DNS recursivos diferentes durante su recorrido. El cliente puede enrutarse a un punto de conexión diferente. El enrutamiento también puede verse afectado por las actualizaciones de la tabla de latencia de Internet. Por lo tanto, el método de enrutamiento de tráfico Rendimiento no garantiza que un cliente siempre se enrute al mismo punto de conexión.
* Cuando hay cambios en la tabla de latencia de Internet, puede observar que a algunos clientes se les dirige a un punto de conexión diferente. Este cambio de enrutamiento es más adecuado en función de los datos de latencia actuales. Estas actualizaciones son esenciales para mantener la precisión del enrutamiento de tráfico de rendimiento dado que Internet evoluciona constantemente.

## <a name="geographic-traffic-routing-method"></a><a name = "geographic"></a>Método de enrutamiento de tráfico Geográfico

Los perfiles de Traffic Manager se pueden configurar para usar el método de enrutamiento Geográfico para dirigir a los usuarios a puntos de conexión concretos (Azure, Externo o Anidado) en función de la ubicación geográfica de la que parta su consulta de DNS. Con este método de enrutamiento, podrá cumplir los requisitos de los mandatos de soberanía de datos, la localización del contenido y la experiencia del usuario, y la medición del tráfico de distintas regiones.
Cuando se configura un perfil para el enrutamiento geográfico, es preciso que cada punto de conexión asociado a dicho perfil tenga un conjunto de regiones geográficas asignado. Una región geográfica puede estar en los siguientes niveles de granularidad 
- Mundo: cualquier región
- Agrupación regional: por ejemplo, África, Oriente Medio, Australia/Pacífico, etc. 
- País o región: p. ej., Irlanda, Perú, Hong Kong (RAE), etc. 
- Estado o provincia: p. ej., Estados Unidos-California, Australia-Queensland, etc. (este nivel de granularidad solo se admite en estados o provincias de Australia, Canadá y Estados Unidos).

Si se asigna una región o un conjunto de regiones a un punto de conexión, todas las solicitudes de dichas regiones se enrutarán solo a dicho punto de conexión. Traffic Manager usa la dirección IP de origen de la consulta de DNS para identificar la región desde la que un realiza un usuario la consulta. Normalmente, se determina según la dirección IP de la resolución DNS local que realiza la consulta del usuario.  

![Método de enrutamiento del tráfico "geográfico" de Azure Traffic Manager](./media/traffic-manager-routing-methods/geographic.png)

Traffic Manager lee la dirección IP de origen de la consulta de DNS y decide la región geográfica de la que es originaria. Después, examina si hay algún punto de conexión que tiene dicha región geográfica asignada. Esta búsqueda se inicia en el nivel de granularidad más bajo (estado o provincia donde se admita o en el nivel de país o región si no se admite) y asciende hasta el nivel más alto, que es el **Mundo**. La primera coincidencia que se encuentre mediante este recorrido se designará como el punto de conexión de devolución en la respuesta de la consulta. En el caso de coincidencia con un punto de conexión de tipo Anidado, se devolverá un punto de conexión de dicho perfil secundario, en función de su método de enrutamiento. Los siguientes puntos se pueden aplicar a este comportamiento:

- Si se elige Enrutamiento geográfico, una región geográfica puede asignarse únicamente a un punto de conexión en un perfil de Traffic Manager. Esto garantiza que el enrutamiento de los usuarios es determinista y que los clientes pueden habilitar escenarios que requieran límites geográficos inequívocos.
- Si la región de un usuario se muestra en la asignación geográfica de dos puntos de conexión diferentes, Traffic Manager selecciona el punto de conexión con la granularidad más baja. Traffic Manager no enrutará las solicitudes de esa región al otro punto de conexión. Ejemplo: considere un perfil del tipo enrutamiento Geográfico con dos puntos de conexión, Punto de conexión 1 y Punto de conexión 2. Punto de conexión 1 está configurado para recibir tráfico de Irlanda, mientras que Punto de conexión 2 está configurado para recibir tráfico de Europa. Si una solicitud parte de Irlanda, siempre se enrutará al punto de conexión 1.
- Puesto que una región se puede asignar solo a un punto de conexión, Traffic Manager devolverá una respuesta, independientemente de que el punto de conexión sea correcto o no.

    >[!IMPORTANT]
    >Se recomienda encarecidamente que los clientes que puedan usar el método de enrutamiento geográfico lo asocien con puntos de conexión del tipo Anidado que tengan perfiles secundarios que contengan al menos dos puntos de conexión cada uno.
- Si se encuentra un punto de conexión coincidente que esté en estado **Detenido**, Traffic Manager devolverá una respuesta NODATA. En este caso, no se realizarán más búsquedas en la parte superior de la jerarquía de la región geográfica. Este comportamiento también se puede aplicar a los tipos de punto de conexión anidados cuando el perfil secundario está en los estados **Detenido** o **Deshabilitado**.
- Si un punto de conexión se encuentra en el estado **Deshabilitado**, no se incluirá en el proceso de coincidencia de región. Este comportamiento también se puede aplicar a los tipos de punto de conexión anidados cuando el punto de conexión se encuentra en estado **Deshabilitado**.
- Si una consulta procede de una región geográfica que no tiene ninguna asignación en dicho perfil, Traffic Manager devolverá una respuesta NODATA. Esta es la razón por la que se recomienda encarecidamente que use el enrutamiento geográfico con un punto de conexión. Idealmente, de tipo Anidado con al menos dos puntos de conexión en el perfil secundario, con la región **Mundo** asignada. Esta configuración también garantiza que se controlen todas las direcciones IP que no estén asignadas a una región.

Según se explica en [Cómo funciona Traffic Manager](traffic-manager-how-it-works.md), Traffic Manager no recibe consultas de DNS directamente desde los clientes. Las consultas de DNS proceden del servicio DNS recursivo que se ha configurado para que lo usen los clientes. Por tanto, la dirección IP usada para determinar la región no es la del cliente, sino la de su servicio DNS recursivo. Esta dirección IP constituye un buen proxy para el cliente.

### <a name="faqs"></a>Preguntas más frecuentes

* [¿En qué casos de uso el enrutamiento geográfico resulta útil?](./traffic-manager-faqs.md#what-are-some-use-cases-where-geographic-routing-is-useful)

* [¿Cómo decido si debo usar el método de enrutamiento por rendimiento o el método de enrutamiento geográfico?](./traffic-manager-faqs.md#how-do-i-decide-if-i-should-use-performance-routing-method-or-geographic-routing-method)

* [¿Cuáles son las regiones que son compatibles con Traffic Manager para el enrutamiento geográfica?](./traffic-manager-faqs.md#what-are-the-regions-that-are-supported-by-traffic-manager-for-geographic-routing)

* [¿Cómo determina Traffic Manager desde dónde consulta un usuario?](./traffic-manager-faqs.md#how-does-traffic-manager-determine-where-a-user-is-querying-from)

* [¿Está garantizado que Traffic Manager puede determinar correctamente la ubicación geográfica exacta del usuario en cada caso?](./traffic-manager-faqs.md#is-it-guaranteed-that-traffic-manager-can-correctly-determine-the-exact-geographic-location-of-the-user-in-every-case)

* [¿Un punto de conexión debe encontrarse físicamente en la misma región que la configurada para el enrutamiento geográfico?](./traffic-manager-faqs.md#does-an-endpoint-need-to-be-physically-located-in-the-same-region-as-the-one-it-is-configured-with-for-geographic-routing)

* [¿Se pueden asignar regiones geográficas a puntos de conexión en un perfil que no está configurado para el enrutamiento geográfico?](./traffic-manager-faqs.md#can-i-assign-geographic-regions-to-endpoints-in-a-profile-that-is-not-configured-to-do-geographic-routing)

* [¿Por qué cuando intento cambiar el método de enrutamiento de un perfil existente a geográfico obtengo un error?](./traffic-manager-faqs.md#why-am-i-getting-an-error-when-i-try-to-change-the-routing-method-of-an-existing-profile-to-geographic)

* [¿Por qué se recomienda encarecidamente que los clientes creen perfiles anidados en lugar de puntos de conexión en un perfil con el enrutamiento geográfico habilitado?](./traffic-manager-faqs.md#why-is-it-strongly-recommended-that-customers-create-nested-profiles-instead-of-endpoints-under-a-profile-with-geographic-routing-enabled)

* [¿Existen restricciones en la versión de API que admite este tipo de enrutamiento?](./traffic-manager-faqs.md#are-there-any-restrictions-on-the-api-version-that-supports-this-routing-type)

## <a name="multivalue-traffic-routing-method"></a><a name = "multivalue"></a>Método de enrutamiento de tráfico de varios valores
El método de enrutamiento de tráfico **Multivalor** permite obtener varios puntos de conexión correctos en una única respuesta de consulta de DNS. Esta configuración permite al autor de la llamada hacer reintentos en el lado cliente con otros puntos de conexión en el caso de que un punto de conexión devuelto no responda. Este patrón puede aumentar la disponibilidad de un servicio y reducir la latencia asociada a una nueva consulta de DNS para obtener un punto de conexión correcto. El método de enrutamiento Multivalor solo funciona si todos los puntos de conexión del tipo "Externo" están especificados como direcciones IPv4 o IPv6. Al recibirse una consulta relacionada con este perfil, se devuelven todos los puntos de conexión correctos y están sujetos a un número máximo de devoluciones configurado.

### <a name="faqs"></a>Preguntas más frecuentes

* [¿En qué casos de uso el enrutamiento multivalor resulta útil?](./traffic-manager-faqs.md#what-are-some-use-cases-where-multivalue-routing-is-useful)

* [¿Cuántos puntos de conexión se devuelven cuando se usa el enrutamiento de varios valores?](./traffic-manager-faqs.md#how-many-endpoints-are-returned-when-multivalue-routing-is-used)

* [¿Obtendré el mismo conjunto de puntos de conexión cuando se use el enrutamiento de varios valores?](./traffic-manager-faqs.md#will-i-get-the-same-set-of-endpoints-when-multivalue-routing-is-used)

## <a name="subnet-traffic-routing-method"></a><a name = "subnet"></a>Método de enrutamiento de tráfico de subred
El método de enrutamiento de tráfico **Subred** permite asignar un conjunto de intervalos de direcciones IP de usuario final a puntos de conexión específicos en un perfil. Si Traffic Manager recibe una consulta de DNS para ese perfil, inspeccionará la dirección IP de origen de esa solicitud. A continuación, determinará el punto de conexión al que está asignado y devolverá ese punto de conexión en la respuesta de la consulta. En la mayoría de los casos, la dirección IP de origen es la resolución DNS que usa el autor de la llamada.

La dirección IP que debe asignarse a un punto de conexión puede especificarse como intervalos CIDR (por ejemplo, 1.2.3.0/24) o como un intervalo de direcciones (por ejemplo, 1.2.3.4-5.6.7.8). Los intervalos IP asociados a un punto de conexión deben ser únicos dentro de ese perfil. El intervalo de direcciones no puede superponerse con el conjunto de direcciones IP de un punto de conexión diferente en el mismo perfil.
Si define un punto de conexión sin intervalo de direcciones, este funcionará como una reserva y tomará el tráfico de las subredes restantes. Si no se incluye ningún punto de conexión de reserva, Traffic Manager envía una respuesta NODATA para cualquier intervalo indefinido. Se recomienda encarecidamente definir un punto de conexión de reserva para asegurarse de que todos los intervalos IP posibles se especifiquen en los puntos de conexión.

El enrutamiento de subredes se puede utilizar para ofrecer una experiencia diferente a los usuarios que se conectan desde un espacio IP específico. Por ejemplo, puede hacer que todas las solicitudes de su oficina corporativa se enruten a un punto de conexión diferente. Este método de enrutamiento es especialmente útil si está intentando probar una versión de la aplicación de forma interna. Otro escenario es si desea proporcionar una experiencia diferente a los usuarios que se conectan desde un ISP específico (por ejemplo, bloquear a los usuarios de un ISP determinado).

### <a name="faqs"></a>Preguntas más frecuentes

* [¿En qué casos de uso el enrutamiento de subredes resulta útil?](./traffic-manager-faqs.md#what-are-some-use-cases-where-subnet-routing-is-useful)

* [¿Cómo sabe Traffic Manager la dirección IP del usuario final?](./traffic-manager-faqs.md#how-does-traffic-manager-know-the-ip-address-of-the-end-user)

* [¿Cómo se pueden especificar direcciones IP al usar el enrutamiento de subredes?](./traffic-manager-faqs.md#how-can-i-specify-ip-addresses-when-using-subnet-routing)

* [¿Cómo se puede especificar un punto de conexión de reserva al usar el enrutamiento de subredes?](./traffic-manager-faqs.md#how-can-i-specify-a-fallback-endpoint-when-using-subnet-routing)

* [¿Qué ocurre si se deshabilita un punto de conexión en un perfil de tipo de enrutamiento de subredes?](./traffic-manager-faqs.md#what-happens-if-an-endpoint-is-disabled-in-a-subnet-routing-type-profile)


## <a name="next-steps"></a>Pasos siguientes

Aprenda a desarrollar aplicaciones de alta disponibilidad mediante la [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md)