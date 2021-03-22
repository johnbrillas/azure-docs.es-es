---
title: 'Tutorial: Creación de una directiva de firewall de aplicaciones web para Azure Front Door: Azure Portal'
description: En este tutorial aprenderá a crear una directiva de firewall de aplicaciones web (WAF) mediante Azure Portal.
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: tutorial
ms.date: 02/18/2021
ms.author: victorh
ms.openlocfilehash: 8b1d1007e817bafe3d75f0f0d7c3fc6eb5470854
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101729477"
---
# <a name="tutorial-create-a-web-application-firewall-policy-on-azure-front-door-using-the-azure-portal"></a>Tutorial: Creación de una directiva de firewall de aplicaciones web en Azure Front Door mediante Azure Portal

En este tutorial se muestra cómo crear una directiva de Azure Web Application Firewall (WAF) básica y aplicarla a un host de front-end en Azure Front Door.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una directiva WAF
> * Asociarla a un host de front-end
> * Configurar las reglas de WAF

## <a name="prerequisites"></a>Prerrequisitos

Para crear un perfil de Front Door siga las instrucciones que se describen en [Inicio rápido: Creación de un perfil de Front Door](../../frontdoor/quickstart-create-front-door.md). 

## <a name="create-a-web-application-firewall-policy"></a>Creación de una directiva de firewall de aplicaciones web

En primer lugar, en el portal, cree una directiva WAF con un conjunto de reglas predeterminado (DRS) administrado. 

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso**> busque **WAF**> seleccione **Firewall de aplicaciones Web (versión preliminar)** > seleccione  **Crear**.
2. En la pestaña **Datos básicos** de la página **Crear una directiva WAF**, escriba o seleccione la siguiente información, acepte los valores predeterminados para el resto de la configuración y, luego, seleccione **Revisar y crear**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Subscription            |Seleccione el nombre de la suscripción a Front Door.|
    | Resource group          |Seleccione el nombre del grupo de recursos de Front Door.|
    | Nombre de la directiva             |Escriba un nombre único para la directiva WAF.|

   :::image type="content" source="../media/waf-front-door-create-portal/basic.png" alt-text="Captura de pantalla de la página Crear una directiva de W A F, con el botón Revisar y crear, y cuadros de lista para la suscripción, el grupo de recursos y el nombre de la directiva." border="false":::

3. En la pestaña **Asociación** de la página **Crear una directiva WAF**, seleccione **Agregar un host de front-end**, escriba la siguiente configuración y, a continuación, seleccione **Agregar**:

    | Configuración                 | Value                                              |
    | ---                     | ---                                                |
    | Front Door              | Seleccione el nombre de su perfil de Front Door.|
    | Host de front-end           | Seleccione el nombre de su host de Front Door y luego seleccione **Agregar**.|
    
    > [!NOTE]
    > Si el host de front-end está asociado a una directiva WAF, se muestra como atenuado. Debe quitar primero el host de front-end de la directiva asociada y, a continuación, volver a asociarlo a una nueva directiva WAF.
1. Seleccione **Revisar y crear** y, luego, **Crear**.

## <a name="configure-web-application-firewall-rules-optional"></a>Configuración de las reglas del firewall de aplicaciones web (opcional)

### <a name="change-mode"></a>Cambio del modo

Cuando se crea una directiva WAF, de forma predeterminada, esta está en modo **Detección**. En el modo **Detección**, WAF no bloquea las solicitudes, sino que las solicitudes que coinciden con las reglas de WAF se registran en los registros de WAF.
Para ver WAF en acción, puede cambiar la configuración del modo de **Detección** a **Prevención**. En el modo **Prevención**, las solicitudes que coinciden con las reglas que se definen en el conjunto de reglas predeterminado (DRS) se bloquean y se registran en los registros de WAF.

 :::image type="content" source="../media/waf-front-door-create-portal/policy.png" alt-text="Captura de pantalla de la sección de configuración de la directiva. El conmutador Modo está establecido en Prevención." border="false":::

### <a name="custom-rules"></a>Reglas personalizadas

Puede crear una regla personalizada si selecciona **Agregar regla personalizada** bajo la sección **Reglas personalizadas**. De esta forma se inicia la página de configuración de reglas personalizadas. A continuación se presenta un ejemplo de cómo configurar una regla personalizada para bloquear una solicitud si la cadena de consulta contiene **blockme**.

:::image type="content" source="../media/waf-front-door-create-portal/customquerystring2.png" alt-text="Captura de pantalla de la página de configuración de regla personalizada que muestra la configuración de una regla que comprueba si la variable QueryString contiene el valor blockme." border="false":::

### <a name="default-rule-set-drs"></a>Conjunto de reglas predeterminado (DRS)

El conjunto de reglas predeterminado administrado por Azure está habilitado de forma predeterminada. La versión predeterminada actual es DefaultRuleSet_1.0. En WAF, en **Reglas administradas**, **Asignar**, el conjunto de reglas Microsoft_DefaultRuleSet_1.1 está disponible en la lista desplegable.

Para deshabilitar una regla individual dentro de un grupo de reglas, expanda las reglas dentro de ese grupo de reglas, active la **casilla** delante del número de regla y seleccione **Deshabilitar** en la pestaña anterior. Para cambiar los tipos de acciones para los conjuntos de reglas individuales dentro del conjunto de reglas, active la casilla situada delante del número de regla y, a continuación, seleccione la pestaña **Cambiar acción** anterior.

 :::image type="content" source="../media/waf-front-door-create-portal/managed2.png" alt-text="Captura de pantalla de la página Reglas administradas que muestra un conjunto de reglas, grupos de reglas, reglas y los botones Habilitar, Deshabilitar y Cambiar acción. Esta marcada una regla." border="false":::

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine el grupo de recursos y todos los recursos relacionados.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre Azure Front Door](../../frontdoor/front-door-overview.md).
