---
title: 'Análisis detallado de la sincronización en la nube de Azure AD Connect: cómo funciona'
description: En este tema se proporciona información detallada sobre cómo funciona la sincronización en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0e1079ab4a523fbbc99c1e9190aef960b3f0723f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98612836"
---
# <a name="cloud-sync-deep-dive---how-it-works"></a>Análisis detallado de la sincronización en la nube: cómo funciona

## <a name="overview-of-components"></a>Información general de los componentes

![Funcionamiento](media/concept-how-it-works/how-1.png)

La sincronización en la nube se basa en los servicios de Azure AD y tiene dos componentes clave:

- **Agente de aprovisionamiento**: el agente de aprovisionamiento en la nube de Azure AD Connect es el mismo que el de aprovisionamiento de entrada de Workday y se basa en la misma tecnología de servidor que el proxy de aplicación y la autenticación de paso a través. Solo se necesita la conexión de salida y los agentes se actualizan automáticamente. 
- **Servicio de aprovisionamiento**: el mismo servicio de aprovisionamiento que el aprovisionamiento saliente y el aprovisionamiento de entrada de Workday que usa un modelo basado en un programador. En el caso de la sincronización en la nube, los cambios se aprovisionan cada dos minutos.


## <a name="initial-setup"></a>Configuración inicial
Durante la configuración inicial, tienen lugar algunas actividades que hacen posible la sincronización de la nube.  Dichos componentes son: 

- **Durante la instalación del agente**: se configura el agente de los dominios de AD desde los que quiere realizar el aprovisionamiento.  Esta configuración registra los dominios en el servicio de identidad híbrida y establece una conexión saliente con el bus de servicio que escucha las solicitudes.
- **Al habilitar el aprovisionamiento**: seleccione el dominio de AD y habilite el aprovisionamiento, que se ejecuta cada dos minutos. Opcionalmente, puede anular la selección de la sincronización del hash de contraseña y definir el correo electrónico de notificación. También puede administrar la transformación de atributos mediante instancias de Microsoft Graph API.


## <a name="agent-installation"></a>Instalación del agente
A continuación, se ofrece un tutorial sobre lo que ocurre cuando se instala el agente de aprovisionamiento en la nube.

- En primer lugar, el instalador instala los archivos binarios del agente y el servicio del agente que se ejecuta con la cuenta de servicio virtual (NETWORK SERVICE\AADProvisioningAgent).  Una cuenta de servicio virtual es un tipo especial de cuenta que no tiene contraseña y está administrada por Windows.
- A continuación, el instalador inicia el asistente.
- El asistente solicitará las credenciales de Azure AD, se autenticará y recuperará un token.
- Luego, el asistente solicita las credenciales actuales de administrador de dominio de la máquina.
- Con estas credenciales, se crea la cuenta de servicio administrada general del agente (GMSA) para este dominio o, si ya existe, se busca y se reutiliza.
- El servicio del agente se ha reconfigurado para ejecutarse con la cuenta GMSA.
- El asistente solicita ahora la configuración de dominio junto con la cuenta de administrador de empresa (EA) o administrador de dominio (DA) de cada dominio que quiera que atienda el agente.
- La cuenta GMSA se actualiza entonces con los permisos que permiten el acceso a cada dominio especificado anteriormente.
- Luego, el asistente desencadena el registro del agente.
- El agente crea un certificado y, con el token de Azure AD, se registra a sí mismo y registra el certificado con el servicio de registro del servicio de identidad híbrida (HIS).
- El asistente desencadena una llamada a AgentResourceGrouping. Esta llamada a su servicio de administración de HIS es para asignar el agente a uno o varios dominios de AD en la configuración de HIS.
- Ahora el asistente reinicia el servicio de agente.
- El agente llama a un servicio de arranque durante el reinicio (y después cada 10 minutos) para comprobar si hay actualizaciones de configuración.  El servicio de arranque valida la identidad del agente.  También actualiza la hora del último arranque.  Esto es importante porque, si los agentes no arrancan, no reciben puntos de conexión de Service Bus actualizados y es posible que no puedan recibir solicitudes. 


## <a name="what-is-system-for-cross-domain-identity-management-scim"></a>¿Qué es el sistema de administración de identidades entre dominios (SCIM)?

La [especificación SCIM](https://tools.ietf.org/html/draft-scim-core-schema-01) es un estándar que se usa para automatizar el intercambio de información de identidad de usuarios o grupos entre dominios de identidad, como Azure AD. SCIM se está convirtiendo en el estándar de facto para aprovisionar y, cuando se usa junto con estándares de federación como SAML u OpenID Connect, ofrece a los administradores una solución de un extremo a otro basada en estándares para la administración del acceso.

El agente de aprovisionamiento en la nube de Azure AD Connect usa SCIM con Azure AD para aprovisionar y desaprovisionar usuarios y grupos.

## <a name="synchronization-flow"></a>Flujo de sincronización
![aprovisionamiento](media/concept-how-it-works/provisioning-4.png) Cuando haya instalado el agente y habilitado el aprovisionamiento, se produce el siguiente flujo.

1.  Una vez configurado, el servicio de aprovisionamiento de Azure AD llama al servicio híbrido Azure AD para agregar una solicitud a Service Bus. El agente mantiene constantemente una conexión saliente con la instancia de Service Bus que escucha las solicitudes y recopila la solicitud de System for Cross-domain Identity Management (SCIM) inmediatamente. 
2.  El agente divide la solicitud en consultas independientes según el tipo de objeto. 
3.  AD devuelve el resultado al agente y el agente filtra estos datos antes de enviarlos a Azure AD.  
4.  El agente devuelve la respuesta de SCIM a Azure AD.  Esta respuesta se basa en el filtrado que se produjo dentro del agente.  El agente usa la determinación del ámbito para filtrar los resultados. 
5.  El servicio de aprovisionamiento escribe los cambios en Azure AD.
6. Si se trata de una sincronización diferencial en lugar de una sincronización completa, se usa la cookie o la marca de agua. En adelante, las nuevas consultas recibirán los cambios de esa cookie o marca de agua.

## <a name="supported-scenarios"></a>Escenarios admitidos:
Se admiten los siguientes escenarios de sincronización en la nube.


- **Cliente híbrido existente con un nuevo bosque**: la sincronización de Azure AD Connect se usa con los bosques principales. La sincronización en la nube se usa para el aprovisionamiento desde un bosque de AD (incluso si está desconectado). Para más información, consulte el tutorial [aquí](tutorial-existing-forest.md).

 ![Híbrido existente](media/tutorial-existing-forest/existing-forest-new-forest-2.png)
- **Nuevo cliente híbrido**:      no se utiliza la sincronización de Azure AD Connect. La sincronización en la nube se usa para el aprovisionamiento desde un bosque de AD.  Para más información, consulte el tutorial [aquí](tutorial-single-forest.md).
 
 ![Nuevos clientes](media/tutorial-single-forest/diagram-2.png)

- **Cliente híbrido existente**: la sincronización de Azure AD Connect se usa con los bosques principales. Se pone a prueba la sincronización en la nube para un pequeño conjunto de usuarios de los bosques principales [aquí](tutorial-existing-forest.md).

 ![Piloto existente](media/tutorial-migrate-aadc-aadccp/diagram-2.png)

Para más información, consulte [Topologías admitidas](plan-cloud-sync-topologies.md).



## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es la sincronización en la nube de Azure AD Connect?](what-is-cloud-sync.md)
