---
title: 'Servicios de datos habilitados para Azure Arc: problemas conocidos'
description: Problemas conocidos más recientes
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
ms.date: 03/02/2021
ms.topic: conceptual
ms.openlocfilehash: 8100d9e12f107e0c4598876c46453b46c6ee4d0e
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "102122007"
---
# <a name="known-issues---azure-arc-enabled-data-services-preview"></a>Problemas conocidos: Servicios de datos habilitados para Azure Arc (versión preliminar)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="february-2021"></a>Febrero de 2021

- El modo de clúster conectado está deshabilitado

## <a name="introduced-prior-to-february-2021"></a>Incorporado antes de febrero de 2021

### <a name="data-controller"></a>Controlador de datos

- No se admite en la versión 1.19.x de Kubernetes en Azure Kubernetes Service (AKS).
- No se admite `containerd` en Kubernetes 1.19.
- El recurso de controlador de datos en Azure es actualmente un recurso de Azure. Las actualizaciones como la eliminación no se propagan de vuelta al clúster de Kubernetes.
- Los nombres de instancia no pueden tener más de 13 caracteres
- No hay actualización local para el controlador de datos de Azure Arc ni las instancias de base de datos.
- Las imágenes de contenedor de los servicios de datos habilitados para Arc no están firmadas.  Es posible que deba configurar los nodos de Kubernetes para permitir la extracción de imágenes de contenedor sin firmar.  Por ejemplo, si usa Docker como entorno de ejecución del contenedor, puede establecer la variable de entorno DOCKER_CONTENT_TRUST=0 y reiniciar.  Otros entornos de ejecución de contenedores tienen opciones similares, como ocurre con [OpenShift](https://docs.openshift.com/container-platform/4.5/openshift_images/image-configuration.html#images-configuration-file_image-configuration).
- No se pueden crear instancias de SQL Managed Instance ni grupos de servidores de Hiperescala de PostgreSQL habilitada para Azure Arc desde Azure Portal.
- Por ahora, si usa NFS, debe establecer `allowRunAsRoot` en `true` en el archivo del perfil de implementación antes de crear el controlador de datos de Azure Arc.
- Solo autenticación de inicio de sesión de SQL y PostgreSQL.  No se admiten Azure Active Directory ni Active Directory.
- La creación de un controlador de datos en OpenShift requiere restricciones de seguridad relajadas.  Para obtener información detallada, consulte la documentación.
- No se admite la actualización a una versión de Kubernetes más reciente si usa el motor de Azure Kubernetes Service (AKS) en Azure Stack Hub con el controlador de datos de Azure Arc e instancias de base de datos. Desinstale el controlador de datos de Azure Arc y todas las instancias de base de datos antes de actualizar el clúster de Kubernetes.
- Los clústeres de AKS que abarcan [varias zonas de disponibilidad](../../aks/availability-zones.md) no se admiten actualmente para los servicios de datos habilitados para Azure Arc. Para evitar este problema, al crear el clúster de AKS en Azure Portal, si selecciona una región en la que hay zonas disponibles, desactive todas las zonas del control de selección. Consulte la siguiente imagen:

   :::image type="content" source="media/release-notes/aks-zone-selector.png" alt-text="Desactive las casillas de cada zona para no especificar ninguna.":::

### <a name="postgresql"></a>PostgreSQL

- Hiperescala de PostgreSQL habilitada para Azure Arc devuelve un mensaje de error inexacto cuando no se puede restaurar al punto relativo en el tiempo que se indica. Por ejemplo, si ha especificado una restauración a un momento dado anterior al contenido de las copias de seguridad, se produce un error en la restauración con un mensaje de error como: `ERROR: (404). Reason: Not found. HTTP response body: {"code":404, "internalStatus":"NOT_FOUND", "reason":"Failed to restore backup for server...}`
Cuando esto suceda, reinicie el comando después de indicar un punto en el tiempo que se encuentre dentro del rango de fechas para las que tiene copias de seguridad. Para determinar este rango, enumere las copias de seguridad y examine las fechas en las que se han realizado.
- La restauración a un momento dado solo se admite en grupos de servidores. El servidor de destino de una operación de restauración a un momento dado no puede ser el servidor desde el que se ha realizado la copia de seguridad. Debe ser otro grupo de servidores. Pero se admite la restauración completa en el mismo grupo de servidores.
- Al realizar una restauración completa se necesita un identificador de copia de seguridad. De forma predeterminada, si no indica un identificador de copia de seguridad, se usará la última copia de seguridad. Esto no funciona en esta versión.

## <a name="next-steps"></a>Pasos siguientes

> **¿Solo desea realizar algunas pruebas?**  
> Empiece a trabajar rápidamente con el [Inicio rápido de Azure Arc](https://azurearcjumpstart.io/azure_arc_jumpstart/azure_arc_data/) en AKS, AWS Elastic Kubernetes Service (EKS), Google Cloud Kubernetes Engine (GKE) o en una máquina virtual de Azure.

- [Instalación de las herramientas de cliente](install-client-tools.md)
- [Creación del controlador de datos de Azure Arc](create-data-controller.md) (requiere primero la instalación de las herramientas de cliente).
- [Creación de una instancia administrada de Azure SQL en Azure Arc](create-sql-managed-instance.md) (requiere primero la creación de un controlador de datos de Azure Arc).
- [Creación de un grupo de servidores de Hiperescala de Azure Database for PostgreSQL en Azure Arc](create-postgresql-hyperscale-server-group.md) (requiere primero la creación de un controlador de datos de Azure Arc).
- [Proveedores de recursos para servicios de Azure](../../azure-resource-manager/management/azure-services-resource-providers.md)
- [Notas de la versión: servicios de datos habilitados para Azure Arc (versión preliminar)](release-notes.md)
