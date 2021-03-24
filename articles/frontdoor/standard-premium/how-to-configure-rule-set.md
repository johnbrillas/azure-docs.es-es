---
title: 'Azure Front Door: configuración del conjunto de reglas de Front Door'
description: En este artículo se proporcionan instrucciones sobre cómo configurar un conjunto de reglas.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 6863c492059ccee152ecf3d03a09e61793576bcb
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101715605"
---
# <a name="configure-a-rule-set-with-azure-front-door-standardpremium-preview"></a>Configuración de un conjunto de reglas con Azure Front Door Estándar/Prémium (versión preliminar)

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Consulte [aquí](../front-door-overview.md).

En este artículo se muestra cómo crear un conjunto de reglas y su primer conjunto de reglas en Azure Portal. A continuación, aprenderá a asociar el conjunto de reglas a una ruta desde la página del conjunto de reglas o desde Endpoint Manager.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Requisitos previos

* Para poder configurar un conjunto de reglas, primero debe crear una instancia de Azure Front Door Estándar/Premium. Para más información, consulte el artículo de [inicio rápido sobre cómo crear un perfil de Azure Front Door Estándar/Prémium](create-front-door-portal.md).

## <a name="configure-rule-set-in-azure-portal"></a>Configuración de un conjunto de reglas en Azure Portal

1. En el perfil de Front Door, seleccione **Conjunto de reglas** en **Configuración**. Seleccione **Agregar** y asígnele un nombre de conjunto de reglas.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-create-rule-set-1.png" alt-text="Captura de pantalla de la página de aterrizaje del conjunto de reglas.":::
    
1. Seleccione **Agregar regla** para crear la primera regla. Asígnele un nombre de regla. Luego, seleccione **Agregar condición** o **Agregar acción** para definir la regla. Puede agregar hasta 10 condiciones y 5 acciones para una regla. En este ejemplo, usamos la variable de servidor para agregar un encabezado de respuesta de país geográfico para las solicitudes que incluyen *contoso* en la dirección URL.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-create-rule-set.png" alt-text="Captura de pantalla de la página de configuración del conjunto de reglas.":::
    
    > [!NOTE]
    > * Para eliminar una condición o acción de la regla, use la papelera que aparece en el lado derecho de la condición o acción específica.
    > * Para crear una regla que se aplique a todo el tráfico entrante, no especifique ninguna condición.
    > * Para detener la evaluación de las reglas restantes si se cumple una regla específica, active **Detener la evaluación de las reglas restantes**. Si esta opción está activada, no se ejecutarán las reglas restantes del conjunto de reglas, independientemente de que se cumplan las condiciones de coincidencia.  

1. Puede determinar la prioridad de las reglas dentro de su conjunto de reglas utilizando los botones de flecha para subir o bajar la prioridad de las reglas. La lista está en orden ascendente, por lo que la regla más importante aparece en primer lugar.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-rule-set-change-orders.png" alt-text="Captura de pantalla de la prioridad del conjunto de reglas." lightbox="../media/how-to-configure-rule-set/front-door-rule-set-change-orders-expanded.png":::

1. Una vez que haya creado una o varias reglas, seleccione **Guardar** para completar la creación del conjunto de reglas.

1. Ahora, asocie el conjunto de reglas a una ruta para que pueda surtir efecto. Puede asociar el conjunto de reglas a través de la página del conjunto de reglas o puede ir a Endpoint Manager para crear la asociación.
 
    **Página del conjunto de reglas**: 
    
    1. Seleccione el conjunto de reglas que se va a asociar.
    
    1. Seleccione el vínculo *Sin asociar*.
     

    1. A continuación, en la página **Asociar una ruta**, seleccione el punto de conexión y la ruta que desea asociar al conjunto de reglas. 
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set.png" alt-text="Captura de pantalla de la página para crear una ruta.":::    
        
    1. Seleccione *Siguiente* para cambiar los órdenes del conjunto de reglas si hay varios conjuntos de reglas en la ruta seleccionada. El conjunto de reglas se ejecutará de arriba abajo. Puede cambiar los órdenes seleccionando el conjunto de reglas y moviéndolo hacia arriba o hacia abajo. Luego, seleccione *Asociar*.
    
        > [!Note]
        > Solo puede asociar un conjunto de reglas a una sola ruta en esta página. Para asociar un conjunto de reglas con varias rutas, use Endpoint Manager.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-2.png" alt-text="Captura de pantalla de los órdenes del conjunto de reglas.":::
    
    1. El conjunto de reglas ya está asociado a una ruta. Puede fijarse el encabezado de respuesta y ver que se ha agregado el país geográfico.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-3.png" alt-text="Captura de pantalla de la regla asociada a una ruta.":::

   **Endpoint Manager**: 
    
    1. Vaya a Endpoint Manager y seleccione el punto de conexión que desea asociar al conjunto de reglas.
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-1.png" alt-text="Captura de pantalla de selección del punto de conexión en Endpoint Manager." lightbox="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-1-expanded.png":::

    1. Seleccione *Editar punto de conexión*.  
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-2.png" alt-text="Captura de pantalla de selección del punto de conexión en Endpoint Manager." lightbox="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-2-expanded.png":::

    1. Seleccione la ruta. 
    
         :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-3.png" alt-text="Captura de pantalla de la selección de una ruta":::.
    
    1. En la página *Actualizar ruta*, en *Reglas*, seleccione los conjuntos de reglas que desea asociar a la ruta de la lista desplegable. A continuación, puede cambiar los órdenes subiendo y bajando el conjunto de reglas. 
    
        :::image type="content" source="../media/how-to-configure-rule-set/front-door-associate-rule-set-endpoint-manager-4.png" alt-text="Captura de pantalla de la página para actualizar una ruta.":::
    
    1. A continuación, seleccione *Actualizar* o *Agregar* para finalizar la asociación.

## <a name="delete-a-rule-set-from-your-azure-front-door-profile"></a>Eliminación de un conjunto de reglas del perfil de Azure Front Door

En los pasos anteriores, ha configurado un conjunto de reglas y lo ha asociado a la ruta. Si ya no desea que el conjunto de reglas esté asociado a su instancia de Front Door, puede eliminar el conjunto de reglas siguiendo estos pasos:

1. Vaya a la **página del conjunto de reglas** en **Configuración** para desasociar el conjunto de reglas de todas las rutas asociadas.

1. Expanda la ruta y seleccione los tres puntos. A continuación, seleccione *Editar ruta*.

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-1.png" alt-text="Captura de pantalla de la ruta expandida en el conjunto de reglas.":::

1. Vaya a la sección de reglas en la página de ruta, seleccione el conjunto de reglas y seleccione en el botón *Eliminar*. 

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-2.png" alt-text="Captura de pantalla de la página de actualización de ruta para eliminar un conjunto de reglas." lightbox="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-2-expanded.png":::

1. Seleccione *Actualizar* y el conjunto de reglas se desasociará de la ruta.

1. Repita los pasos 2-5 para desasociar otras rutas asociadas a este conjunto de reglas hasta que vea que el estado de las rutas es *Sin asociar*.

1. Para el conjunto de reglas que está *Sin asociar*, puede eliminar el conjunto de reglas haciendo clic en los tres puntos de la derecha y seleccionando *Eliminar*. 

   :::image type="content" source="../media/how-to-configure-rule-set/front-door-disassociate-rule-set-3.png" alt-text="Captura de pantalla de cómo eliminar un conjunto de reglas.":::

1. El conjunto de reglas se ha eliminado.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo agregar [encabezados de seguridad con un conjunto de reglas](how-to-add-security-headers.md).
