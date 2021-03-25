---
title: Estrategia de búsqueda de delimitadores
description: Más información sobre las diferentes estrategias al llamar a la API de búsqueda
author: pamistel
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: pamistel
ms.date: 02/11/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 43273ccd7c882bbac6cbc68d359db4ecb100800e
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102617410"
---
# <a name="understanding-the-anchorlocatecriteria-class"></a>Descripción de la clase AnchorLocateCriteria
En este artículo aprenderá las diferentes opciones que puede usar al consultar un delimitador. Analizaremos la clase AnchorLocateCriteria, sus opciones y las combinaciones de opciones válidas.

## <a name="anchor-locate-criteria"></a>Criterios de búsqueda de delimitadores
La [clase AnchorLocateCriteria](https://docs.microsoft.com/dotnet/api/microsoft.azure.spatialanchors.anchorlocatecriteria) ayuda a consultar el servicio para los delimitadores creados previamente. Se puede usar un objeto AnchorLocateCriteria por cada monitor en cualquier momento. Cada objeto AnchorLocateCriteria debe incluir **exactamente una** de las siguientes propiedades: [Identifiers](#identifiers), [NearAnchor](#nearanchor) o [NearDevice](#neardevice). Si lo desea, puede establecer propiedades adicionales como [Strategy](#strategy), [BypassCache](#bypasscache) y [RequestedCategories](#requestedcategories). 

### <a name="properties"></a>Propiedades
Defina **exactamente una** de las siguientes propiedades en el monitor:
#### <a name="identifiers"></a>Identificadores
*Valor predeterminado: matriz de cadenas vacía*

Si usa Identifiers, puede definir una lista de identificadores de delimitador para los delimitadores que quiera buscar. Los identificadores de delimitador se devuelven inicialmente después de la creación correcta del delimitador. Con Identifiers especificado, AnchorLocateCriteria restringe el conjunto de delimitadores solicitados a los delimitadores con identificadores de delimitador coincidentes. Esta propiedad se especifica mediante una matriz de cadenas. 

#### <a name="nearanchor"></a>NearAnchor
*Valor predeterminado: sin establecer*

Si usa NearAnchor, puede especificar que AnchorLocateCriteria restrinja el conjunto de delimitadores solicitados a los delimitadores dentro de una distancia deseada fuera de un delimitador elegido. Debe proporcionar este delimitador elegido como delimitador de origen. También puede establecer la distancia deseada fuera del delimitador de origen y el número máximo de delimitadores devueltos para limitar aún más la búsqueda.
Esta propiedad se especifica mediante un objeto NearAnchorCriteria.

#### <a name="neardevice"></a>NearDevice
*Valor predeterminado: sin establecer*

Si usa NearDevice, puede especificar que AnchorLocateCriteria restrinja el conjunto de delimitadores solicitados a los que estén cerca de la ubicación física del dispositivo. Cualquier sensor habilitado se usará para ayudar a detectar delimitadores alrededor del dispositivo. Para aumentar al máximo la probabilidad de encontrar delimitadores, debe configurar SensorCapabilities para que la sesión tenga acceso a todos los sensores adecuados. Para obtener más información sobre la configuración y el uso de esta propiedad, consulte [Relocalización general: Azure Spatial Anchors | Microsoft Docs](https://docs.microsoft.com/azure/spatial-anchors/concepts/coarse-reloc) y *Creación y localización de delimitadores mediante relocalización general* en [C#](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-unity), [Objective-C](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-unity), [Swift](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-swift), [Java](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-java), [c++/NDK](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-cpp-ndk) y [c++/WinRT](https://docs.microsoft.com/azure/spatial-anchors/how-tos/set-up-coarse-reloc-cpp-winrt).
Esta propiedad se especifica mediante un objeto NearAnchorCriteria.

### <a name="additional-properties"></a>Propiedades adicionales
#### <a name="bypasscache"></a>BypassCache
*Valor predeterminado: false*

Cuando se crea o se encuentra un delimitador en una sesión, también se almacena en la memoria caché.  Con esta propiedad establecida en false, cualquier consulta posterior en la misma sesión devolverá el valor almacenado en caché. No se realiza ninguna solicitud al servicio ASA.

#### <a name="requestedcategories"></a>RequestedCategories
*Valor predeterminado: propiedades | datos espaciales*

Esta propiedad se usa para determinar qué datos se devuelven en una consulta con AnchorLocateCriteria. El valor predeterminado devuelve las propiedades y los datos espaciales, por lo que no debe modificarse si se busca obtener tanto propiedades como datos espaciales. Esta propiedad se puede especificar mediante la enumeración AnchorDataCategory.

Valor de enumeración AnchorDataCategory | Datos devueltos
-----|------------
None | No se devuelven datos
Propiedades| Se devuelven propiedades de delimitador, incluidas AppProperties.
Espacial| Se devuelve información espacial sobre un delimitador.

#### <a name="strategy"></a>Estrategia
*Valor predeterminado: AnyStrategy*

Strategy define aún más cómo deben ubicarse los delimitadores. La propiedad Strategy se puede especificar mediante una enumeración LocateStrategy.

Valor de enumeración LocateStrategy | Descripción
---------------|------------
AnyStrategy | Esta estrategia permite al sistema usar combinaciones de estrategias VisualInformation y Relationship para buscar delimitadores. 
VisualInformation|Esta estrategia intenta buscar delimitadores comparando la información visual del entorno actual con la de la superficie visual del delimitador. La superficie visual de un delimitador hace referencia a la información visual asociada actualmente al delimitador. Esta información visual se suele recopilar durante la creación del delimitador, aunque no siempre es así. Actualmente solo es posible usar esta estrategia en combinación con las propiedades NearDevice o Identifiers.
Relación|Esta estrategia intenta encontrar delimitadores mediante el uso de [delimitadores conectados](https://docs.microsoft.com/azure/spatial-anchors/concepts/anchor-relationships-way-finding#connect-anchors) existentes. Actualmente solo es posible usar esta estrategia en combinación con las propiedades NearAnchor o Identifiers. Cuando se usa con la propiedad Identifiers, es necesario que, en la misma sesión, el usuario haya encontrado previamente un delimitador con relaciones de conexión ya establecidas con los delimitadores cuyos identificadores se hayan especificado en la matriz Identifiers. 


### <a name="valid-combinations-of-locatestrategy-and-anchorlocatecriteria-properties"></a>Combinaciones válidas de las propiedades LocateStrategy y AnchorLocateCriteria 

Actualmente el sistema no permite todas las combinaciones de las propiedades Strategy y AnchorLocateCriteria. En la siguiente tabla se muestran las combinaciones permitidas:



Propiedad | AnyStrategy | Relación | VisualInformation
-------- | ------------|--------------|-------------------
Identificadores | &check;    | &check;     | &check;
NearAnchor  | &check;   (el valor predeterminado será Relationship) | &check;    | 
NearDevice  | &check;    |   | &check;




## <a name="next-steps"></a>Pasos siguientes

Consulte [Cómo crear y ubicar delimitadores con Azure Spatial Anchors](https://docs.microsoft.com/azure/spatial-anchors/create-locate-anchors-overview) para ver más ejemplos de uso de la clase AnchorLocateCriteria.