---
title: Información sobre la actualización de dispositivos en grupos de Azure IoT Hub | Microsoft Docs
description: Descubra cómo se usan los grupos de dispositivos.
author: aysancag
ms.author: aysancag
ms.date: 2/09/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: 18388f067ccb5b8a8876aeae685664694c207613
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "101678844"
---
# <a name="device-groups"></a>Grupos de dispositivos

Un grupo de dispositivos es una colección de dispositivos. Los grupos de dispositivos conforman un buen mecanismo para escalar las implementaciones en numerosos dispositivos. Cada dispositivo pertenece exclusivamente a un grupo.
Si lo desea, puede crear varios grupos para organizar los dispositivos. Por ejemplo, Contoso podría utilizar el grupo de dispositivos "Distribución de paquetes piloto" para los dispositivos del laboratorio de pruebas y el grupo de dispositivos "Evaluación" para los dispositivos que el equipo de campo utiliza en el centro de operaciones. Además, podría agrupar los dispositivos de producción en función de las regiones geográficas en las que opera, y actualizar así los dispositivos conforme a una programación que se ajuste a la zona horaria de cada región. 


## <a name="using-device-twin-tag-for-device-group-creation"></a>Uso de etiquetas de dispositivos gemelos para la creación de grupos de dispositivos

Las etiquetas de dispositivos gemelos permiten a los usuarios agrupar los dispositivos. Los dispositivos deben tener una clave ADUGroup y un valor en su dispositivo gemelo para que puedan agruparse.

### <a name="device-twin-tag-format"></a>Formato de etiqueta de dispositivo gemelo

```markdown
"tags": {
  "ADUGroup": "<CustomTagValue>"
}
```


## <a name="uncategorized-device-group"></a>Grupo de dispositivos "Uncategorized" (sin categoría)

"Uncategorized" (Sin categoría) es una palabra reservada que se utiliza para agrupar dispositivos que:
- No tienen una etiqueta de dispositivo gemelo de ADUGroup.
- Tienen una etiqueta de dispositivo gemelo de ADUGroup, pero no se crea ningún grupo con este nombre.

Por ejemplo, piense en unos dispositivos que tengan las siguientes etiquetas de dispositivo gemelo:

```markdown
"deviceId": "Device1",
"tags": {
  "ADUGroup": "Group1"
}
```

```markdown
"deviceId": "Device2",
"tags": {
  "ADUGroup": "Group1"
}
```

```markdown
"deviceId": "Device3",
"tags": {
  "ADUGroup": "Group2"
}
```

```markdown
"deviceId": "Device4",
```

Estos son los dispositivos y los grupos que se pueden crear para ellos.

|Dispositivo |Grupo  |
|-----------|--------------|
|Device1    |Group1|
|Device2    |Group1|
|Device3    |Group2|
|Device4    |Sin categoría|



## <a name="next-steps"></a>Pasos siguientes

[Crear grupo de dispositivos](./create-update-group.md)
