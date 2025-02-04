---
title: Configuración de un firewall de dirección IP para el servicio Azure Cognitive Search
titleSuffix: Azure Cognitive Search
description: Configure directivas de control de direcciones IP para restringir el acceso al servicio de Azure Cognitive Search.
manager: nitinme
author: markheff
ms.author: maheff
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/16/2021
ms.openlocfilehash: de34c2921c7829cb6d7e7354a1ebcff44271efd3
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "100545554"
---
# <a name="configure-ip-firewall-for-azure-cognitive-search"></a>Configuración de un firewall de dirección IP para Azure Cognitive Search

Azure Cognitive Search admite las reglas de IP para la compatibilidad con el firewall de entrada. Este modelo proporciona un nivel adicional de seguridad para el servicio de búsqueda, que es similar a las reglas de IP que encontrará en un grupo de seguridad de red virtual de Azure. Con estas reglas de IP, puede configurar su servicio de búsqueda para que sea accesible solo desde un conjunto aprobado de máquinas o servicios en la nube. Para obtener acceso a los datos almacenados en el servicio de búsqueda desde estos conjuntos aprobados de máquinas y servicios, seguirá siendo necesario que el autor de la llamada presente un token de autorización válido.

Puede definir reglas IP en Azure Portal, como se describe en este artículo. También puede usar la [API de REST de administración, versión 2020-03-13](/rest/api/searchmanagement/), [Azure PowerShell](/powershell/module/az.search) o la [CLI de Azure](/cli/azure/search).

## <a name="configure-an-ip-firewall-using-the-azure-portal"></a><a id="configure-ip-policy"></a> Configuración de un firewall de dirección IP mediante Azure Portal

Para establecer la directiva de control de acceso de direcciones IP en Azure Portal, vaya a la página del servicio de Azure Cognitive Search y seleccione **Redes** en el menú de navegación. La conectividad de red del punto de conexión debe ser **pública**. Si la conectividad está establecida como **privada**, solo puede obtener acceso al servicio de búsqueda a través de un punto de conexión privado.

![Captura de pantalla que muestra cómo configurar un firewall de dirección IP en Azure Portal](./media/service-configure-firewall/azure-portal-firewall.png)

Azure Portal proporciona la capacidad para especificar direcciones IP e intervalos de direcciones IP en formato CIDR. Un ejemplo de notación CIDR es 8.8.8.0/24, que representa las direcciones IP que van de 8.8.8.0 a 8.8.8.255.

> [!NOTE]
> Después de habilitar la directiva de control de acceso de direcciones IP en el servicio de Azure Cognitive Search, se rechazan todas las solicitudes al plano de datos desde máquinas que estén fuera de la lista permitida de rangos de direcciones IP. Cuando se configuran las reglas de IP, algunas características de Azure Portal se deshabilitan. Podrá ver y administrar información de nivel de servicio, pero, por motivos de seguridad, se ha restringido el acceso del portal a los datos del índice y de los distintos componentes de este servicio, como índice, indizador y definiciones del conjunto de aptitudes. Como alternativa al portal, puede usar la [extensión de VS Code](https://aka.ms/vscode-search) para interactuar con los distintos componentes del servicio.

### <a name="requests-from-your-current-ip"></a>Solicitudes desde la dirección IP actual

Para simplificar el desarrollo, Azure Portal le ayuda a identificar y agregar la dirección IP de su equipo cliente a la lista de permitidos. Las aplicaciones que se ejecutan en la máquina pueden obtener acceso al servicio de Azure Cognitive Search.

El portal detecta automáticamente la dirección IP de cliente. Puede que sea la dirección IP de cliente de la máquina o la de la puerta de enlace de red. Asegúrese de quitar esta dirección IP antes de llevar la carga de trabajo a producción.

Para agregar la dirección IP actual a la lista de direcciones IP, consulte **Incorporación de la dirección IP de cliente**. Después, seleccione **Guardar**.

![Captura de pantalla que muestra cómo configurar un firewall de direcciones IP para la dirección IP actual](./media/service-configure-firewall/enable-current-ip.png)

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>Solución de problemas con una directiva de control de acceso por IP

Puede solucionar problemas con una directiva de control de acceso por IP mediante las siguientes opciones:

### <a name="azure-portal"></a>Azure Portal

Si habilita la directiva de control de acceso de direcciones IP en el servicio de Azure Cognitive Search, se bloquean todas las solicitudes desde máquinas que estén fuera de la lista permitida de rangos de direcciones IP, incluyendo Azure Portal.  Podrá ver y administrar información de nivel de servicio, pero, por motivos de seguridad, se ha restringido el acceso del portal a los datos del índice y de los distintos componentes de este servicio, como índice, indizador y definiciones del conjunto de aptitudes. 

### <a name="sdks"></a>SDK

Cuando tenga acceso al servicio de Azure Cognitive Search mediante el SDK desde máquinas que no están en la lista de elementos permitidos, se devolverá una respuesta genérica de tipo **403 Prohibido** sin más detalles. Compruebe la lista de direcciones IP permitidas para la cuenta y asegúrese de que se aplique la configuración de actualización correcta para el servicio de búsqueda.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el acceso al servicio de búsqueda a través de Private Link, consulte el siguiente artículo:

* [Creación de un punto de conexión privado para una conexión segura a Azure Cognitive Search](service-create-private-endpoint.md)