---
title: Configuración de claves administradas por el cliente Azure API for FHIR
description: Incorpore su propia característica clave compatible con Azure API for FHIR mediante Cosmos DB
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: overview
ms.date: 09/28/2020
ms.author: ginle
ms.openlocfilehash: ae78aa80594e46b02d77adcafed961e801780d4f
ms.sourcegitcommit: eb546f78c31dfa65937b3a1be134fb5f153447d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99430266"
---
# <a name="configure-customer-managed-keys-at-rest"></a>Configuración de claves administradas por el cliente en reposo

Cuando crea una cuenta de Azure API for FHIR, de forma predeterminada los datos se cifran mediante claves administradas por Microsoft. Ahora puede agregar una segunda capa de cifrado de datos mediante una clave que usted mismo elige y administra.

En Azure, para hacerlo se suele utilizar una clave de cifrado en la instancia de Azure Key Vault del cliente. Azure SQL, Azure Storage y Cosmos DB son algunos ejemplos que proporcionan esta funcionalidad hoy en día. Azure API for FHIR aprovecha esta compatibilidad con Cosmos DB. Al crear una cuenta, tendrá la opción de especificar el identificador URI de una clave de Azure Key Vault. Esta clave se pasará a Cosmos DB al aprovisionar la cuenta de la base de datos. Cuando se realiza una solicitud de FHIR, Cosmos DB captura su clave y la usa para cifrar o descifrar los datos. Para empezar, puede consultar los vínculos siguientes:

- [Registro del proveedor de recursos de Azure Cosmos DB para su suscripción a Azure](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) 
- [Configuración de la instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#configure-your-azure-key-vault-instance)
- [Adición de una directiva de acceso a una instancia de Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance)
- [Generación de una clave en Azure Key Vault](../cosmos-db/how-to-setup-cmk.md#generate-a-key-in-azure-key-vault)

## <a name="specify-the-azure-key-vault-key"></a>Especifique la clave de Azure Key Vault

Al crear una cuenta de Azure API for FHIR en Azure Portal, puede ver la opción de configuración "Cifrado de datos" en la sección "Configuración de base de datos" de la pestaña "Configuración adicional". De forma predeterminada, se elegirá la opción de clave administrada por el servicio. 

La clave se puede elegir en KeyPicker:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-keypicker.png" alt-text="KeyPicker":::

O bien, se puede especificar la clave de Azure Key Vault seleccionando la opción "Clave administrada por el cliente". Aquí puede escribir el identificador URI de la clave:

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-create.png" alt-text="Creación de Azure API for FHIR":::

En el caso de las cuentas de FHIR existentes, puede ver la opción de cifrado de clave (clave administrada por el cliente o por el servicio) en la hoja "Base de datos" como se indica a continuación. La opción de configuración no se puede modificar una vez elegida. Sin embargo, se puede modificar y actualizar la clave.

:::image type="content" source="media/bring-your-own-key/bring-your-own-key-database.png" alt-text="Base de datos":::

Además, puede crear una nueva versión de la clave especificada, después de la cual los datos se cifrarán con la nueva versión sin ninguna interrupción del servicio. También puede quitar el acceso a la clave para quitar el acceso a los datos. Cuando la clave esté deshabilitada, las consultas generarán un error. Si se vuelve a habilitar, las consultas se volverán a realizar correctamente.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido a configurar las claves administradas por el cliente en reposo. A continuación, puede consultar la sección de preguntas más frecuentes sobre Azure Cosmos DB: 
 
>[!div class="nextstepaction"]
>[Cosmos DB: procedimientos para configurar CMK](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk#frequently-asked-questions)
