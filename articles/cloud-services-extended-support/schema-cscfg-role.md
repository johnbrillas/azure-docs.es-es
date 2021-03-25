---
title: Esquema de rol de Azure Cloud Services (soporte extendido) | Microsoft Docs
description: Información relacionada con el esquema de rol de Cloud Services (soporte extendido)
ms.topic: article
ms.service: cloud-services-extended-support
ms.date: 10/14/2020
author: gachandw
ms.author: gachandw
ms.reviewer: mimckitt
ms.custom: ''
ms.openlocfilehash: 2567f5bb817a34f6274d5e265a266d67a9c81413
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98744048"
---
# <a name="azure-cloud-services-extended-support-config-role-schema"></a>Esquema de rol de configuración de Azure Cloud Services (soporte extendido).

El elemento `Role` del archivo de configuración especifica el número de instancias de rol que se implementan para cada rol del servicio, los valores de los parámetros de configuración y las huellas digitales de los certificados asociados a un rol.

Para más información sobre el esquema de configuración de servicios de Azure, consulte el [esquema de configuración de servicio en la nube (soporte extendido)](schema-cscfg-file.md). Para más información sobre el esquema de definición de servicio de Azure, consulte [Esquema de definición de Cloud Services (soporte extendido)](schema-csdef-file.md).

##  <a name="role-element"></a><a name="Role"></a> elemento de rol
En el ejemplo siguiente se muestra el elemento `Role` y sus elementos secundarios.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

En la tabla siguiente se describen los atributos del elemento `Role`.

| Atributo | Descripción |
| --------- | ----------- |
| name   | Necesario. Especifica el nombre del rol. El nombre debe coincidir con el nombre proporcionado en el rol en el archivo de definición de servicio.|
| vmName | Opcional. Especifica el nombre DNS de una máquina virtual. El nombre debe tener 10 caracteres como máximo.|

En la siguiente tabla se describen los elementos secundarios del elemento `Role`.

| Elemento | Descripción |
| ------- | ----------- |
| Instancias | Necesario. Especifica el número de instancias para implementar en el rol. El número de instancias se define mediante un entero en el atributo `count`.|
| Configuración   | Opcional. Especifica un nombre y un valor de configuración en una colección de valores para un rol. El nombre de la configuración se define mediante una cadena en el atributo `name` y el valor de configuración se define mediante una cadena en el atributo `value`.|
| Certificado | Opcional. Especifica el nombre, la huella digital y el algoritmo de un certificado de servicio que se va a asociar con el rol. El nombre del certificado se define mediante una cadena en el atributo `name`. La huella digital del certificado se define mediante una cadena de números hexadecimales que no contienen espacios en el atributo `thumbprint`. Los números hexadecimales deben estar representados por dígitos y caracteres alfanuméricos en mayúscula. El algoritmo de certificado se define mediante una cadena en el atributo `thumbprintAlgorithm`.|

## <a name="see-also"></a>Vea también
[Esquema de configuración de Cloud Services (soporte extendido)](schema-cscfg-file.md).