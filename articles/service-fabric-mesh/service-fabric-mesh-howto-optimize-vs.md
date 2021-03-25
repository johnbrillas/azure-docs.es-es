---
title: Optimización de Visual Studio para Azure Service Fabric Mesh
description: En este artículo se muestra cómo optimizar el rendimiento de Visual Studio para los proyectos de Service Fabric Mesh para que la primera ejecución de depuración (F5) sea mucho más rápida.
author: georgewallace
ms.author: gwallace
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: e676286dca852c94f94e8bd8ff2ab73ee92b1412
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "99625759"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Optimización del rendimiento de Visual Studio para proyectos Service Fabric Mesh

> [!IMPORTANT]
> Se ha retirado la versión preliminar de Azure Service Fabric Mesh. Ya no se permitirán nuevas implementaciones a través de la API de Service Fabric Mesh. La compatibilidad con las implementaciones existentes continuará hasta el 28 de abril de 2021.
> 
> Para obtener más información, consulte [Retirada de la versión preliminar de Azure Service Fabric Mesh](https://azure.microsoft.com/updates/azure-service-fabric-mesh-preview-retirement/).

En este artículo se muestra cómo optimizar el rendimiento de Visual Studio para los proyectos de Service Fabric Mesh para que la primera ejecución de depuración (F5) sea mucho más rápida.  

## <a name="change-visual-studio-settings"></a>Cambio de la configuración de Visual Studio
 
En Visual Studio, en **Tools** > **Options**  > **Service Fabric Mesh Tools** > **General** (Herramientas > Opciones > Herramientas de Service Fabric Mesh), puede ajustar la siguiente configuración:

- **Extraer las imágenes de Docker necesarias al abrir el proyecto** hace que la primera ejecución de depuración (F5) sea más rápida al iniciar el proceso de descarga de imágenes mientras el proyecto se carga.  
- **Deploy application on project open** (Implementar aplicación al abrir el proyecto) puede hacer que la primera ejecución de depuración (F5) sea más rápida al iniciar el proceso de implementación una vez abierto el proyecto.  
- **Remove application on project close** (Quitar la aplicación al cerrar el proyecto) reclama los recursos (CPU, RAM) asignados a la aplicación mediante la eliminación de la aplicación de Mesh cuando se cierra el proyecto.  

Cuando ve mensajes en la ventana de salida de Service Fabric Tools que indican que Visual Studio "extrae imágenes", "prepara" o "quita la aplicación", se refiere a la configuración anterior. Puede desactivar esta configuración.

## <a name="next-steps"></a>Pasos siguientes

Lea el tutorial [para depurar una aplicación Mesh](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md)