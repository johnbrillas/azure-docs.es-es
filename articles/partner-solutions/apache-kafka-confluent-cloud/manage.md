---
title: 'Administración de una instancia de Confluent Cloud: soluciones de partners de Azure'
description: En este artículo se describe la administración de una instancia de Confluent Cloud en Azure Portal. Cómo configurar el inicio de sesión único, eliminar una organización de Confluent y obtener soporte técnico.
ms.service: partner-services
ms.topic: conceptual
ms.date: 01/15/2021
author: tfitzmac
ms.author: tomfitz
ms.openlocfilehash: 2d13c183f0b3891fa92b5e2a6534acbf8102e032
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98253277"
---
# <a name="manage-the-confluent-cloud-resource"></a>Administración del recurso de Confluent Cloud

En este artículo se describe cómo administrar la instancia de Apache Kafka para Confluent Cloud en Azure. Muestra cómo configurar el inicio de sesión único (SSO), eliminar una organización de Confluent y crear una solicitud de soporte técnico.

## <a name="single-sign-on"></a>Inicio de sesión único

Para implementar el inicio de sesión único para su organización, el administrador de inquilinos puede importar la aplicación de la galería. Este paso es opcional. Para obtener información sobre cómo importar una aplicación, consulte [Inicio rápido: Incorporación de una aplicación al inquilino de Azure Active Directory (Azure AD)](../../active-directory/manage-apps/add-application-portal.md) Cuando el administrador de inquilinos importa la aplicación, los usuarios no tienen que dar su consentimiento explícito para permitir el acceso al portal de Confluent Cloud.

Para habilitar el inicio de sesión único, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Vaya a la **Información general** de la instancia del recurso de Confluent Cloud.
1. Seleccione el vínculo para **administrar en Confluent Cloud**.

   :::image type="content" source="media/sso-link.png" alt-text="Inicio de sesión único del portal de Confluent.":::

1. Si el administrador de inquilinos no ha importado la aplicación de la galería para el consentimiento del SSO, conceda los permisos y el consentimiento. Este paso solo es necesario la primera vez que se tiene acceso al vínculo para **administrar en Confluent Cloud**.
1. Elija una cuenta de Azure AD para el inicio de sesión único en el portal de Confluent Cloud.
1. Una vez proporcionado el consentimiento, se le redirigirá al portal de Confluent Cloud.

## <a name="set-up-cluster"></a>Configuración del clúster

Para obtener información acerca de cómo configurar el clúster, consulte [Creación de un clúster en Confluent Cloud: documentación de Confluent](https://docs.confluent.io/cloud/current/clusters/create-cluster.html).

## <a name="delete-confluent-organization"></a>Eliminación de organización de Confluent

Cuando ya no necesite el recurso de Confluent Cloud, elimine el recurso en Azure y Confluent Cloud.

Para eliminar los recursos en Azure:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **Todos los recursos** y **filtre por el nombre** del recurso de Confluent Cloud o el **tipo de recurso** _Organización de Confluent_. O bien, en Azure Portal, busque el nombre del recurso.
1. En la **Información general** del recurso, seleccione **Eliminar**.

    :::image type="content" source="media/delete-resources-icon.png" alt-text="Icono de eliminación de recursos.":::

1. Para confirmar la eliminación, escriba el nombre del recurso y seleccione **Eliminar**.

    :::image type="content" source="media/delete-resources-prompt.png" alt-text="Solicitud de confirmación de eliminación de recursos.":::

Para eliminar el recurso en Confluent Cloud, consulte la documentación de [Entornos de Confluent Cloud: documentación de Confluent](https://docs.confluent.io/current/cloud/using/environments.html) y [Aspectos básicos de Confluent Cloud: documentación de Confluent](https://docs.confluent.io/current/cloud/using/cloud-basics.html).

El clúster y todos los datos del clúster se eliminan de forma permanente. Si el contrato incluye una cláusula de retención de datos, Confluent mantiene los datos durante el período de tiempo especificado en los [términos del servicio de la documentación de Confluent](https://www.confluent.io/confluent-cloud-tos).

Se le facturará el uso prorrateado hasta el momento de la eliminación del clúster. Una vez que el clúster se elimina permanentemente, Confluent le envía una confirmación por correo electrónico.

## <a name="get-support"></a>Obtener soporte técnico

Para enviar una solicitud de soporte técnico a Confluent, póngase en contacto con el [soporte técnico de Confluent](https://support.confluent.io) o envíe una solicitud a través del portal, como se muestra a continuación.

> [!NOTE]
> Para los nuevos usuarios, restablezca la contraseña antes de iniciar sesión en el portal de soporte técnico de Confluent. Si no dispone de una cuenta con Confluent Cloud, envíe un correo electrónico a `cloud-support@confluent.io` para obtener más ayuda.

Para enviar una solicitud desde el recurso, siga estos pasos:

1. En Azure Portal, seleccione la organización de Confluent.
1. En el menú del lado izquierdo de la pantalla, seleccione **Nueva solicitud de soporte técnico**.
1. Para crear una solicitud de soporte técnico, seleccione el vínculo al **portal de Confluent**.

    :::image type="content" source="media/support-request.png" alt-text="Creación de una solicitud de soporte técnico.":::

## <a name="next-steps"></a>Pasos siguientes

Para obtener ayuda con la solución de problemas, consulte [Solución de problemas de las soluciones de Apache Kafka para Confluent Cloud](troubleshoot.md).
