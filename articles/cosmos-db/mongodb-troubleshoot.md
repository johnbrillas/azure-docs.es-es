---
title: Solución de errores habituales de la API de Azure Cosmos DB para MongoDB
description: En este documento se describen las formas de solucionar los problemas habituales detectados en la API de Azure Cosmos DB para MongoDB.
author: christopheranderson
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: troubleshooting
ms.date: 07/15/2020
ms.author: chrande
ms.openlocfilehash: 26097408d0b83b043f4a25183146c892fc4b48ad
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2021
ms.locfileid: "98538550"
---
# <a name="troubleshoot-common-issues-in-azure-cosmos-dbs-api-for-mongodb"></a>Solución de errores habituales de la API de Azure Cosmos DB para MongoDB
[!INCLUDE[appliesto-mongodb-api](includes/appliesto-mongodb-api.md)]

En el siguiente artículo se describen los errores comunes y las soluciones para las implementaciones usan la API de Azure Cosmos DB para MongoDB.

>[!Note]
> Azure Cosmos DB no hospeda el motor de MongoDB. Proporciona una implementación del protocolo de transferencia de MongoDB. Por lo tanto, algunos de estos errores solo se encuentran en la API de Azure Cosmos DB para MongoDB. 

## <a name="common-errors-and-solutions"></a>Errores habituales y soluciones

| Código       | Error                | Descripción  | Solución  |
|------------|----------------------|--------------|-----------|
| 2 | La ruta de acceso del índice correspondiente al elemento order-by especificado se excluye o la consulta order by no tiene un índice compuesto correspondiente desde el que se puede atender. | La consulta solicita que se ordene por un campo que no está indexado. | Cree un índice coincidente (o índice compuesto) para la consulta de ordenación que se intenta realizar. |
| 13 | No autorizado | La solicitud no tiene los permisos necesarios para completarse. | Asegúrese de establecer los permisos adecuados para la base de datos y la colección.  |
| 16 | InvalidLength | La solicitud especificada tiene una longitud no válida. | Si usa la función explain(), asegúrese de especificar una sola operación. |
| 26 | NamespaceNotFound | No se encuentra la base de datos o la colección a la que se hace referencia en la consulta. | Asegúrese de que el nombre de la base de datos o colección coincida exactamente con el nombre de la consulta.|
| 50 | ExceededTimeLimit | La solicitud ha superado el tiempo de espera de 60 segundos de ejecución. |  Este error puede deberse a muchas causas. Una de las causas es cuando la capacidad de las unidades de solicitud asignada actualmente no es suficiente para completar la solicitud. Para solucionar esto, se pueden aumentar las unidades de solicitud de esa colección o base de datos. En otros casos, este error se puede solucionar dividiendo una solicitud grande en otras más pequeñas. Si se vuelve a intentar una operación de escritura que ha recibido este error, se puede producir una operación de escritura duplicada.|
| 61 | ShardKeyNotFound | El documento de la solicitud no contenía la clave de partición de la colección (clave de partición de Azure Cosmos DB). | Asegúrese de que la clave de partición de la colección se use en la solicitud.|
| 66 | ImmutableField | La solicitud está intentando cambiar un campo inmutable. | los campos "id" son inmutables. Asegúrese de que la solicitud no trate de actualizar el campo. |
| 67 | CannotCreateIndex | No se puede completar la solicitud para crear un índice. | Se pueden crear hasta 500 índices de un solo campo en cada contenedor. Se pueden incluir hasta ocho campos en un índice compuesto (los índices compuestos se admiten en la versión 3.6 y posteriores). |
| 115 | CommandNotSupported | No se admite el intento de solicitud. | Se deben especificar los detalles adicionales en el error. Si esta funcionalidad es importante para sus implementaciones, háganoslo saber creando una incidencia de soporte técnico en [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). |
| 11000 | DuplicateKey | La clave de partición (clave de partición de Azure Cosmos DB) del documento que se está insertando ya existe en la colección, o se infringió alguna restricción de un campo de índice único. | Use la función update() para actualizar un documento existente. Si se infringió la restricción de un campo de índice único, inserte o actualice el documento con un valor de campo que no exista todavía en la partición. |
| 16500 | TooManyRequests  | El número total de unidades de solicitud consumidas es mayor que la tasa de unidades de solicitud aprovisionadas para la colección y se ha limitado. | Considere la posibilidad de escalar el rendimiento asignado a un contenedor o un conjunto de contenedores desde Azure Portal o vuelva a intentarlo. Si [habilita SSR](prevent-rate-limiting-errors.md) (reintento en el servidor), Azure Cosmos DB reintenta automáticamente las solicitudes que no se envían debido a este error. |
| 16501 | ExceededMemoryLimit | Como se trata de un servicio de varios inquilinos, la operación ha superado la asignación de memoria del cliente. | Reduzca el ámbito de la operación a través de criterios de consulta más restrictivos o póngase en contacto con soporte técnico desde [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Ejemplo: `db.getCollection('users').aggregate([{$match: {name: "Andy"}}, {$sort: {age: -1}}]))` |
| 40324 | Nombre de fase de canalización no reconocido. | No se reconoció el nombre de fase en la solicitud de canalización de agregación. | Asegúrese de que todos los nombres de canalización de agregación de la solicitud sean válidos. |
| - | Problemas de versión de la conexión de MongoDB | Las versiones anteriores de los controladores de MongoDB son no detectan el nombre de la cuenta de Azure Cosmos en las cadenas de conexión. | Anexe *appName = @**nombreDeLaCuenta**@* al final de la API de Cosmos DB para la cadena de conexión de MongoDB, donde ***nombreDeLaCuenta*** es el nombre de la cuenta de Cosmos DB. |
| - | Problemas de red del cliente de MongoDB (como excepciones de socket o endOfStream)| Error en la solicitud de red. Esto suele deberse a una conexión TCP inactiva que el cliente de MongoDB está tratando de usar. Los controladores de MongoDB suelen usar la agrupación de conexiones, que da como resultado una conexión aleatoria elegida del grupo que se usa para una solicitud. Las conexiones inactivas suelen agotar el tiempo de espera de Azure Cosmos DB después de cuatro minutos. | Puede volver a intentar estas solicitudes con error en el código de la aplicación, cambiar la configuración del cliente de MongoDB (controlador) para desmontar las conexiones TCP inactivas antes de que se agote el tiempo de espera de cuatro minutos, o configurar las opciones Keepalive del sistema operativo para mantener las conexiones TCP en estado activo. |

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [usar Studio 3T](mongodb-mongochef.md) con la API de Azure Cosmos DB para MongoDB.
- Aprenda a [usar Robo 3T](mongodb-robomongo.md) con la API de Azure Cosmos DB para MongoDB.
- Explore [ejemplos](mongodb-samples.md) de MongoDB con la API de Azure Cosmos DB para MongoDB.
