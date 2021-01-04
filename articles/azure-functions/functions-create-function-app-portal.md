---
title: Creación de una instancia de Function App desde Azure Portal
description: Cree una nueva aplicación de funciones en Azure desde el portal.
ms.topic: how-to
ms.date: 08/29/2019
ms.custom: mvc
ms.openlocfilehash: 8d19a269903de309bf219c2546fa70c3abe7be10
ms.sourcegitcommit: 5db975ced62cd095be587d99da01949222fc69a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97093597"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Creación de una instancia de Function App desde Azure Portal

En este tema se muestra cómo usar Azure Functions para crear una aplicación de funciones en Azure Portal. Una instancia de Function App es el contenedor que hospeda la ejecución de funciones individuales. 

## <a name="create-a-function-app"></a>Creación de una aplicación de función

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

Después de crear la instancia de Function App, puede crear funciones individuales en uno o más lenguajes distintos. Cree funciones [a través del portal](functions-create-first-azure-function.md#create-function), la [implementación continua](functions-continuous-deployment.md) o mediante la [carga con FTP](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp).

## <a name="service-plans"></a>Planes de servicio

Azure Functions tiene tres planes de servicio diferentes: Plan de consumo, plan Premium y plan Dedicado (App Service). Debe elegir el plan de servicio cuando se crea la aplicación de funciones y no se puede cambiar posteriormente. Para más información, consulte [Elección de un plan de hospedaje de Azure Functions](functions-scale.md).

Si va a ejecutar funciones de JavaScript en un plan Dedicado (App Service), debería elegir un plan con menos núcleos. Para obtener más información, consulte la [referencia de JavaScript relativa a las funciones](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Requisitos de la cuenta de almacenamiento

Al crear una aplicación de funciones, debe crear o vincular una cuenta de Azure Storage de uso general compatible con Blob, Queue y Table Storage. A nivel interno, Functions usa Storage para operaciones como la administración de desencadenadores y las ejecuciones de la función de registro. Algunas cuentas de almacenamiento no son compatibles con colas ni tablas, como las cuentas de almacenamiento solo para blob, Azure Premium Storage y cuentas de almacenamiento de uso general con replicación ZRS. 

Las cuentas de un tipo no admitido se filtran al crear una aplicación de función en Azure Portal. El portal también permite usar una cuenta de almacenamiento existente cuando esa cuenta está en la misma región que la aplicación de función que está creando. Si por algún motivo desea infringir el procedimiento recomendado para el rendimiento de tener la cuenta de almacenamiento que usa la aplicación de función en la misma región, debe crear la aplicación de función fuera del portal. 

>[!NOTE]
>Cuando usa el plan de hospedaje de consumo, los archivos de configuración de enlace y el código de la función se almacenan en Azure File Storage en la cuenta de almacenamiento principal. Si elimina la cuenta de almacenamiento principal, este contenido se suprimirá y no se podrá recuperar. 

Para más información sobre los tipos de cuenta de almacenamiento, consulte [Introducción de los servicios Azure Storage](../storage/common/storage-introduction.md#core-storage-services). 

## <a name="next-steps"></a>Pasos siguientes

Si bien Azure Portal facilita la tarea de crear y probar Functions, recomendamos el [desarrollo local](functions-develop-local.md). Después de crear una aplicación de funciones en el portal, deberá agregar una función. 

> [!div class="nextstepaction"]
> [Incorporación de una función desencadenada mediante HTTP](functions-create-first-azure-function.md#create-function)
