---
title: Trabajo con directivas de seguridad | Microsoft Docs
description: En este artículo se describe cómo trabajar con directivas de seguridad en Azure Security Center.
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2021
ms.author: memildin
ms.openlocfilehash: 6ecedc20cf6924a82b6b4640d3caa75bc5958de0
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102101331"
---
# <a name="manage-security-policies"></a>Administrar directivas de seguridad

En este artículo se explica cómo configurar directivas de seguridad y cómo visualizarlas en Security Center. 

## <a name="who-can-edit-security-policies"></a>¿Quién puede editar directivas de seguridad?

Puede editar directivas de seguridad mediante el portal de Azure Policy, la API REST o con Windows PowerShell.

Security Center usa el control de acceso basado en roles de Azure (Azure RBAC), que proporciona roles integrados que puede asignar a usuarios, grupos y servicios de Azure. Cuando un usuario abre Security Center, solo ve la información relacionada con los recursos a los que puede tener acceso. Esto significa que a los usuarios se les asigna el rol de *propietario*, *colaborador* o *lector* para la suscripción del recurso. También hay dos roles de Security Center específicos:

- **Lector de seguridad**: tiene derechos para ver elementos de Security Center como recomendaciones, alertas, directiva y estado. No se pueden realizar cambios.
- **Administrador de seguridad**: tiene los mismos derechos de visualización que el *lector de seguridad*. También se puede actualizar la directiva de seguridad y descartar alertas.

## <a name="manage-your-security-policies"></a>Administración de las directivas de seguridad

Para ver las directivas de seguridad de Security Center:

1. En el panel de **Security Center**, seleccione **Directiva de seguridad**.

    :::image type="content" source="./media/security-center-policies/security-center-policy-mgt.png" alt-text="Página Administración de directivas":::

   En la pantalla **Administración de directivas**, puede ver el número de grupos de administración, suscripciones y áreas de trabajo, así como la estructura del grupo de administración.

1. Seleccione el grupo de administración o la suscripción cuyas directivas desea ver.

1. Aparecerá la página de directiva de seguridad de la suscripción o el grupo de administración. Esta muestra las directivas disponibles y asignadas.

    :::image type="content" source="./media/tutorial-security-policy/security-policy-page.png" alt-text="Página de directiva de seguridad de Security Center" lightbox="./media/tutorial-security-policy/security-policy-page.png":::

    > [!NOTE]
    > Si hay una etiqueta "MG Inherited" junto a la directiva predeterminada, significa que la directiva se ha asignado a un grupo de administración y que la ha heredado la suscripción que está viendo.

1. Elija entre las opciones disponibles en esta página:

    1. Para usar estándares del sector, seleccione **Agregar más estándares**. Para más información, consulte [Personalización del conjunto de estándares en el panel de cumplimiento normativo](update-regulatory-compliance-packages.md).

    1. Para asignar y administrar iniciativas personalizadas, seleccione **Agregar iniciativas personalizadas**. Para obtener más información, consulte [Uso de iniciativas y directivas de seguridad personalizadas](custom-security-policies.md).

    1. Para ver y editar la iniciativa predeterminada, seleccione **Ver directiva efectiva** y continúe tal como se describe a continuación. 

        :::image type="content" source="./media/security-center-policies/policy-screen.png" alt-text="Pantalla Directiva efectiva":::

       La pantalla de **directiva de seguridad** refleja las acciones que realizaron las directivas asignadas en el grupo de administración o la suscripción que seleccionó.
       
       * En la parte superior, use los vínculos proporcionados para abrir cada **asignación** de directiva que se aplique al grupo de administración o la suscripción. Puede usar esos vínculos para obtener acceso a la asignación y editar o deshabilitar la directiva. Por ejemplo, si ve que una asignación de directiva determinada está denegando la protección de puntos de conexión, puede usar el vínculo para editar o deshabilitar la directiva.
       
       * En la lista de directivas, puede ver la aplicación efectiva de la directiva en su suscripción o grupo de administración. Esto significa que se tiene en cuenta la configuración de cada directiva que se aplica al ámbito y se proporciona el resultado acumulado de la acción que realiza esa directiva. Por ejemplo, si en una asignación la directiva está deshabilitada, pero en otra está definida en AuditIfNotExist, el efecto acumulado aplica AuditIfNotExist. El efecto más activo siempre tiene prioridad.
       
       * El efecto de las directivas puede ser: Append, Audit, AuditIfNotExists, Deny, DeployIfNotExists, Disabled. Para más información sobre cómo se aplican los efectos, consulte [Descripción de los efectos de Policy](../governance/policy/concepts/effects.md).

       > [!NOTE]
       > Cuando vea directivas asignadas, puede ver varias asignaciones y cómo se configura cada asignación individualmente.


## <a name="disable-security-policies-and-disable-recommendations"></a>Deshabilitar las directivas de seguridad y recomendaciones

Si su iniciativa de seguridad desencadena una recomendación que no es relevante para su entorno, puede evitar que esa recomendación vuelva a aparecer. Para deshabilitar una recomendación, deshabilite la directiva específica que la genera.

La recomendación que desea deshabilitar seguirá apareciendo si es necesaria para un cumplimiento normativo aplicado con las herramientas de cumplimiento normativo de Security Center. Incluso si ha deshabilitado una directiva en la iniciativa integrada, una directiva en la iniciativa del cumplimiento normativo seguirá desencadenando la recomendación si hace falta para el cumplimiento. No puede deshabilitar directivas de iniciativas de cumplimiento normativo.

Para más información sobre las recomendaciones, consulte [Administración de las recomendaciones de seguridad](security-center-recommendations.md).

1. En Security Center, en la sección **Directiva y cumplimiento**, seleccione **Directiva de seguridad**.

    :::image type="content" source="./media/tutorial-security-policy/policy-management.png" alt-text="Inicio del proceso de administración de directivas en Azure Security Center":::

2. Seleccione el grupo de administración o de suscripción para el que quiere deshabilitar la recomendación.

   > [!NOTE]
   > Recuerde que un grupo de administración aplica sus directivas a sus suscripciones. Por lo tanto, si deshabilita la directiva de una suscripción y la suscripción pertenece a un grupo de administración que todavía usa la misma directiva, seguirá recibiendo las recomendaciones de la directiva. La directiva se seguirá aplicando desde el nivel de administración y las recomendaciones se seguirán generando.

1. Seleccione **Ver directiva efectiva**.

    :::image type="content" source="./media/tutorial-security-policy/view-effective-policy.png" alt-text="Apertura de la directiva efectiva asignada a su suscripción":::

1. Seleccione la directiva asignada.

   ![selección de directiva](./media/tutorial-security-policy/security-policy.png)

1. En la sección **PARÁMETROS**, busque la directiva que invoca la recomendación que quiere deshabilitar y, en la lista desplegable, seleccione **Deshabilitado**

   ![deshabilitar la directiva](./media/tutorial-security-policy/disable-policy.png)

1. Seleccione **Guardar**.

   > [!NOTE]
   > Los cambios de deshabilitación de directiva pueden tardar hasta 12 horas en surtir efecto.

## <a name="next-steps"></a>Pasos siguientes
En esta página se explicaron las directivas de seguridad. Para obtener información relacionada, consulte las páginas siguientes:

- [Obtenga información sobre cómo establecer directivas con PowerShell](../governance/policy/assign-policy-powershell.md).
- [Obtenga información sobre cómo editar una directiva de seguridad en Azure Policy](../governance/policy/tutorials/create-and-manage.md).
- [Obtenga información sobre cómo establecer una directiva en varias suscripciones o grupos de administración con Azure Policy](../governance/policy/overview.md).
- [Obtenga información sobre cómo habilitar Security Center en todas las suscripciones de un grupo de administración](onboard-management-group.md).