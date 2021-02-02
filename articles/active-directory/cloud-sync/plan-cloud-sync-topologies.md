---
title: Topologías y escenarios admitidos para la sincronización en la nube de Azure AD Connect
description: Obtenga información sobre diversas topologías locales y de Azure Active Directory (Azure AD) que usan la sincronización en la nube de Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 02/26/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fd14ed8d149cdc5296229c52ceb74afb2ce7b23
ms.sourcegitcommit: 8a74ab1beba4522367aef8cb39c92c1147d5ec13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98612723"
---
# <a name="azure-ad-connect-cloud-sync-supported-topologies-and-scenarios"></a>Topologías y escenarios admitidos para la sincronización en la nube de Azure AD Connect
En este artículo se describen diversas topologías locales y de Azure Active Directory (Azure AD) que usan la sincronización en la nube de Azure AD Connect. En este artículo solo se incluyen las configuraciones y los escenarios admitidos.

> [!IMPORTANT]
> Microsoft no admite la modificación ni el funcionamiento de la sincronización en la nube de Azure AD Connect con configuraciones ni acciones distintas a estas que se documentan formalmente. Cualquiera de estas configuraciones o acciones pueden provocar un estado incoherente o incompatible de sincronización en la nube de Azure AD Connect. Como resultado, Microsoft no ofrece soporte técnico para estas implementaciones.

## <a name="things-to-remember-about-all-scenarios-and-topologies"></a>Aspectos que recordar sobre todos los escenarios y topologías
A continuación se muestra una lista con información que se debe tener en cuenta al seleccionar una solución.

- Los usuarios y grupos se deben identificar de forma única en todos los bosques.
- No se produce coincidencia entre bosques con la sincronización en la nube.
- Un usuario o grupo solo se debe representar una vez en todos los bosques.
- El delimitador de origen de los objetos se selecciona automáticamente.  Usa ms-DS-ConsistencyGuid si está presente; de lo contrario, se usa ObjectGUID.
- No se puede cambiar el atributo que se usa para el delimitador de origen.

## <a name="single-forest-single-azure-ad-tenant"></a>Un único bosque, un único inquilino de Azure AD
![Diagrama que muestra la topología para un único bosque y un solo inquilino.](media/tutorial-single-forest/diagram-2.png)

La topología más sencilla es un único bosque local, con uno o varios dominios y un único inquilino de Azure AD.  Para ver un ejemplo de este escenario, consulte [Tutorial: Un único bosque con un único inquilino de Azure AD](tutorial-single-forest.md).


## <a name="multi-forest-single-azure-ad-tenant"></a>Varios bosques, un único inquilino de Azure AD
![Topología para varios bosques y un solo inquilino](media/plan-cloud-provisioning-topologies/multi-forest-2.png)

Una topología común es la de varios bosques de AD, con uno o varios dominios y un único inquilino de Azure AD.  

## <a name="existing-forest-with-azure-ad-connect-new-forest-with-cloud-provisioning"></a>Bosque existente con Azure AD Connect, nuevo bosque con aprovisionamiento en la nube
![Diagrama que muestra la topología para un bosque existente y otro nuevo.](media/tutorial-existing-forest/existing-forest-new-forest-2.png)

Este escenario es una topología similar a la del escenario de varios bosques; sin embargo, en este caso hay un entorno de Azure AD Connect existente y, a continuación, se incorpora un nuevo bosque con sincronización en la nube de Azure AD Connect.  Para ver un ejemplo de este escenario, consulte [Tutorial: Un bosque existente con un único inquilino de Azure AD](tutorial-existing-forest.md).

## <a name="piloting-azure-ad-connect-cloud-sync-in-an-existing-hybrid-ad-forest"></a>Realización de una prueba piloto de sincronización en la nube de Azure AD Connect en un bosque de AD híbrido existente
![Topología para un único bosque y un solo inquilino](media/tutorial-migrate-aadc-aadccp/diagram-2.png) El escenario de prueba piloto implica la existencia de Azure AD Connect y sincronización en la nube de Azure AD Connect en el mismo bosque y el establecimiento del ámbito de usuarios y grupos en consecuencia. NOTA:  Un objeto debe estar en el ámbito solo en una de las herramientas. 

Para ver un ejemplo de este escenario, consulte [Tutorial: Sincronización piloto en la nube para un bosque de Azure AD Connect sincronizado existente](tutorial-pilot-aadc-aadccp.md)



## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)

