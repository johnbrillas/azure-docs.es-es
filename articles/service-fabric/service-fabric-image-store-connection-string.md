---
title: Cadena de conexión del almacén de imágenes de Azure Service Fabric
description: Obtenga información sobre la cadena de conexión del almacén de imágenes, incluidos sus usos y aplicaciones en un clúster de Service Fabric.
author: alexwun
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: alexwun
ms.openlocfilehash: 8fc0239dd18fc7071823a129a7dbc4f102023d66
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "86246204"
---
# <a name="understand-the-imagestoreconnectionstring-setting"></a>Descripción del valor ImageStoreConnectionString

En alguna parte de nuestra documentación, se menciona brevemente la existencia de un parámetro "ImageStoreConnectionString" sin describir lo que realmente significa. Y después de pasar por un artículo como [Implementación y eliminación de aplicaciones con PowerShell][10], parece que todo lo que hace es copiar y pegar el valor tal y como aparece en el manifiesto de clúster del clúster de destino. Por tanto, el valor se debe configurar por clúster, pero cuando se crea un clúster a través de [Azure Portal][11], no hay ninguna opción para configurar este valor y siempre es "fabric:ImageStore". Entonces, ¿cuál es la finalidad de este valor?

![Manifiesto de clúster][img_cm]

Service Fabric comenzó como una plataforma para consumo interno de Microsoft por numerosos y diferentes equipos, por lo que algunos aspectos de la misma son muy personalizables (por ejemplo, el "almacén de imágenes"). En esencia, el almacén de imágenes es un repositorio conectable para almacenar paquetes de aplicaciones. Cuando la aplicación se implementa en un nodo del clúster, dicho nodo descarga el contenido del paquete de aplicación desde el almacén de imágenes. ImageStoreConnectionString es un valor que incluye toda la información necesaria para los clientes y nodos para buscar el almacén de imágenes correcto para un clúster determinado.

Actualmente existen tres posibles tipos de proveedores de almacenes de imágenes y sus correspondientes cadenas de conexión son las siguientes:

1. Servicio de almacén de imágenes: "fabric:ImageStore"

2. Sistema de archivos: "file:[ruta de acceso al sistema de archivos]"

3. Azure Storage: "xstore:DefaultEndpointsProtocol=https;AccountName=[...];AccountKey=[...];Container=[...]"

El tipo de proveedor utilizado en producción es el servicio de almacén de imágenes, que es un servicio de sistema persistente con estado que puede ver en Service Fabric Explorer. 

![Servicio de almacén de imágenes][img_is]

El hospedaje del almacén de imágenes en un servicio de sistema dentro del propio clúster elimina dependencias externas para el repositorio de paquetes y nos proporciona mayor control sobre la localidad del almacenamiento. Es probable que las futuras mejoras para el almacén de imágenes se centren primero, si no exclusivamente, en el proveedor de dicho almacén. La cadena de conexión para el proveedor del servicio de almacén de imágenes no tiene ninguna información exclusiva puesto que el cliente ya está conectado al clúster de destino. El cliente solo debe saber que se deben usar protocolos cuyo destino sea el servicio del sistema.

El proveedor del sistema de archivos se utiliza en lugar del servicio de almacén de imágenes para los clústeres one-box locales durante el desarrollo para arrancar el clúster un poco más rápido. La diferencia es generalmente pequeña, pero es una optimización útil para la mayoría de la gente durante el desarrollo. Es posible implementar también un clúster one-box local con los otros tipos de proveedor de almacenamiento, pero normalmente no hay ninguna razón para hacerlo ya que el flujo de trabajo de desarrollo y pruebas sigue siendo el mismo independientemente del proveedor. El proveedor de Azure Storage solo existe por la compatibilidad heredada de los clústeres antiguos antes de que se introdujera el proveedor de servicios de almacén de imágenes.

Además, ni el proveedor del sistema de archivos ni el proveedor de Azure Storage se deben usar como método para compartir un almacén de imágenes entre varios clústeres; esto provocará daños en los datos de configuración del clúster dado que cada clúster puede escribir datos en conflicto en el almacén de imágenes. Para compartir paquetes de aplicación aprovisionados entre varios clústeres, use en su lugar archivos [sfpkg][12], que se pueden cargar en cualquier almacén externo con un URI de descarga.

Por lo tanto, aunque ImageStoreConnectionString se puede configurar, solo se usa el valor predeterminado. Al publicar en Azure a través de Visual Studio, el parámetro se establece automáticamente en consecuencia. Para la implementación mediante programación en clústeres hospedados en Azure, la cadena de conexión siempre es "fabric:ImageStore". Sin embargo, en caso de duda, su valor siempre se puede comprobar recuperando el manifiesto de clúster mediante [PowerShell](/powershell/module/servicefabric/get-servicefabricclustermanifest), [.NET](/previous-versions/azure/reference/mt161375(v=azure.100)) o [REST](/rest/api/servicefabric/get-a-cluster-manifest). Tanto los clústeres de producción como de prueba locales siempre deben configurarse para usar también el proveedor de servicio de almacén de imágenes.

### <a name="next-steps"></a>Pasos siguientes
[Implementación y eliminación de aplicaciones con PowerShell][10]

<!--Image references-->
[img_is]: ./media/service-fabric-image-store-connection-string/image_store_service.png
[img_cm]: ./media/service-fabric-image-store-connection-string/cluster_manifest.png

[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-cluster-creation-via-portal.md
[12]: service-fabric-package-apps.md#create-an-sfpkg
