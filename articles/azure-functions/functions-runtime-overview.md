---
title: Introducción al Sistema en ejecución de Azure Functions
description: Introducción al Sistema en ejecución de Azure Functions (versión preliminar)
author: apwestgarth
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 61e3b82e497afcdc8239a9f4fda3e4f739166a1f
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "92108446"
---
# <a name="azure-functions-runtime-overview-preview"></a>Introducción a Azure Functions Runtime (versión preliminar)

[!INCLUDE [intro](../../includes/functions-runtime-preview-note.md)]

Azure Functions Runtime (versión preliminar) proporciona una forma nueva de aprovechar la simplicidad y flexibilidad del modelo de programación de Azure Functions de forma local. Basado en las mismas raíces de código abierto que Azure Functions, el Sistema en ejecución de Azure Functions se implementa de forma local para proporcionar una experiencia de desarrollo prácticamente idéntica al servicio en la nube.

![Portal del Sistema en ejecución de Azure Functions (versión preliminar)][1]

El Sistema en ejecución de Azure Functions proporciona una forma de experimentar Azure Functions antes de pasarse a la nube. De esta manera, se puede llevar a la nube los recursos de código que cree cuando migre.  El sistema en tiempo de ejecución también le ofrece nuevas opciones, como el uso de la capacidad de proceso adicional de sus equipos locales para ejecutar procesos por lotes durante la noche. También puede usar dispositivos dentro de su organización para enviar datos de forma condicional a otros sistemas, tanto locales como en la nube.

El Sistema en ejecución de Azure Functions consta de dos partes:

* Rol de administración del Sistema en ejecución de Azure Functions
* Rol de trabajo del Sistema en ejecución de Azure Functions

## <a name="azure-functions-management-role"></a>Rol de administración de Azure Functions

El rol de administración de Azure Functions proporciona un host para la administración de Functions de forma local. Este rol realiza las siguientes tareas:

* Hospedaje del Portal de administración de Azure Functions, que es el mismo que se ve en [Azure Portal](https://portal.azure.com). Este portal ofrece una experiencia coherente que permite desarrollar las funciones de la misma manera que lo haría en Azure Portal.
* Distribución de funciones entre varios trabajadores de Functions.
* Proporciona un punto de conexión de publicación para que pueda publicar sus funciones directamente desde Microsoft Visual Studio mediante la descarga e importación del perfil de publicación.

## <a name="azure-functions-worker-role"></a>Rol de trabajo de Azure Functions

Los roles de trabajo de Azure Functions se implementan en los contenedores de Windows y son donde se ejecuta el código de la función.  Puede implementar varios roles de trabajo en toda la organización; esta opción es una manera clave en que los clientes pueden hacer uso del exceso de capacidad de proceso.  Un ejemplo de exceso de proceso en muchas organizaciones son las maquinas que están constantemente encendidas y no se usan durante períodos de tiempo prolongados.

## <a name="minimum-requirements"></a>Requisitos mínimos

Para empezar a trabajar con el sistema de tiempo de ejecución de Azure Functions, debe tener una máquina con Windows Server 2016 o Windows 10 Creators Update y acceso a una instancia de SQL Server.

## <a name="next-steps"></a>Pasos siguientes

Instale la [versión preliminar del Sistema en ejecución de Azure Functions](./functions-runtime-install.md)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png