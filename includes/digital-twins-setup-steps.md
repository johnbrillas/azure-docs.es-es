---
author: baanders
description: archivo de inclusión para información general de los pasos de la configuración de Azure Digital Twins
ms.service: digital-twins
ms.topic: include
ms.date: 10/14/2020
ms.author: baanders
ms.openlocfilehash: c1cfab8c9e68d9e6b0c3b84e8848645a50c24710
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100560776"
---
La configuración completa de una instancia nueva de Azure Digital Twins consta de dos partes:
1. **Creación de la instancia**
2. **Configuración de permisos de acceso de usuarios**: los usuarios de Azure deben tener el rol *Propietario de datos de Azure Digital Twins* en la instancia de Azure Digital Twins para poder administrarla y administrar sus datos. En este paso, como propietario o administrador de la suscripción de Azure, se asignará este rol a la persona que vaya a administrar la instancia de Azure Digital Twins. Puede ser usted mismo o alguna otra persona de la organización.
 
>[!NOTE]
>Estas operaciones están pensadas para que las realice un usuario con la capacidad de administrar los recursos y el acceso de usuario en la suscripción de Azure. Aunque algunos pasos se pueden realizar con permisos inferiores, será necesaria la colaboración de alguien con estos permisos para configurar por completo una instancia que se pueda usar. Para más información, consulte la sección [*Requisitos previos: permisos necesarios*](#prerequisites-permission-requirements) que aparece a continuación.