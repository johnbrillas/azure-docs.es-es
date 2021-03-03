---
title: 'Conector de Administración de servicios de TI: Exportación segura en Azure Monitor'
description: En este artículo se muestra cómo conectar los productos o servicios de ITSM con Exportación seguridad en Azure Monitor para supervisar y administrar de manera centralizada los elementos de trabajo de ITSM.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 09/08/2020
ms.openlocfilehash: 6b273b864c8329547ef91eb7063a064e0f5cb77d
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "100604716"
---
# <a name="connect-azure-to-itsm-tools-by-using-secure-export"></a>Conexión de Azure a las herramientas de ITSM mediante Exportación segura

En este artículo se muestra cómo configurar la conexión entre un producto o servicio de la Administración de servicios de TI (ITSM) mediante Exportación segura.

Exportación segura es una versión actualizada del [Conector de Administración de servicios de TI (ITSMC)](../platform/itsmc-overview.md). Ambas versiones le permiten crear elementos de trabajo en una herramienta de ITSM cuando Azure Monitor envía alertas. La funcionalidad incluye alertas de métricas, de registros y de registros de actividad.

ITSMC usa las credenciales de nombre de usuario y contraseña. Exportación segura tiene una autenticación más sólida porque usa Azure Active Directory (Azure AD). Azure AD es un servicio de administración de acceso y de identidades basado en la nube de Microsoft. Ayuda a los usuarios a iniciar sesión y acceder a los recursos internos o externos. Usar Azure AD con ITSM ayuda a identificar las alertas de Azure (a través del id. de aplicación de Azure AD) que se enviaron al sistema externo.

> [!NOTE]
> La capacidad de conectar Azure a las herramientas de ITSM mediante Exportación segura está en versión preliminar.

## <a name="secure-export-architecture"></a>Arquitectura de Exportación segura

La arquitectura de Exportación segura presenta estas funcionalidades nuevas:

* **Nuevo grupo de acciones**: las alertas se envían a la herramienta de ITSM a través del grupo de acciones Webhook seguro en lugar del grupo de acciones ITSM que ITSMC usa.
* **Autenticación de Azure AD**: la autenticación se realiza a través de Azure AD en lugar de las credenciales de nombre de usuario y contraseña.

## <a name="secure-export-data-flow"></a>Flujo de datos de Exportación segura

Los pasos del flujo de datos de Exportación segura son los siguientes:

1. Azure Monitor envía una alerta que está configurada para usar Exportación segura.
2. Una acción de webhook seguro envía la carga de la alerta a la herramienta de ITSM.
3. La aplicación de ITSM comprueba con Azure AD si la alerta está autorizada para entrar a la herramienta de ITSM.
4. Si la alerta está autorizada, la aplicación:
   
   1. Crea un elemento de trabajo (por ejemplo, un incidente) en la herramienta de ITSM.
   2. Enlaza el id. del elemento de configuración (CI) a la base de datos de administración de clientes (CMDB).

![Diagrama que muestra cómo la herramienta de ITSM se comunica con Azure AD, las alertas de Azure y un grupo de acciones.](media/it-service-management-connector-secure-webhook-connections/secure-export-diagram.png)

## <a name="benefits-of-secure-export"></a>Ventajas de Exportación segura

Las principales ventajas de la integración son las siguientes:

* **Mejor autenticación**: Azure AD proporciona una autenticación más segura sin los tiempos de espera que suelen producirse en ITSMC.
* **Alertas resueltas en la herramienta de ITSM**: las alertas de métricas implementan los estados "activado" y "resuelto". Cuando se cumple la condición, el estado de la alerta es "activado". Cuando ya no se cumple la condición, el estado de la alerta es "resuelto". En ITSMC, las alertas no se pueden resolver de manera automática. Con Exportación segura, el estado resuelto fluye a la herramienta de ITSM, por lo que se actualiza automáticamente.
* **[Esquema de alerta común](./alerts-common-schema.md)** : en ITSMC, el esquema de la carga de la alerta es distinto en función del tipo de alerta. En Exportación segura, hay un esquema común para todos los tipos de alerta. Este esquema común contiene el elemento de configuración para todos los tipos de alerta. Todos los tipos de alerta podrán enlazar su elemento de configuración con la CMDB.

## <a name="next-steps"></a>Pasos siguientes

* [Creación de elementos de trabajo de ITSM a partir de alertas de Azure](../platform/itsmc-overview.md)
