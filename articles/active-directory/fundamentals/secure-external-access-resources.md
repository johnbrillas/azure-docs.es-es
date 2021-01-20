---
title: Protección de la colaboración externa en Azure Active Directory
description: Una guía para arquitectos y administradores de TI sobre cómo proteger el acceso externo a recursos internos
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc8ec364380fc5cb9f6e33a29cd8dac96ea1fb69
ms.sourcegitcommit: d59abc5bfad604909a107d05c5dc1b9a193214a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98219834"
---
# <a name="securing-external-collaboration-in-azure-active-directory-and-microsoft-365"></a>Protección de la colaboración externa en Azure Active Directory y Microsoft 365

La colaboración segura con asociados externos garantiza que estos asociados tengan acceso a los recursos internos apropiados por el período de tiempo adecuado. Mediante un enfoque global de gobernanza, puede reducir los riesgos de seguridad, lograr los objetivos de cumplimiento y cerciorarse de qué usuarios tienen acceso.

La colaboración no regulada genera confusión con respecto a la propiedad del acceso y puede dar lugar a la exposición de los recursos confidenciales. Adoptar un entorno de colaboración seguro y regulado contribuye a garantizar que haya líneas claras de propiedad y responsabilidad en relación con el acceso de los usuarios externos. Esto incluye:

* Administrar las organizaciones externas y los usuarios de estas que tienen acceso a los recursos.

* Asegurarse de que el acceso sea adecuado, se supervise y tenga un límite temporal cuando corresponda.

* Permitir a los propietarios de empresas administrar la colaboración por medio de los mecanismos de protección establecidos por el equipo de TI.

Si debe adherirse a marcos de cumplimiento, la colaboración regulada le permite constatar la pertinencia del acceso.

Microsoft ofrece un completo conjunto de herramientas para un acceso externo seguro.  La colaboración B2B de Azure Active Directory (Azure AD) es una herramienta fundamental para plan de colaboración externo. Azure AD B2B se integra con otras herramientas de Azure AD y de los servicios de Microsoft 365 para proteger y administrar el acceso externo.

Esta documentación se ha concebido para ayudarle a cambiar de un entorno de colaboración externa ad hoc poco regulado a uno más seguro. 

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos sobre cómo proteger el acceso externo a los recursos. Se recomienda realizar las acciones en el orden indicado.


1. [Determinación de la posición de seguridad para el acceso externo](1-secure-access-posture.md)

2. [Detección del estado actual](2-secure-access-current-state.md)

3. [Creación de un plan de gobernanza](3-secure-access-plan.md)

4. [Uso de grupos con fines de seguridad](4-secure-access-groups.md)

5. [Transición a B2B de Azure AD](5-secure-access-b2b.md)

6. [Acceso seguro mediante la administración de derechos](6-secure-access-entitlement-managment.md)

7. [Acceso seguro mediante directivas de acceso condicional](7-secure-access-conditional-access.md)

8. [Acceso seguro mediante etiquetas de confidencialidad](8-secure-access-sensitivity-labels.md)

9. [Acceso seguro a Microsoft Teams, OneDrive y SharePoint](9-secure-access-teams-sharepoint.md)
