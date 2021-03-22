---
title: 'Inicio rápido: Creación de un perfil de Azure Front Door Estándar/Premium: Azure Portal'
description: En este inicio rápido se muestra cómo usar Azure Front Door Service Estándar/Premium para una aplicación web global de alta disponibilidad y alto rendimiento mediante Azure Portal.
services: frontdoor
author: duongau
manager: KumudD
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2021
ms.author: duau
ms.openlocfilehash: 18956948a32a79c1435bf4fc1554b09480c9010c
ms.sourcegitcommit: 97c48e630ec22edc12a0f8e4e592d1676323d7b0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2021
ms.locfileid: "101100758"
---
# <a name="quickstart-create-an-azure-front-door-standardpremium-profile---azure-portal"></a>Inicio rápido: Creación de un perfil de Azure Front Door Estándar/Premium: Azure Portal

> [!Note]
> Esta documentación trata sobre Azure Front Door Estándar/Prémium (versión preliminar). ¿Busca información sobre Azure Front Door? Vea la [documentación sobre Azure Front Door](../front-door-overview.md).

En este inicio rápido, aprenderá a crear un perfil de Azure Front Door Estándar/Premium mediante Azure Portal. El perfil de Azure Front Door Estándar/Premium se puede crear mediante *Creación rápida* con configuraciones básicas o mediante *Creación personalizada* con configuraciones más avanzadas. Con *Creación personalizada*, se implementan dos instancias de Web Apps. Después, se crea el perfil de Azure Front Door Estándar/Premium con las dos instancias de Web Apps como origen. Luego, se comprueba la conectividad con Web Apps mediante el nombre de host de front-end de Azure Front Door Estándar/Premium.

> [!IMPORTANT]
> Azure Front Door Estándar/Prémium (versión preliminar) está disponible actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

Una cuenta de Azure con una suscripción activa. [Cree una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-front-door-profile---quick-create"></a>Creación de un perfil de Front Door: Creación rápida

1. Inicie sesión en [Azure Portal](https://portal.azure.com).

1. En la página principal o en el menú de Azure Portal, seleccione  **+Crear un recurso**. Busque  *Front Door Standard/Premium (Preview)* [Front Door Estándar/Premium (versión preliminar)] Seleccione **Crear**.

1. En la página **Compare offerings** (Comparar ofertas), seleccione **Quick create** (Creación rápida). Luego, seleccione **Continue to create a Front Door** (Continuar para crear instancia de Front Door).

   :::image type="content" source="../media/create-front-door-portal/front-door-quick-create.png" alt-text="Captura de pantalla de comparación de ofertas.":::

1. En la página **Create a front door profile** (Crear un perfil de Front Door), escriba o seleccione la siguiente configuración.

    :::image type="content" source="../media/create-front-door-portal/front-door-quick-create-2.png" alt-text="Captura de pantalla de la página de creación rápida de un perfil de Front Door.":::    

    | Configuración | Value |
    | --- | --- |
    | **Suscripción**  | Seleccione su suscripción. |
    | **Grupos de recursos**  | Seleccione **Crear** y escriba *contoso-appservice* en el cuadro de texto.|
    | **Nombre** | Asigne un nombre al perfil. En este ejemplo se usa **contoso-afd-quickcreate**. |
    | **Nivel** | Seleccione la SKU Estándar o Premium. La SKU Estándar está optimizada para la entrega de contenido. La SKU Premium se basa en la SKU Estándar y se centra en la seguridad. Consulte el artículo en que se [comparan los distintos niveles](tier-comparison.md).  |
    | **Nombre del extremo** | Escriba un nombre único global para el punto de conexión. |
    | **Tipo de origen** | Seleccione el tipo de recurso del origen. En este ejemplo, se selecciona como origen una instancia de App Service que tiene Private Link habilitado. |
    | **Nombre de host del origen** | Escriba el nombre de host del origen. |
    | **Habilitación de Private Link** | Si desea tener una conexión privada entre Azure Front Door y su origen. Para más información, consulte la [guía de Private Link](concept-private-link.md) y el artículo sobre la [habilitación de Private Link](how-to-enable-private-link.md).
    | **Almacenamiento en caché** | Active la casilla si desea almacenar en caché el contenido más cerca de los usuarios de forma global mediante los POP perimetrales de Azure Front Door y la red de Microsoft. |
    | **Directiva de WAF** | Si desea habilitar esta característica, seleccione **Crear** o seleccione una directiva de WAF existente en la lista desplegable. |

    > [!NOTE]
    > Al crear un perfil de Azure Front Door Estándar/Premium, debe seleccionar un origen que se encuentre en la misma suscripción en la que se crea Front Door.

1. Seleccione **Revisar y crear** para que el perfil de Front Door empiece a funcionar.

   > [!NOTE]
   > Las configuraciones pueden tardar unos minutos en propagarse a todos los POP perimetrales.

1. Luego, haga clic en **Crear** para que el perfil de Front Door se implemente y se ejecute.

1. Si ha habilitado Private Link, vaya a su origen (en este ejemplo es App Service). Seleccione **Redes** > **Configurar Private Link**. Luego, seleccione la solicitud pendiente de Azure Front Door y haga clic en Aprobar. Después de unos segundos, se podrá acceder a la aplicación a través de Azure Front Door de forma segura.

## <a name="create-front-door-profile---custom-create"></a>Creación de un perfil de Front Door: Creación personalizada

### <a name="create-a-web-app-with-two-instances-as-the-origin"></a>Creación de una aplicación web con dos instancias como origen

Si ya tiene un origen o un grupo de orígenes configurados, vaya a Creación de un perfil de Front Door Estándar/Premium (versión preliminar) para su aplicación.

En este ejemplo, se crea una aplicación web con dos instancias que se ejecutan en diferentes regiones de Azure. Ambas instancias de la aplicación web se ejecutan en modo *activo/activo*, por lo que cualquiera de ellas puede asumir el tráfico. Esta configuración difiere de una configuración *activa/en espera*, en la que una realiza la conmutación por error.

Si aún no tiene una aplicación web, siga los pasos que se indicarán a continuación para configurar una aplicación web de ejemplo.

1. Inicie sesión en Azure Portal en https://portal.azure.com.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **WebApp**.

1. En la pestaña **Datos básicos** de la página **Crear aplicación web**, escriba o seleccione la información siguiente.

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | **Suscripción**               | Seleccione su suscripción. |
    | **Grupos de recursos**       | Seleccione **Crear nuevo** y escriba *FrontDoorQS_rg1* en el cuadro de texto.|
    | **Nombre**                   | Escriba un **Nombre** único para la aplicación web. En este ejemplo se usa *WebAppContoso-001*. |
    | **Publicar** | Seleccione **Código**. |
    | **Pila del entorno en tiempo de ejecución**         | Seleccione **.NET Core 2.1 (LTS)** . |
    | **Sistema operativo**          | Seleccione **Windows**. |
    | **Región**           | Seleccione **Centro de EE. UU.** |
    | **Plan de Windows** | Seleccione **Crear nuevo** y escriba *myAppServicePlanCentralUS* en el cuadro de texto. |
    | **SKU y tamaño** | Seleccione **Standard S1 100 total ACU, 1.75-GB memory** (Estándar S1 100 ACU total, 1,75 GB de memoria). |

     :::image type="content" source="../media/create-front-door-portal/create-web-app.png" alt-text="Creación rápida de SKU de Front Door Premium en Azure Portal":::

1. Seleccione **Review + create** (Revisar y crear), revise el resumen y, después, seleccione **Create** (Crear). La implementación puede tardar varios minutos.

Una vez finalizada la implementación, cree una segunda aplicación web. Use la misma configuración que antes, salvo en los siguientes valores:

| Configuración          | Value     |
| ---              | ---  |
| **Grupos de recursos**   | Seleccione **Crear** y escriba *FrontDoorQS_rg2*. |
| **Nombre**             | Escriba un nombre único para la aplicación web, en este ejemplo, *WebAppContoso-002*.  |
| **Región**           | Una región distinta, en este ejemplo, *Centro-sur de EE. UU.* |
| **Plan de App Service** > **Plan de Windows**         | Seleccione **Nuevo**, escriba *myAppServicePlanSouthCentralUS* y seleccione **Aceptar**. |

### <a name="create-a-front-door-standardpremium-preview-for-your-application"></a>Creación de un perfil de Front Door Estándar/Premium (versión preliminar) para su aplicación

Configure Azure Front Door Estándar/Premium (versión preliminar) para dirigir el tráfico del usuario en función de la latencia más baja entre los dos servidores de las aplicaciones web. Proteja también Front Door con Web Application Firewall 

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
 
1. En la página principal o en el menú de Azure Portal, seleccione  **+Crear un recurso**. Busque  *Front Door Standard/Premium (Preview)* [Front Door Estándar/Premium (versión preliminar)] Seleccione **Crear**.

1. En la página **Compare offerings** (Comparar ofertas), seleccione **Custom create** (Creación personalizada). Luego, seleccione **Continue to create a Front Door** (Continuar para crear instancia de Front Door).

1. En la pestaña  **Datos básicos** , escriba o seleccione la información siguiente y, después, seleccione  **Siguiente: Secreto**. 

    | Configuración | Value |
    | --- | --- |
    | **Suscripción** | Seleccione su suscripción. |
    | **Grupos de recursos** | Seleccione **Crear nuevo** y escriba *FrontDoorQS_rg0* en el cuadro de texto. |
    | **Ubicación del grupo de recursos** | Seleccione **Este de EE. UU**. |
    | **Nombre del perfil** | Escriba un nombre único en esta suscripción **Webapp-Contoso-AFD** |
    | **Nivel** | Seleccione **Premium**. |
    
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-2.png" alt-text="Creación de un perfil de Front Door":::

1. *Opcional*: **Secretos**. Si planea usar certificados administrados, este paso es opcional. Si tiene una instancia de Key Vault existente en Azure y planea usar la opción Bring Your Own Certificate para traer su dominio personalizado, seleccione **Agregar un certificado**. También puede agregar un certificado en la experiencia de administración después de la creación.

    > [!NOTE]
    > Debe tener el permiso adecuado para agregar el certificado desde Azure Key Vault como usuario. 

    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-secret.png" alt-text="Captura de pantalla en la que se agrega un secreto en una creación personalizada.":::

1. En la pestaña **Punto de conexión**, seleccione **Agregar un punto de conexión** y asigne un nombre único global al punto de conexión. En el perfil de Azure Front Door Estándar/Premium se pueden crear varios puntos de conexión después de finalizar la experiencia de creación. En este ejemplo se usa *contoso-frontend*. Deje los valores predeterminados en Origin response timeout (in seconds) [Tiempo de espera de respuesta de origen (en segundos)] y Status (Estado). Seleccione **Agregar** para agregar el punto de conexión.
    
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-endpoint.png" alt-text="Captura de pantalla de la página para agregar un punto de conexión.":::

1. A continuación, agregue un grupo de orígenes que contenga sus dos aplicaciones web. Seleccione **+ Agregar**   para abrir la página **Add an origin group** (Agregar un grupo de orígenes). En Nombre, escriba *myOrignGroup* y, después, seleccione **+ Agregar un origen**.
 
     :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-group.png" alt-text="Captura de pantalla de la página para agregar un grupo de orígenes.":::

1. En la página **Add an origin** (Agregar un origen)  , escriba o seleccione la información siguiente. A continuación, seleccione **Agregar**.

    | Configuración | Value |
    | --- | --- |
    | **Nombre** | Escriba **webapp1** |
    | **Tipo de origen** | Seleccione **App Services** |
    | **Nombre de host** | Seleccione `WebAppContoso-001.azurewebsites.net` |
    | **Encabezado del host de origen** | Seleccione `WebAppContoso-001.azurewebsites.net` |
    | **Otros campos** | Deje todos los demás campos con sus valores predeterminados. |

    > [!NOTE]
    > Al crear un perfil de Azure Front Door Estándar/Premium, debe seleccionar un origen que se encuentre en la misma suscripción en la que se crea Azure Front Door Estándar/Premium.
   
   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-1.png" alt-text="Captura de pantalla de la página de agregar más orígenes.":::

1. Repita el paso 8 para agregar el segundo origen, webapp002. Seleccione `webappcontoso-002.azurewebsite.net` como **nombre de host de origen** y **encabezado de host de origen**.

1. En la página **Add an origin group** (Agregar un grupo de orígenes), verá que se han agregado dos orígenes; deje los valores predeterminados en los restantes campos.
  
   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-origin-group-2.png" alt-text="Captura de pantalla de la página para agregar un grupo de orígenes.":::

1. Luego, agregue una ruta para asignar el punto de conexión de front-end al grupo de orígenes. Esta ruta reenvía las solicitudes del punto de conexión a myOriginGroup. Seleccione **+ Agregar** en Ruta para configurar una ruta.  

1. En la página **Agregar una ruta**, escriba o seleccione la información siguiente. A continuación, seleccione **Agregar**.
  
    :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-route-without-caching.png" alt-text="Agregar una ruta sin almacenamiento en caché":::

    | Configuración | Value |
    | --- | --- |
    | **Nombre** | Escriba **MyRoute** |    
    | **Dominio** | Seleccione `contoso-frontend.z01.azurefd.net` | 
    | **Nombre de host** | Seleccione `WebAppContoso-001.azurewebsites.net` |
    | **Patrones de coincidencia** | Deje el valor predeterminado. |
    | **Protocolos aceptados** | Deje el valor predeterminado. | 
    | **Redirect** | Deje la opción predeterminada en **Redirect all traffic to use HTTPS** (Redirigir todo el tráfico para que use HTTPS). | 
    | **Grupo de orígenes** | Seleccione **MyOriginGroup**. | 
    | **Ruta de acceso de origen** | Deje el valor predeterminado. | 
    | **Protocolo de reenvío** | Seleccione **Match incoming request** (Coincidir con solicitud entrante). | 
    | **Almacenamiento en caché** | Déjela desactivada en este inicio rápido. Si desea que el contenido se almacene en caché en los bordes, active la casilla **Habilitar almacenamiento en caché**. |
    | **Reglas** | Deje el valor predeterminado. Una vez que haya creado el perfil de Front Door, puede crear reglas personalizadas y aplicarlas a las rutas. |  
       
    >[!WARNING]
    > **Asegúrese** de que hay una ruta para cada punto de conexión. La ausencia de una ruta puede provocar un error en un punto de conexión.

1. Luego, seleccione **+ Agregar** en Seguridad para agregar una directiva de WAF. Seleccione **Agregar** Nuevo y asigne un nombre único a la directiva. Active la casilla **Add bot protection** (Agregar protección contra bots). Seleccione el punto de conexión en **Dominios** y, después, seleccione **Agregar**.

   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-add-waf-policy-2.png" alt-text="Agregar directiva de WAF":::

1. Seleccione **Revisar y crear** y, después,  **Crear**. Las configuraciones tardan unos minutos en propagarse a todos los POP perimetrales. Ya tiene el primer perfil y punto de conexión de Front Door.

   :::image type="content" source="../media/create-front-door-portal/front-door-custom-create-review.png" alt-text="Revisar creación personalizada":::

## <a name="verify-azure-front-door"></a>Comprobación de Azure Front Door

Cuando se crea el perfil de Azure Front Door Estándar/Premium, la configuración tarda unos minutos en implementarse globalmente. Cuando haya finalizado, puede acceder al host de front-end que ha creado. Vaya a `contoso-frontend.z01.azurefd.net` en un explorador. La solicitud se enrutará automáticamente al servidor más cercano de entre los servidores especificados en el grupo de orígenes.

Si ha creado estas aplicaciones en este inicio rápido, verá una página de información.

Para probar la conmutación por error global instantánea, siga estos pasos:

1. Abra un explorador como se indica más arriba y vaya a la dirección de front-end: `contoso-frontend.azurefd.net`.

1. En Azure Portal, busque y seleccione *App Services*. Desplácese hacia abajo hasta encontrar una de sus aplicaciones web, **WebAppContoso-001** en este ejemplo.

1. Seleccione la aplicación web y, a continuación, seleccione **Detener** y **Sí** para confirmar.

1. Actualice el explorador. Debería aparecer la misma página de información.

   >[!TIP]
   >Hay algo de retraso en estas acciones. Puede que tenga que actualizar de nuevo.

1. Busque la otra aplicación web y deténgala también.

1. Actualice el explorador. Esta vez debería aparecer un mensaje de error.

    :::image type="content" source="../media/create-front-door-portal/web-app-stopped-message.png" alt-text="Las dos instancias de la aplicación web detenidas":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Una vez que haya terminado, puede eliminar todos los elementos que haya creado. La eliminación de un grupo de recursos también elimina su contenido. Si no tiene pensado utilizar esta instancia de Front Door, debe eliminar los recursos para evitar cargos innecesarios.

1. En Azure Portal, busque y seleccione **Grupos de recursos** o seleccione **Grupos de recursos** desde el menú de Azure Portal.

1. Filtre o desplácese hacia abajo hasta encontrar un grupo de recursos, por ejemplo **FrontDoorQS_rg0**.

1. Seleccione el grupo de recursos y, después, seleccione **Eliminar grupo de recursos**.

   >[!WARNING]
   >Esta acción es irreversible.

1. Escriba el nombre del grupo de recursos para confirmar y, a continuación, seleccione **Eliminar**.

Repita el procedimiento anterior con los otros dos grupos.

## <a name="next-steps"></a>Pasos siguientes

Pase al siguiente artículo para aprender a agregar un dominio personalizado a Front Door.
> [!div class="nextstepaction"]
> [Agregar un dominio personalizado](how-to-add-custom-domain.md)
