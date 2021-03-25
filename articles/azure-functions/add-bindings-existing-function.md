---
title: Conexión de funciones a otros servicios de Azure
description: Obtenga información sobre cómo agregar enlaces que conecten otros servicios de Azure a una función existente en un proyecto de Azure Functions.
ms.topic: how-to
ms.date: 04/29/2020
ms.openlocfilehash: d1c6f5bb8ca5fcf995b8a8d326abbec96f1d2e35
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99258208"
---
# <a name="connect-functions-to-azure-services-using-bindings"></a>Conexión de funciones a servicios de Azure mediante enlaces

Al crear una función, se agrega código del desencadenador específico del idioma en el proyecto desde un conjunto de plantillas de desencadenador. Si desea conectar la función a otros servicios mediante enlaces de entrada o salida, debe agregar definiciones de enlace específicas en la función. Para más información acerca de los enlaces, consulte [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md).

## <a name="local-development"></a>Desarrollo local       

Al desarrollar funciones localmente, es necesario que actualice el código de función para agregar enlaces. El uso de Visual Studio Code puede facilitar la adición de enlaces a una función.  

### <a name="visual-studio-code"></a>Visual Studio Code

Al usar Visual Studio Code para desarrollar la función y utilizar esta un archivo function.json, la extensión de Azure Functions puede agregar automáticamente un enlace a un archivo function.json existente. Para obtener más información, consulte [Incorporación de enlaces de entrada y de salida](functions-develop-vs-code.md#add-input-and-output-bindings).   

### <a name="manually-add-bindings-based-on-examples"></a>Incorporación manual de enlaces basados en ejemplos

Al agregar un enlace a una función existente, tendrá que actualizar tanto el código de función como el archivo de configuración function.json, si su lenguaje los usa. Tanto la biblioteca de clases .NET como las funciones de Java usan atributos en lugar de function.json, de modo que tendrá que actualizarlo en su lugar.

Use la tabla siguiente para buscar ejemplos de tipos de enlace específicos que puede usar para que le guíen al actualizar una función existente. En primer lugar, elija la pestaña de lenguaje que corresponde al proyecto. 

[!INCLUDE [functions-bindings-code-example-chooser](../../includes/functions-bindings-code-example-chooser.md)]

## <a name="azure-portal"></a>Azure portal

Al desarrollar las funciones en [Azure Portal](https://portal.azure.com), se agregan enlaces de entrada y salida en la pestaña **Integrar** para una función determinada. Los nuevos enlaces se agregan al archivo function.json o a los atributos de método, dependiendo del lenguaje. En los siguientes artículos se muestran ejemplos de cómo agregar enlaces a una función existente en el portal:

+ [Enlace de salida de Queue Storage](functions-integrate-storage-queue-output-binding.md)
+ [Enlace de salida de Azure Cosmos DB](functions-integrate-store-unstructured-data-cosmosdb.md)

## <a name="next-steps"></a>Pasos siguientes

+ [Conceptos básicos sobre los enlaces y desencadenadores de Azure Functions](functions-triggers-bindings.md)
