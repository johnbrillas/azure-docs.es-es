---
author: memildin
ms.service: security-center
ms.topic: include
ms.date: 03/14/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: c142f6a00275b4b21b6bbc64e51a5c4d7d957258
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467989"
---
- Se debe restringir el acceso a las cuentas de almacenamiento con configuraciones de red virtual y firewall. Las cuentas de almacenamiento deben tener el acceso de red restringido.
- Las variables de la cuenta de Automation deben cifrarse. Las variables de la cuenta de Automation deben cifrarse.
- Azure Cache for Redis debe residir en una red virtual
- Las cuentas de Azure Cosmos DB deben usar claves administradas por el cliente para cifrar los datos en reposo
- Las áreas de trabajo de Azure Machine Learning deben cifrarse con una clave administrada por el cliente (CMK).
- Azure Spring Cloud debe usar la inserción de red
- Las cuentas de Cognitive Services deben habilitar el cifrado de datos con una clave administrada por el cliente (CMK)
- Debe aplicar los límites de CPU y memoria de los contenedores.
- Las imágenes de contenedor solo deben implementarse desde registros de confianza
- Las instancias de Container Registry se deben cifrar con una clave administrada por el cliente (CMK)
- Debe evitar los contenedores con elevación de privilegios.
- Deben evitarse los contenedores que comparten espacios de nombres de host confidenciales.
- Los contenedores solo deben escuchar en los puertos permitidos.
- El sistema de archivos raíz inmutable (de solo lectura) debe aplicarse para los contenedores.
- Las claves de Key Vault deben tener una fecha de expiración
- Los secretos de Key Vault deben tener una fecha de expiración
- Los almacenes de claves deben tener habilitada la protección contra operaciones de purga
- Los almacenes de claves deben tener habilitada la eliminación temporal
- Deben aplicarse funcionalidades de Linux con privilegios mínimos para los contenedores
- Solo deben habilitarse las conexiones seguras a su instancia de Redis Cache. Solo deben habilitarse las conexiones seguras a su instancia de Azure Cache for Redis.
- La opción de reemplazar o deshabilitar el perfil de AppArmor de los contenedores debe estar restringida.
- Deben evitarse los contenedores con privilegios.
- Debe evitar la ejecución de contenedores como usuario raíz.
- La transferencia segura a las cuentas de almacenamiento debe habilitarse. La transferencia segura a las cuentas de almacenamiento debe estar habilitada.
- Los clústeres de Service Fabric deben tener la propiedad ClusterProtectionLevel establecida en EncryptAndSign Service Fabric. Los clústeres deben tener la propiedad ClusterProtectionLevel establecida en EncryptAndSign.
- Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente. Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente.
- Los servicios solo deben escuchar en los puertos permitidos.
- No se debe permitir el acceso público a la cuenta de almacenamiento
- Las cuentas de almacenamiento se deben migrar a nuevos recursos de Azure Resource Manager. Las cuentas de almacenamiento se deben migrar a nuevos recursos de Azure Resource Manager.
- Las cuentas de almacenamiento deben restringir el acceso a la red mediante el uso de reglas de red virtual
- El uso de puertos y redes de hosts debe estar restringido.
- El uso de montajes de volúmenes HostPath de pod debe restringirse a una lista conocida, con el fin de restringir el acceso a los nodos de los contenedores en peligro
- El período de validez de los certificados almacenados en Azure Key Vault no debe superar los 12 meses
- Las máquinas virtuales se deben migrar a nuevos recursos de Azure Resource Manager. Las máquinas virtuales se deben migrar a nuevos recursos de Azure Resource Manager.
- El firewall de aplicaciones web (WAF) debe estar habilitado para Application Gateway
- Web Application Firewall (WAF) debe estar habilitado en Azure Front Door Service

