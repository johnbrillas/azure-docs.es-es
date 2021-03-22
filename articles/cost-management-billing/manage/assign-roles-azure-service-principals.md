---
title: Asignación de roles a nombres de entidad de seguridad de servicio de Contrato Enterprise de Azure
description: Este artículo le ayuda a asignar roles a nombres de entidad de seguridad de servicio mediante PowerShell y las API REST.
author: bandersmsft
ms.reviewer: ruturajd
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 03/07/2021
ms.author: banders
ms.openlocfilehash: e7f5370e1e387947d196959fef31043ea8f4d3bd
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/09/2021
ms.locfileid: "102508527"
---
# <a name="assign-roles-to-azure-enterprise-agreement-service-principal-names"></a>Asignación de roles a nombres de entidad de seguridad de servicio de Contrato Enterprise de Azure

Puede administrar su inscripción de Contrato Enterprise (EA) en [Azure Enterprise Portal](https://ea.azure.com/). Puede crear roles diferentes para administrar la organización, ver los costos y crear suscripciones. Este artículo le ayuda a automatizar algunas de esas tareas mediante Azure PowerShell y las API REST con nombres de entidad de seguridad de servicio (SPN) de Azure.

Antes de comenzar, asegúrese de que está familiarizado con los siguientes artículos:

- [Administración de los roles del Contrato Enterprise de Azure](understand-ea-roles.md)
- [Inicio de sesión con Azure PowerShell](/powershell/azure/authenticate-azureps)
- [Llamada a las API REST con Postman](/rest/api/azure/#how-to-call-azure-rest-apis-with-postman)

## <a name="create-and-authenticate-your-service-principal"></a>Creación y autenticación de la entidad de servicio

Para automatizar las acciones de EA mediante un SPN, debe crear una aplicación de Azure Active Directory (Azure AD). Esta se puede autenticar de forma automatizada. Consulte los artículos siguientes y siga los pasos que se describen en ellos para crear y autenticar la entidad de servicio.

1. [Creación de una entidad de servicio](../../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal)
2. [Obtención de valores de identificador de inquilino y aplicación para iniciar sesión](../../active-directory/develop/howto-create-service-principal-portal.md#get-tenant-and-app-id-values-for-signing-in)

Esta es una captura de pantalla de ejemplo que muestra el registro de aplicación.

:::image type="content" source="./media/assign-roles-azure-service-principals/register-an-application.png" alt-text="Captura de pantalla que muestra el registro de una aplicación." lightbox="./media/assign-roles-azure-service-principals/register-an-application.png" :::

### <a name="find-your-spn-and-tenant-id"></a>Búsqueda del SPN y el identificador de inquilino

También necesita el identificador de objeto del SPN y el identificador de inquilino de la aplicación. Necesita la información para las operaciones de asignación de permisos en secciones posteriores.

Puede encontrar el identificador de inquilino de la aplicación de Azure AD en la página de información general de la aplicación. Para encontrarlo en Azure Portal, vaya a Azure Active Directory y seleccione **Aplicaciones empresariales**. Busque la aplicación.

:::image type="content" source="./media/assign-roles-azure-service-principals/enterprise-application.png" alt-text="Captura de pantalla que muestra una aplicación empresarial de ejemplo." lightbox="./media/assign-roles-azure-service-principals/enterprise-application.png" :::

Seleccione la aplicación. Este es un ejemplo que muestra el identificador de la aplicación y el identificador de objeto.

:::image type="content" source="./media/assign-roles-azure-service-principals/application-id-object-id.png" alt-text="Captura de pantalla que muestra un identificador de aplicación y un identificador de objeto de una aplicación empresarial." lightbox="./media/assign-roles-azure-service-principals/application-id-object-id.png" :::

Puede encontrar el identificador de inquilino en la página de información general de Microsoft Azure AD.

:::image type="content" source="./media/assign-roles-azure-service-principals/tenant-id.png" alt-text="Captura de pantalla que muestra dónde ver el identificador de inquilino." lightbox="./media/assign-roles-azure-service-principals/tenant-id.png" :::

El identificador de inquilino de la entidad de seguridad también se conoce como identificador de entidad de seguridad, SPN e identificador de objeto en varias ubicaciones. El valor del identificador de inquilino de Azure AD es similar a un GUID con el siguiente formato: `11111111-1111-1111-1111-111111111111`.

## <a name="permissions-that-can-be-assigned-to-the-spn"></a>Permisos que se pueden asignar al SPN

En los pasos siguientes, va a conceder permiso a la aplicación de Azure AD para realizar acciones con un rol de EA. Solo puede asignar los siguientes roles al SPN. El identificador de definición de rol, tal y como se muestra, se usa más adelante en los pasos de asignación.

| Role | Acciones permitidas | Id. de definición de roles |
| --- | --- | --- |
| EnrollmentReader | Puede ver el uso y los cargos en todas las cuentas y suscripciones. Puede ver el saldo de pago por adelantado de Azure (anteriormente llamado compromiso monetario) asociado a la inscripción. | 24f8edb6-1668-4659-b5e2-40bb5f3a7d7e |
| DepartmentReader | Descargar los detalles de uso del departamento que administran. Puede ver el uso y los cargos asociados a su departamento. | db609904-a47f-4794-9be8-9bd86fbffd8a |
| SubscriptionCreator | Crear nuevas suscripciones en el ámbito de cuenta especificado. | a0bcee42-bf30-4d1b-926a-48d21664ef71 |

- Solo un usuario con el rol de escritor de inscripciones puede asignar un rol de lector de inscripciones a un SPN.
- Solo un usuario que tenga un rol de escritor de inscripciones o un rol de escritor de departamento puede asignar un rol de lector de departamento a un SPN.
- Solo un usuario que sea el propietario de la cuenta de inscripción puede asignar un rol de creador de suscripciones a un SPN.

## <a name="assign-enrollment-account-role-permission-to-the-spn"></a>Asignación del permiso de rol de cuenta de inscripción al SPN

Consulte el artículo de la API REST [Asignaciones de roles: Put](/rest/api/billing/2019-10-01-preview/roleassignments/put).

Al leer el artículo, seleccione **¡Pruébelo!** para empezar a usar el SPN.

:::image type="content" source="./media/assign-roles-azure-service-principals/put-try-it.png" alt-text="Captura de pantalla que muestra la opción Pruébelo en el artículo sobre Put." lightbox="./media/assign-roles-azure-service-principals/put-try-it.png" :::

Inicie sesión con su cuenta en el inquilino que tiene acceso a la inscripción a la que desea asignar acceso.

Proporcione los parámetros siguientes como parte de la solicitud de API.

**billingAccountName**

El parámetro es el identificador de la cuenta de facturación. Puede encontrarlo en Azure Portal en la página de información general de Cost Management + Billing.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Captura de pantalla que muestra el identificador de la cuenta de facturación." lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

El parámetro es un GUID único que debe proporcionar. Puede generar un GUID mediante el comando de PowerShell [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid).

O bien, puede usar el sitio web [Generador en línea de GUID y UUID](https://guidgenerator.com/) para generar un GUID único.

**api-version**

Use la versión **2019-10-01-preview**.

El cuerpo de la solicitud tiene código JSON que debe usar.

Use el cuerpo de la solicitud de ejemplo de [Asignaciones de roles: ejemplos de Put](/rest/api/billing/2019-10-01-preview/roleassignments/put#examples).

Hay tres parámetros que debe usar como parte del código JSON.

| Parámetro | Dónde encontrarla |
| --- | --- |
| properties.principalId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | "/providers/Microsoft.Billing/billingAccounts/{BillingAccountName}/billingRoleDefinitions/24f8edb6-1668-4659-b5e2-40bb5f3a7d7e" |

El nombre de la cuenta de facturación es el mismo parámetro que usó en los parámetros de la API. Es el identificador de inscripción que ve en el portal de EA y Azure Portal.

Tenga en cuenta que `24f8edb6-1668-4659-b5e2-40bb5f3a7d7e` es un identificador de definición de rol de facturación de un rol EnrollmentReader.

Seleccione **Ejecutar** para iniciar el comando.

:::image type="content" source="./media/assign-roles-azure-service-principals/roleassignments-put-try-it-run.png" alt-text="Captura de pantalla en la que se muestra una asignación de roles de ejemplo con el botón Pruébelo con información de ejemplo lista para ejecutarse." lightbox="./media/assign-roles-azure-service-principals/roleassignments-put-try-it-run.png" :::

Una respuesta `200 OK` muestra que el SPN se ha agregado correctamente.

Ahora puede usar el SPN (aplicación de Azure AD con el identificador de objeto) para acceder a las API de EA de forma automatizada. El SPN tiene el rol EnrollmentReader.

## <a name="assign-the-department-reader-role-to-the-spn"></a>Asignación del rol de lector de departamento al SPN

Antes de empezar, consulte el artículo de la API REST [Asignaciones de roles del departamento de inscripción: Put](/rest/api/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put).

Al leer el artículo, seleccione **¡Pruébelo!** .

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" alt-text="Captura de pantalla que muestra la opción Pruébelo en el artículo Asignaciones de roles del departamento de inscripción: Put" lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" :::

Inicie sesión con su cuenta en el inquilino que tiene acceso a la inscripción a la que desea asignar acceso.

Proporcione los parámetros siguientes como parte de la solicitud de API.

**billingAccountName**

Es el identificador de la cuenta de facturación. Puede encontrarlo en Azure Portal en la página de información general de Cost Management + Billing.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Captura de pantalla que muestra el identificador de la cuenta de facturación." lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

El parámetro es un GUID único que debe proporcionar. Puede generar un GUID mediante el comando de PowerShell [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid).

O bien, puede usar el sitio web [Generador en línea de GUID y UUID](https://guidgenerator.com/) para generar un GUID único.

**departmentName**

Es el identificador del departamento. Puede ver los identificadores de departamento en Azure Portal. Vaya a Cost Management + Billing > **Departamentos**.

En este ejemplo, usamos el departamento ACE. El identificador del ejemplo es `84819`.

:::image type="content" source="./media/assign-roles-azure-service-principals/department-id.png" alt-text="Captura de pantalla que muestra un identificador de departamento de ejemplo." lightbox="./media/assign-roles-azure-service-principals/department-id.png" :::

**api-version**

Use la versión **2019-10-01-preview**.

El cuerpo de la solicitud tiene código JSON que debe usar.

Use el ejemplo de [Asignaciones de roles del departamento de inscripción: Put](/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put). Hay tres parámetros que debe usar como parte del código JSON.

| Parámetro | Dónde encontrarla |
| --- | --- |
| properties.principalId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | "/providers/Microsoft.Billing/billingAccounts/{BillingAccountName}/billingRoleDefinitions/db609904-a47f-4794-9be8-9bd86fbffd8a" |

El nombre de la cuenta de facturación es el mismo parámetro que usó en los parámetros de la API. Es el identificador de inscripción que ve en el portal de EA y Azure Portal.

El identificador de la definición de rol de facturación `db609904-a47f-4794-9be8-9bd86fbffd8a` es para un lector de departamento.

Seleccione **Ejecutar** para iniciar el comando.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it-run.png" alt-text="Captura de pantalla en la que se muestra una asignación de roles del departamento de inscripción de ejemplo con el botón Pruébelo con información de ejemplo lista para ejecutarse." lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it-run.png" :::

Una respuesta `200 OK` muestra que el SPN se ha agregado correctamente.

Ahora puede usar el SPN (aplicación de Azure AD con el identificador de objeto) para acceder a las API de EA de forma automatizada. El SPN tiene el rol DepartmentReader.

## <a name="assign-the-subscription-creator-role-to-the-spn"></a>Asignación del rol de creador de suscripciones al SPN

Consulte el artículo [Asignaciones de roles de la cuenta de inscripción: Put](/rest/api/billing/2019-10-01-preview/enrollmentaccountroleassignments/put).

Al leer el artículo, seleccione **¡Pruébelo!** para asignar el rol de creador de suscripciones al SPN.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" alt-text="Captura de pantalla que muestra la opción Pruébelo en el artículo Asignaciones de roles de la cuenta de inscripción: Put" lightbox="./media/assign-roles-azure-service-principals/enrollment-department-role-assignments-put-try-it.png" :::

Inicie sesión con su cuenta en el inquilino que tiene acceso a la inscripción a la que desea asignar acceso.

Proporcione los parámetros siguientes como parte de la solicitud de API. Consulte el artículo [Asignaciones de roles de la cuenta de inscripción: Put (Sección Parámetros del identificador URI)](/rest/api/billing/2019-10-01-preview/enrollmentaccountroleassignments/put#uri-parameters).

**billingAccountName**

El parámetro es el identificador de la cuenta de facturación. Puede encontrarlo en Azure Portal en la página de información general de Cost Management + Billing.

:::image type="content" source="./media/assign-roles-azure-service-principals/billing-account-id.png" alt-text="Captura de pantalla que muestra el identificador de la cuenta de facturación." lightbox="./media/assign-roles-azure-service-principals/billing-account-id.png" :::

**billingRoleAssignmentName**

El parámetro es un GUID único que debe proporcionar. Puede generar un GUID mediante el comando de PowerShell [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid).

O bien, puede usar el sitio web [Generador en línea de GUID y UUID](https://guidgenerator.com/) para generar un GUID único.
**enrollmentAccountName**

Este parámetro es el identificador de la cuenta. Busque el identificador de cuenta del nombre de la cuenta en Azure Portal en Cost Management + Billing en el ámbito de inscripción y departamento.

En este ejemplo, se utiliza la cuenta de prueba GTM. El identificador es `196987`.

:::image type="content" source="./media/assign-roles-azure-service-principals/account-id.png" alt-text="Captura de pantalla que muestra el identificador de cuenta." lightbox="./media/assign-roles-azure-service-principals/account-id.png" :::

**api-version**

Use la versión **2019-10-01-preview**.

El cuerpo de la solicitud tiene código JSON que debe usar.

Use el ejemplo de [Asignaciones de roles del departamento de inscripción: Put (Ejemplos)](/rest/api/billing/2019-10-01-preview/enrollmentdepartmentroleassignments/put#putenrollmentdepartmentadministratorroleassignment).

Hay tres parámetros que debe usar como parte del código JSON.

| Parámetro | Dónde encontrarla |
| --- | --- |
| properties.principalId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.principalTenantId | Consulte [Búsqueda del SPN y el identificador de inquilino](#find-your-spn-and-tenant-id). |
| properties.roleDefinitionId | "/providers/Microsoft.Billing/billingAccounts/{BillingAccountID}/enrollmentAccounts/196987/billingRoleDefinitions/a0bcee42-bf30-4d1b-926a-48d21664ef71" |

El nombre de la cuenta de facturación es el mismo parámetro que usó en los parámetros de la API. Es el identificador de inscripción que ve en el portal de EA y Azure Portal.

El identificador de la definición de rol de facturación `a0bcee42-bf30-4d1b-926a-48d21664ef71` es para el rol de creador de suscripciones.

Seleccione **Ejecutar** para iniciar el comando.

:::image type="content" source="./media/assign-roles-azure-service-principals/enrollment-account-role-assignments-put-try-it.png" alt-text="Captura de pantalla que muestra la opción Pruébelo en el artículo Asignaciones de roles de la cuenta de inscripción: Put" lightbox="./media/assign-roles-azure-service-principals/enrollment-account-role-assignments-put-try-it.png" :::

Una respuesta `200 OK` muestra que el SPN se ha agregado correctamente.

Ahora puede usar el SPN (aplicación de Azure AD con el identificador de objeto) para acceder a las API de EA de forma automatizada. El SPN tiene el rol SubscriptionCreator.

## <a name="next-steps"></a>Pasos siguientes

- Más información en [Administración del portal del Contrato Enterprise de Azure](ea-portal-administration.md).