---
title: Esquema de definición de Azure Cloud Services (soporte extendido) (archivo .cscfg) | Microsoft Docs
description: Información relacionada con el esquema de definición de Cloud Services (soporte extendido)
ms.topic: article
ms.service: cloud-services-extended-support
ms.date: 10/14/2020
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 3aac5ca8d146f0cd74f483846d22fdf0052f86f2
ms.sourcegitcommit: 6272bc01d8bdb833d43c56375bab1841a9c380a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2021
ms.locfileid: "98744044"
---
# <a name="azure-cloud-services-extended-support-config-schema-cscfg-file"></a>Esquema de configuración de Azure Cloud Services (soporte extendido) (archivo .cscfg)

El archivo de configuración de servicio especifica el número de instancias de rol que se implementan para cada rol del servicio, los valores de los parámetros de configuración y las huellas digitales de los certificados asociados a un rol. Si el servicio forma parte de una red virtual, se debe proporcionar información de configuración de la red en el archivo de configuración de servicio, así como en el archivo de configuración de red virtual. La extensión predeterminada del archivo de configuración de servicio es. cscfg.

El modelo de servicio se describe en [Esquema de definición de servicio en la nube (clásico)](schema-csdef-file.md).

De forma predeterminada, el archivo de esquema de configuración de Diagnósticos de Azure se instala en el directorio `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas`. Reemplace `<version>` por la versión instalada del [SDK de Azure](https://azure.microsoft.com/downloads/).

Para más información sobre la configuración de roles en un servicio, consulte el artículo sobre el [modelo de servicio en la nube](../cloud-services/cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Esquema básico de configuración de servicio
El formato básico del archivo de configuración de servicio es el siguiente.

```xml
<ServiceConfiguration serviceName="<service-name>" osFamily="<osfamily-number>" osVersion="<os-version>" schemaVersion="<schema-version>">

  <Role …>
    …
  </Role>

  <NetworkConfiguration>
    …
  </NetworkConfiguration>

</ServiceConfiguration>
```

## <a name="schema-definitions"></a>Definiciones de esquema
En los temas siguientes se describe el esquema del elemento `ServiceConfiguration`:

- [Esquema de rol](schema-cscfg-role.md)
- [Esquema de NetworkConfiguration](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Espacio de nombres de configuración de servicio
El espacio de nombres XML del archivo de configuración de servicio es: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="serviceconfiguration-element"></a><a name="ServiceConfiguration"></a> Elemento ServiceConfiguration
El elemento `ServiceConfiguration` es el elemento de nivel superior del archivo de configuración de servicio.

En la tabla siguiente se describen los atributos del elemento `ServiceConfiguration`: Todos los valores de atributo son tipos de cadena.

| Atributo | Descripción |
| --------- | ----------- |
|serviceName|Necesario. Nombre del servicio en la nube. El nombre especificado aquí debe coincidir con el nombre especificado en el archivo de definición de servicio.|
|osFamily|Opcional. Especifica el sistema operativo invitado que se ejecutará en las instancias de rol del servicio en la nube. Para información sobre las versiones admitidas del SO invitado, consulte [Matriz de compatibilidad del SDK y versiones del SO invitado de Azure](../cloud-services/cloud-services-guestos-update-matrix.md).<br /><br /> Si no incluye un valor `osFamily` y no ha establecido el atributo `osVersion` en una versión específica del sistema operativo invitado, se usa un valor predeterminado de 1.|
|osVersion|Opcional. Especifica la versión del sistema operativo invitado que se ejecutará en las instancias de rol del servicio en la nube. Para más información sobre las versiones del sistema operativo invitado, consulte [Matriz de compatibilidad del SDK y versiones del SO invitado de Azure](../cloud-services/cloud-services-guestos-update-matrix.md).<br /><br /> Puede especificar que el sistema operativo invitado debe actualizarse automáticamente a la versión más reciente. Para ello, establezca el valor del atributo `osVersion` en `*`. Cuando se establece en `*`, las instancias de rol se implementan con la versión más reciente del SO invitado de la familia de SO especificada y se actualizarán automáticamente cuando se publican nuevas versiones de él.<br /><br /> Para especificar manualmente una versión concreta, use el elemento `Configuration String` de la tabla de la sección sobre **versiones futuras, actuales y transicionales del SO invitado** de [Matriz de compatibilidad del SDK y versiones del SO invitado de Azure](../cloud-services/cloud-services-guestos-update-matrix.md).<br /><br /> El valor predeterminado del atributo `osVersion` es `*`.|
|schemaVersion|Opcional. Especifica la versión del esquema de configuración de servicio. La versión del esquema permite que Visual Studio seleccione las herramientas del SDK correcto que se usarán en la validación del esquema si se instala más de una versión del SDK en paralelo. Para más información sobre la compatibilidad con versiones y esquemas, consulte [Matriz de compatibilidad del SDK y versiones del SO invitado de Azure](../cloud-services/cloud-services-guestos-update-matrix.md)|

El archivo de configuración de servicio debe contener un elemento `ServiceConfiguration`. El elemento `ServiceConfiguration` puede incluir cualquier número de elementos `Role` y ningún elemento `NetworkConfiguration` o 1.

## <a name="see-also"></a>Vea también

[Esquema de definición de Azure Cloud Services (soporte extendido) (archivo .cscfg)](schema-csdef-file.md)