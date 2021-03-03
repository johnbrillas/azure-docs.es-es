---
title: Información sobre el cumplimiento de Device Update para Azure IoT Hub | Microsoft Docs
description: Descubra cómo Device Update para Azure IoT Hub mide el cumplimiento en la actualización de dispositivos.
author: vimeht
ms.author: vimeht
ms.date: 2/11/2021
ms.topic: conceptual
ms.service: iot-hub-device-update
ms.openlocfilehash: ac6094efde8a32b1fcc04c55bbc537afeb4166f7
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660760"
---
# <a name="device-update-compliance"></a>Cumplimiento de Device Update

En Device Update para IoT Hub, el cumplimiento determina cuántos dispositivos tienen instalada la actualización con la versión más reciente que es compatible. Se considera que un dispositivo es conforme si tiene instalada la actualización con la versión más reciente que es compatible. 

Por ejemplo, imaginemos una instancia de Device Update que tiene las siguientes actualizaciones:

|Nombre de actualización|Versión de actualización|Modelo de dispositivo compatible|
|-----------|--------------|-----------------------|
|Update 1    |1.0    |Model1|
|Update2    |1.0    |Model2|
|Update3    |2.0    |Model1|

Supongamos que se han creado las siguientes implementaciones:

|Nombre de implementación    |Nombre de actualización    |Grupo de destino|
|-----------|--------------|-------------------|
|Deployment1    |Update 1    |Group1|
|Deployment2    |Update2    |Group2|
|Deployment3    |Update3    |Grupo3|

Ahora, imaginemos los siguientes dispositivos, con las pertenencias a grupos y las versiones instaladas que se indican:

|deviceId   |Modelo del dispositivo   |Versión de actualización instalada|Grupo |Cumplimiento normativo|
|-----------|--------------|-----------------------|-----|---------|
|Device1    |Model1 |1.0    |Group1 |Nuevas actualizaciones disponibles</span>|
|Device2    |Model1 |2.0    |Grupo3 |Con la última actualización|
|Device3    |Model2 |1.0    |Group2 |Con la última actualización|
|Device4    |Model1 |1.0    |Grupo3 |Actualización en curso|

Device1 y Device4 no son conformes, ya que tienen instalada la versión 1.0 cuando hay otra más reciente, Update3, que es compatible con el modelo de la instancia de Device Update. Tanto Device2 como Device3 son conformes, ya que tienen instaladas las actualizaciones más recientes que son compatibles con sus modelos.

El cumplimiento no tiene en cuenta si una actualización está implementada en un grupo de un dispositivo o no, sino que examina las actualizaciones publicadas en Device Update. Por tanto, en el ejemplo anterior, aunque Device1 tenga instalada la actualización recibida, se considera que no es conforme. Se seguirá considerando que no es conforme hasta que se instale correctamente la actualización Update3. El estado de cumplimiento puede ayudarle a identificar si es necesario realizar nuevas implementaciones. 

Tal y como se indicó anteriormente, existen tres estados de cumplimiento en Device Update para IoT Hub:

*   **Con la última actualización** : el dispositivo ha instalado la versión de la actualización más reciente publicada en Device Update que es compatible.
*   **Actualización en curso**: hay una implementación activa en curso que proporcionará al dispositivo la actualización con la versión más reciente que es compatible.
*   **Nuevas actualizaciones disponibles**: algún dispositivo aún no tiene instaladas las versiones más recientes de la actualización y no está en ninguna implementación activa de esa actualización.
