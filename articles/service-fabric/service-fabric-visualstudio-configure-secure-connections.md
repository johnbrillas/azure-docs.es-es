---
title: Configuración de conexiones de clúster de Azure Service Fabric seguras
description: Obtenga información sobre cómo usar Visual Studio para configurar conexiones seguras que son compatibles con el clúster de Azure Service Fabric.
author: cawaMS
ms.topic: conceptual
ms.date: 8/04/2017
ms.author: cawa
ms.openlocfilehash: d4d6b781d97d481793e69cf2ca97cca5b93ce432
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "96008538"
---
# <a name="configure-secure-connections-to-a-service-fabric-cluster-from-visual-studio"></a>Configuración de las conexiones seguras en un clúster de Service Fabric desde Visual Studio
Obtenga información sobre cómo usar Visual Studio para acceder de forma segura a un clúster de Azure Service Fabric con directivas de control de acceso configuradas.

## <a name="cluster-connection-types"></a>Tipos de conexión del clúster
El clúster de Azure Service Fabric admite dos tipos de conexiones: conexiones **no seguras** y **basadas en certificados x509**. (En los clústeres de Service Fabric hospedados localmente también se admiten las autenticaciones de **Windows** y **dSTS**). Tendrá que configurar el tipo de conexión del clúster cuando se cree el clúster. Una vez creado, el tipo de conexión no se puede cambiar.

Las herramientas de Visual Studio Service Fabric admiten todos los tipos de autenticación para conectarse a un clúster para la publicación. Consulte [Configuración de un clúster de Service Fabric en Azure Portal](service-fabric-cluster-creation-via-portal.md) para obtener instrucciones sobre cómo configurar un clúster de Service Fabric seguro.

## <a name="configure-cluster-connections-in-publish-profiles"></a>Configuración de las conexiones del clúster en perfiles de publicación
Si publica un proyecto de Service Fabric desde Visual Studio, use el cuadro de diálogo **Publicar aplicación de Service Fabric** para elegir un clúster de Azure Service Fabric. En **Punto de conexión**, seleccione un clúster actual en su suscripción.

![El cuadro de diálogo **Publicar aplicación de Service Fabric** se usa para configurar una conexión de Service Fabric.][publishdialog]

El cuadro de diálogo **Publicación de la aplicación de Service Fabric** valida automáticamente la conexión del clúster. Si se le pide, inicie sesión en su cuenta de Azure. Si la validación es correcta, significa que el sistema tiene instalados los certificados correctos para conectarse al clúster de forma segura, o el clúster no es seguro. Los errores de validación pueden surgir debido a problemas de red o si el sistema no se ha configurado correctamente para conectarse a un clúster seguro.

![El cuadro de diálogo **Publicación de la aplicación de Service Fabric** valida una conexión de clúster de Service Fabric existente y configurada correctamente.][selectsfcluster]

### <a name="to-connect-to-a-secure-cluster"></a>Para conectarse a un clúster seguro
1. Asegúrese de que puede acceder a uno de los certificados de cliente en el que el clúster de destino puede confiar. Normalmente, el certificado se comparte como un archivo de intercambio de información personal (.pfx, Personal Information Exchange). Consulte [Configuración de un clúster de Service Fabric desde Azure Portal](service-fabric-cluster-creation-via-portal.md) para ver cómo configurar el servidor y conceder acceso a un cliente.
2. Instale el certificado de confianza. Para ello, haga doble clic en el archivo .pfx o use el script de PowerShell Import-PfxCertificate para importar los certificados. Instale el certificado en la ubicación **Cert:\LocalMachine\My**. Acepte toda la configuración predeterminada cuando importe el certificado.
3. Elija el comando **Publicar...** en el menú contextual del proyecto para abrir el cuadro de diálogo **Publicar aplicación de Azure** y, a continuación, seleccione el clúster de destino. La herramienta resuelve automáticamente la conexión y guarda los parámetros de conexión segura en el perfil de publicación.
4. Opcional: puede editar el perfil de publicación para especificar una conexión de clúster segura.
   
   Dado que está editando manualmente el archivo XML de perfil de publicación para especificar la información del certificado, asegúrese de anotar el nombre de almacén de certificados, ubicación de almacén y huella digital de certificado. Necesitará proporcionar estos valores para la ubicación del almacén y el nombre del almacén del certificado. Consulte [Cómo: Procedimiento para recuperar la huella digital de un certificado](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric) para obtener más información.
   
   Puede usar los parámetros *ClusterConnectionParameters* para especificar los parámetros de PowerShell que se van a usar cuando se conecte al clúster de Service Fabric. Los parámetros válidos son los aceptados por el cmdlet Connect-ServiceFabricCluster. Consulte [Conexión de ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster) para obtener una lista de parámetros disponibles.
   
   Si va a publicar en un clúster remoto, deberá especificar los parámetros adecuados para ese clúster concreto. A continuación se proporciona un ejemplo la conexión a un clúster no seguro:
   
   `<ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000" />`
   
   Este es un ejemplo para conectarse a un clúster seguro basado en el certificado x509:
   
   ```xml
   <ClusterConnectionParameters
   ConnectionEndpoint="mycluster.westus.cloudapp.azure.com:19000"
   X509Credential="true"
   ServerCertThumbprint="0123456789012345678901234567890123456789"
   FindType="FindByThumbprint"
   FindValue="9876543210987654321098765432109876543210"
   StoreLocation="CurrentUser"
   StoreName="My" />
   ```
5. Edite las otras opciones de configuración necesarias, como los parámetros de actualización y la ubicación del archivo de parámetros de aplicaciones, y luego publique la aplicación desde el cuadro de diálogo **Publicar aplicación de Service Fabric** en Visual Studio.

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre el acceso a los clústeres de Service Fabric, consulte [Visualización del clúster mediante el Explorador de Service Fabric](service-fabric-visualizing-your-cluster.md).

<!--Image references-->
[publishdialog]:./media/service-fabric-visualstudio-configure-secure-connections/publishdialog.png
[selectsfcluster]:./media/service-fabric-visualstudio-configure-secure-connections/selectsfcluster.png
