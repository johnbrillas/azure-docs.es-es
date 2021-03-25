---
title: Creación y carga de un VHD de Flatcar Container Linux para su uso en Azure
description: Aprenda a crear y cargar un VHD que contiene el sistema operativo Flatcar Container Linux.
author: marga-kinvolk
ms.author: danis
ms.service: virtual-machines
ms.collection: linux
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/16/2020
ms.reviewer: cynthn
ms.openlocfilehash: 5d8be9493b7a312270301e3520f301f797fe2167
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2021
ms.locfileid: "102565298"
---
# <a name="using-a-prebuilt-flatcar-image-for-azure"></a>Uso de una imagen de Flatcar precompilada para Azure

Puede descargar imágenes de disco duro virtual de Azure precompiladas de Flatcar Container Linux para cada uno de los canales de Flatcar admitidos:

- [stable](https://stable.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [beta](https://beta.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [alpha](https://alpha.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)
- [edge](https://edge.release.flatcar-linux.net/amd64-usr/current/flatcar_production_azure_image.vhd.bz2)

Esta imagen ya está completamente configurada y optimizada para ejecutarse en Azure. Solo tiene que descomprimirla.

## <a name="building-your-own-flatcar-based-virtual-machine-for-azure"></a>Creación de su propia máquina virtual basada en Flatcar para Azure

También puede optar por crear su propia imagen de Flatcar Container Linux.

En cualquier máquina basada en Linux, siga las instrucciones que se detallan en la [guía del SDK para desarrolladores de Flatcar Container Linux](https://docs.flatcar-linux.org/os/sdk-modifying-flatcar/). Al ejecutar el script `image_to_vm.sh`, asegúrese de pasar el parámetro `--format=azure` para crear un disco duro virtual de Azure.

## <a name="next-steps"></a>Pasos siguientes

Una vez que disponga del archivo .vhd, puede usar el archivo resultante para crear nuevas máquinas virtuales en Azure. Si es la primera vez que carga un archivo .vhd en Azure, vea [Creación de una VM Linux a partir de un disco personalizado](upload-vhd.md#option-1-upload-a-vhd).
