---
title: Configuración de un punto de conexión de Azure Front Door Standard o Premium con Endpoint Manager
description: En este artículo, se explica cómo se configura un punto de conexión con Endpoint Manager.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: 241f4a61e8d8c8de7a5573e8de534cb927a71b30
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101098163"
---
# <a name="configure-an-azure-front-door-standardpremium-preview-endpoint-with-endpoint-manager"></a>Configuración de un punto de conexión de Azure Front Door Standard o Premium (versión preliminar) con Endpoint Manager

> [!NOTE]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Vea la **[documentación sobre Azure Front Door](../front-door-overview.md)** .

En este artículo, aprenderá a crear un punto de conexión para un perfil existente de Azure Front Door Standard o Premium con Endpoint Manager.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

Para poder crear un punto de conexión de Azure Front Door Standard o Premium con Endpoint Manager, debe haber creado al menos un perfil en Azure Front-Door. El perfil debe tener como mínimo uno o varios puntos de conexión de Azure Front Door Standard o Premium. Puede utilizar varios perfiles para organizar los puntos de conexión de Azure Front Door Standard o Premium en función del dominio de Internet, la aplicación web u otros criterios. 

Para crear un perfil de Azure Front Door, consulte este artículo sobre la [creación de un nuevo perfil de Azure Front Door Standard o Premium](create-front-door-portal.md).

## <a name="create-a-new-azure-front-door-standardpremium-endpoint"></a>Creación de un nuevo punto de conexión de Azure Front Door Standard o Premium

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a su perfil de Azure Front Door Estándar/Prémium.

1. Seleccione **Endpoint Manager**. Después, seleccione **Agregar un punto de conexión** para crear un nuevo punto de conexión.
   
    :::image type="content" source="../media/how-to-configure-endpoint-manager/select-create-endpoint.png" alt-text="Instantánea de &quot;Agregar un punto de conexión&quot; de Endpoint Manager.":::

1. En la página **Agregar un punto de conexión**, especifique y seleccione las siguientes opciones.
    
    :::image type="content" source="../media/how-to-configure-endpoint-manager/create-endpoint-page.png" alt-text="Instantánea de &quot;Agregar un punto de conexión&quot;.":::

    | Configuración | Value |
    | -------- | ----- |
    | Nombre | Especifique un nombre único para el nuevo punto de conexión de Azure Front Door Standard o Premium. Este nombre se va a utilizar para acceder a los recursos almacenados en la memoria caché del dominio `<endpointname>.az01.azurefd.net`. |
    | Origin Response timeout (secs) (Tiempo de espera de respuesta de origen [segundos]) | Especifique en segundos el tiempo que Azure Front Door tiene que esperar antes de considerar que la conexión con el origen ha superado el tiempo de espera. |
    | Status | Active la casilla para habilitar este punto de conexión. |

## <a name="add-domains-origin-group-routes-and-security"></a>Incorporación de dominios, grupos de orígenes, rutas y seguridad

1. Seleccione **Editar extremo** en el punto de conexión para configurar la ruta.

1. En la página **Editar extremo**, seleccione **+ Agregar** en Dominios.
   
    :::image type="content" source="../media/how-to-configure-endpoint-manager/select-add-domain.png" alt-text="Instantánea del dominio seleccionado en la página &quot;Editar extremo&quot;.":::

### <a name="add-domain"></a>Incorporación del dominio

1. En la página **Agregar dominio**, elija *From your Azure Front Door profile* (Desde el perfil de Azure Front Door) si desea agregar el dominio desde el perfil de Azure Front Door o *Agregar un dominio nuevo* si prefiere agregar un nuevo dominio. Para más información acerca de cómo crear un dominio completamente nuevo, consulte este artículo sobre la [creación de un nuevo dominio personalizado en Azure Front Door Standard o Premium](how-to-add-custom-domain.md).

    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-domain-page.png" alt-text="Instantánea de la página &quot;Agregar un dominio&quot;.":::

1. Seleccione **Agregar** para agregar el dominio al punto de conexión actual. El dominio seleccionado debe aparecer en el panel Dominio.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/domain-in-domainview.png" alt-text="Instantánea de los dominios que aparecen en la vista Dominio.":::

### <a name="add-origin-group"></a>Incorporación de un grupo de orígenes

1. Seleccione **Agregar** en la vista de grupos de orígenes. Aparecerá la página **Add an origin group** (Agregar un grupo de orígenes). 

    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-origin-group-view.png" alt-text="Instantánea de la página para agregar un grupo de orígenes":::

1. En **Nombre**, especifique un nombre único para el nuevo grupo de orígenes.

1. Seleccione **Add an Origin** (Agregar un origen) para agregar un nuevo origen al grupo actual.
 
#### <a name="health-probes"></a>Sondeos de estado
Front Door envía periódicamente solicitudes de sondeo HTTP/HTTPS a cada uno de los orígenes. Las solicitudes de sondeo determinan la proximidad y el estado de cada origen para equilibrar la carga de las solicitudes del usuario final. La configuración del sondeo de estado de un grupo de orígenes determinará cómo se va a sondear el estado del origen de aplicaciones. Las siguientes opciones están disponibles para la configuración del equilibrio de carga:

> [!WARNING]
> Como Front Door tiene muchos entornos perimetrales en todo el mundo, el volumen de solicitudes para sondear el estado del origen puede ser bastante elevado, desde 25 solicitudes por minuto hasta un máximo de 1200 solicitudes por minuto, dependiendo de la frecuencia de sondeo de estado configurada. Con la frecuencia de sondeo predeterminada de 30 segundos, el volumen de sondeos del origen sería de unas 200 solicitudes por minuto.

* **Estado**: especifique si desea activar el sondeo de estado. Si tiene un único origen en el grupo de orígenes, puede deshabilitar los sondeos de estado y reducir la carga en el back-end de la aplicación. Aunque haya varios orígenes en el grupo y solo uno de ellos tenga el estado habilitado, puede deshabilitar los sondeos de estado.
   
* **Ruta de acceso**: dirección URL que se utiliza con las solicitudes de sondeo de todo el origen de este grupo. Por ejemplo, si uno de los orígenes es contoso-westus.azurewebsites.net y la ruta de acceso se establece en /probe/test.aspx, los entornos de Front Door enviarán las solicitudes de sondeo de estado a `http://contoso-westus.azurewebsites.net/probe/test.aspx`, suponiendo que el protocolo esté establecido en HTTP. 
   
* **Protocolo**: define si las solicitudes de sondeo de estado se van a enviar al origen desde Front Door utilizando el protocolo HTTP o HTTPS.
   
* **Método**: método HTTP que se va a utilizar para enviar sondeos de estado. Las opciones incluyen GET o HEAD (valor predeterminado).

    > [!NOTE]
    > Para reducir la carga y el costo del origen, Front Door recomienda utilizar solicitudes HEAD con los sondeos de estado.

* **Intervalo (en segundos)** : determina la frecuencia de los sondeos de estado del origen o los intervalos que utiliza cada entorno de Front Door para enviar un sondeo.
   
    >[!NOTE]
    >Para las conmutaciones por error rápidas, establezca el intervalo en un valor inferior. Cuanto menor sea el valor, mayor será el volumen de sondeos de estado que recibe el origen. Por ejemplo, si el intervalo se establece en 30 segundos con, por ejemplo, 100 POP de Front Door globalmente, cada back-end recibirá en torno a 200 solicitudes de sondeo por minuto.

#### <a name="load-balancing"></a>Equilibrio de carga
La configuración del equilibrio de carga del grupo de orígenes determina cómo se van a evaluar los sondeos de estado. Esta configuración determina si el back-end es correcto o incorrecto. También establece cómo se va a equilibrar la carga del tráfico entre los diferentes orígenes del grupo. Las siguientes opciones están disponibles para la configuración del equilibrio de carga:

- **Tamaño de muestra**. Identifica cuántas muestras de los sondeos de estado se deben tener en cuenta para evaluar el estado del origen.

- **Tamaño de muestra correcto**. Define el tamaño que debe tener la muestra, tal y como se indicó anteriormente; es decir, el número de muestras correctas necesario para determinar que el estado del origen es correcto. Por ejemplo, suponga que un intervalo de sondeo de estado de Front Door es de 30 segundos, el tamaño de muestra es 5 y el tamaño de muestra correcto es 3. Cada vez que evaluamos los sondeos de estado del origen, analizamos las cinco últimas muestras en un período de 150 segundos (5 x 30). Se requieren al menos tres sondeos correctos para declarar el back-end como correcto.

- **Latency sensitivity (extra latency)** (Sensibilidad a la latencia [latencia adicional]). Determina si Front Door va a enviar la solicitud al origen dentro del intervalo de sensibilidad de medición de latencia o va a reenviar la solicitud al back-end más cercano.

Seleccione **Agregar** para agregar el grupo de orígenes al punto de conexión actual. El grupo de orígenes debe aparecer en el panel del grupo de orígenes.

:::image type="content" source="../media/how-to-configure-endpoint-manager/origin-in-origin-group.png" alt-text="Instantánea de los orígenes de un grupo.":::

### <a name="add-route"></a>Incorporación de una ruta

Seleccione **Agregar** en la vista de rutas para que aparezca la página **Agregar una ruta**. Para más información acerca de cómo asociar el dominio y el grupo de orígenes, consulte este artículo sobre la [creación de una nueva ruta de Azure Front Door](how-to-configure-route.md).

### <a name="add-security"></a>Incorporación de medidas de seguridad

1. Seleccione **Agregar** en la vista de seguridad para que aparezca la página **Add a WAF policy** (Agregar una directiva WAF).
 
    :::image type="content" source="../media/how-to-configure-endpoint-manager/add-waf-policy-page.png" alt-text="Instantánea de la página para agregar una directiva WAF.":::

1. **Directiva WAF**: seleccione la directiva WAF que quiera aplicar en el dominio seleccionado dentro de este punto de conexión. 
  
   Seleccione **Crear nuevo** para crear una directiva WAF completamente nueva.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/create-new-waf-policy.png" alt-text="Instantánea de la página para crear una nueva directiva WAF.":::
   
    **Nombre**: especifique un nombre único para la nueva directiva WAF. Puede editar esta directiva con más opciones de configuración desde la página de Web Application Firewall.

    **Dominios**: seleccione el dominio que desea aplicar a la directiva WAF.

1. Seleccione el botón **Agregar**. La directiva WAF debería aparecer en el panel de seguridad.

    :::image type="content" source="../media/how-to-configure-endpoint-manager/waf-in-security-view.png" alt-text="Instantánea de la directiva WAF en la vista de seguridad.":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Para eliminar un punto de conexión cuando ya no es necesario, seleccione la opción **Eliminar punto de conexión** que encontrará al final de la fila de dicho punto de conexión. 

:::image type="content" source="../media/how-to-configure-endpoint-manager/delete-endpoint.png" alt-text="Instantánea de la página para eliminar un punto de conexión.":::

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los recursos personalizados, consulte este artículo sobre la [incorporación de dominios personalizados](how-to-add-custom-domain.md).
