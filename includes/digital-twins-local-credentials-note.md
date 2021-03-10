---
author: baanders
description: 'archivo de inclusión para DefaultAzureCredential en ejemplos de Azure Digital Twins: nota'
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: bec2681c33108417aab1a33932c4f5f4d2ed730f
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102473726"
---
>[!NOTE]
> En este ejemplo se usa [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential) (parte de la biblioteca `Azure.Identity`) para autenticar a los usuarios en la instancia de Azure Digital Twins cuando la ejecuta en la máquina local. Con este tipo de autenticación, el ejemplo buscará las credenciales de Azure en el entorno local, como un inicio de sesión de una instancia local de la [CLI de Azure](/cli/azure/install-azure-cli) o en Visual Studio y Visual Studio Code.
>
> Para más información sobre el uso de `DefaultAzureCredential` y otras opciones de autenticación, consulte [*Procedimiento: Escritura de código de autenticación de aplicación*](../articles/digital-twins/how-to-authenticate-client.md).