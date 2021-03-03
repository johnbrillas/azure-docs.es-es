---
title: Actualización de Azure Percept DK a través de una conexión USB
description: Aprenda a actualizar Azure Percept DK a través de una conexión USB
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 7f5e5e4da9fea671fc85a55fc8cc191fa14b720f
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2021
ms.locfileid: "101660572"
---
# <a name="how-to-update-azure-percept-dk-over-a-usb-connection"></a>Actualización de Azure Percept DK a través de una conexión USB

Siga esta guía para aprender cómo actualizar la placa base de Azure Percept DK mediante USB.

## <a name="prerequisites"></a>Requisitos previos
- Equipo host con un puerto USB-C o USB-A disponible.
- Placa base de Azure Percept DK (kit de desarrollo) y cable suministrado de USB-C a USB-C. Si el equipo host tiene un puerto USB-A, pero no un puerto USB-C, puede usar un cable de USB-C a USB-A (se vende por separado).
- Instale [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (se requiere acceso de administrador).
- Instale la herramienta NXP UUU. [Descargue la versión más reciente](https://github.com/NXPmicro/mfgtools/releases) del archivo uuu.exe (para Windows) o del archivo uuu (para Linux) en la pestaña Activos.
- [Instale 7-Zip](https://www.7-zip.org/). Este software se utilizará para extraer el archivo de imagen sin procesar de su archivo comprimido XZ. Descargue e instale el archivo .exe adecuado.

## <a name="steps"></a>Pasos
1.  Descargue los siguientes [tres archivos de actualización USB](https://go.microsoft.com/fwlink/?linkid=2155734):
    - pe101-uefi-***&lt;número de versión&gt;***.raw.xz
    - emmc_full.txt
    - fast-hab-fw.raw
 
1. Extraiga a pe101-uefi-* **&lt;número de versión &gt;** _.raw desde el archivo comprimido pe101-uefi_* _&lt;número de versión&gt;_ **.raw.xz. ¿No está seguro de cómo realizar la extracción? Descargue e instale 7-Zip, haga clic con el botón derecho en el archivo de imagen **.xz** y seleccione 7-Zip &gt; Extraer aquí.

1. Copie los tres archivos siguientes en la carpeta que contiene la herramienta UUU:
    - Se ha extraído el archivo pe101-uefi-***&lt;número de versión&gt;***.raw (del paso 2).
    - emmc_full.txt (del paso 1).
    - fast-hab-fw.raw (del paso 1).
 
1. Conecte el kit de desarrollo.
1. [Conexión al kit de desarrollo a través de SSH](./how-to-ssh-into-percept-dk.md)
1. Abra un símbolo del sistema de Windows (Inicio &gt; cmd) o un terminal de Linux y vaya a la carpeta donde se almacenan los archivos de actualización. Ejecute el comando siguiente para iniciar la actualización:
    - Windows: ```uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    - Linux: ```sudo ./uuu -b emmc_full.txt fast-hab-fw.raw pe101-uefi-<version number>.raw```
    
Después de ejecutar estos comandos, es posible que vea un mensaje que indique "Esperando al dispositivo..." en el símbolo del sistema. Esto es normal y debe continuar en el paso siguiente.
    
1. Conecte la placa base del kit de desarrollo al equipo host mediante un cable USB. Conéctese siempre desde el puerto USB-C de las placas base al puerto USB-C o USB-A del equipo host (el cable de USB-C a USB-A se vende por separado), en función de los puertos que estén disponibles. 
 
1. En el terminal SSH/PuTTY, escriba los siguientes comandos para establecer el kit de desarrollo en modo USB y, a continuación, reinicie el kit de desarrollo.
    - ```flagutil    -wBfRequestUsbFlash    -v1```
    - ```reboot -f```
 
1. Es posible que aparezca una indicación de que el equipo host reconoce el dispositivo y que el proceso de actualización se inicia automáticamente. Vuelva al símbolo del sistema para ver el estado. El proceso tardará hasta diez minutos y, cuando la actualización se realice correctamente, verá un mensaje que indica "éxito 1 error 0".
 
1. Una vez finalizada la actualización, desconecte la placa base. Desconecte el cable USB del equipo.  Vuelva a conectar el módulo Azure Percept Vision a la placa base con el cable USB.

1. Conecte de nuevo la placa base.

## <a name="next-steps"></a>Pasos siguientes

El kit de desarrollo se ha actualizado correctamente. Puede continuar con el desarrollo y el funcionamiento mediante el kit de desarrollo.