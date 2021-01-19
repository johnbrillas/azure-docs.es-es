---
title: Establecimiento de la vigencia de los tokens
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo establecer la vigencia de los tokens emitidos por la Plataforma de identidad de Microsoft. Obtenga información sobre cómo administrar la directiva predeterminada de una organización, crear una directiva para el inicio de sesión web, crear una directiva para una aplicación nativa que llama a una API web y administrar una directiva avanzada.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: how-to
ms.date: 01/04/2021
ms.author: ryanwi
ms.custom: aaddev, content-perf, FY21Q1
ms.reviewer: hirsin, jlu, annaba
ms.openlocfilehash: 4d6a7150c854ba89c3cd8eacd6b553c4b8e97343
ms.sourcegitcommit: f6f928180504444470af713c32e7df667c17ac20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2021
ms.locfileid: "97963356"
---
# <a name="configure-token-lifetime-policies-preview"></a>Configuración de las directivas de vigencia de tokens (versión preliminar)
Puede especificar la duración de un token de acceso, SAML o identificador emitido por la plataforma de Microsoft Identity. La vigencia de los tokens se puede configurar para todas las aplicaciones de una organización, para una aplicación multiinquilino (multiorganización) o para una entidad de servicio específica de una organización. Para obtener más información, lea [Vigencias de tokens configurables](active-directory-configurable-token-lifetimes.md).

En esta sección, se explica un escenarios de directiva común que le ayudará a imponer nuevas reglas para la vigencia del token. En este ejemplo, aprende a crear una directiva que requerirá que los usuarios se autentiquen con más frecuencia en la aplicación web.

## <a name="get-started"></a>Introducción
Para comenzar, realice uno de los pasos siguientes:

1. Descargue la [versión preliminar pública más reciente del módulo de PowerShell de Azure AD](https://www.powershellgallery.com/packages/AzureADPreview).
1. Ejecute el comando `Connect` para iniciar sesión en la cuenta de administrador de Azure AD. Ejecute este comando cada vez que inicie una nueva sesión.

    ```powershell
    Connect-AzureAD -Confirm
    ```

1. Para ver todas las directivas creadas en la organización, ejecute el cmdlet [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true).  Los resultados con valores de propiedad definidos que difieren de los valores predeterminados mencionados anteriormente se encuentran en el ámbito de la retirada.

    ```powershell
    Get-AzureADPolicy -All
    ```

1. Para ver qué aplicaciones y entidades de servicio están vinculadas a una directiva específica que identificó, ejecute el siguiente cmdlet [Get-AzureADPolicyAppliedObject](/powershell/module/azuread/get-azureadpolicyappliedobject?view=azureadps-2.0-preview&preserve-view=true) reemplazando **1a37dad8-5da7-4cc8-87c7-efbc0326cf20** con cualquiera de los identificadores de directiva. A continuación, puede decidir si desea configurar la frecuencia de inicio de sesión de acceso condicional o mantener los valores predeterminados de Azure AD.

    ```powershell
    Get-AzureADPolicyAppliedObject -id 1a37dad8-5da7-4cc8-87c7-efbc0326cf20
    ```

Si el inquilino tiene directivas que definen valores personalizados para las propiedades de configuración de tokens de sesión y actualización, Microsoft recomienda actualizarlas a los valores que reflejen los valores predeterminados descritos anteriormente. Si no se realiza ningún cambio, Azure AD respetará de forma automática los valores predeterminados.

## <a name="create-a-policy-for-web-sign-in"></a>Crear una directiva para inicio de sesión web

En este ejemplo, va a crear una directiva que requerirá que los usuarios se autentiquen con más frecuencia en la aplicación web. Esta directiva establecerá la vigencia de los tokens de acceso y de identificador y la antigüedad máxima de un token de sesión de varios factores en la entidad de servicio de su aplicación web.

1. Cree una directiva de vigencia del token.

    Para el inicio de sesión web esta directiva establecerá la vigencia del token de acceso y de identificador y la antigüedad máxima del token de sesión de un solo factor en 2 horas.

    1. Para crear la directiva, ejecute el cmdlet [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"AccessTokenLifetime":"02:00:00","MaxAgeSessionSingleFactor":"02:00:00"}}') -DisplayName "WebPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Para ver la nueva directiva y obtener la directiva de tipo **ObjectId**, ejecute el cmdlet [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. Asigne la directiva a su entidad de servicio. También necesitará obtener el valor de **ObjectId** de su entidad de servicio.

    1. Utilice el cmdlet [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal) para ver todas las entidades de servicio de la organización o una única entidad de servicio.
        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"
        ```

    1. Cuando tenga la entidad de servicio, ejecute el cmdlet [Add-AzureADServicePrincipalPolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true):
        ```powershell
        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

## <a name="create-token-lifetime-policies-for-refresh-and-session-tokens"></a>Creación de directivas de vigencia del token para tokens de actualización y de sesión
> [!IMPORTANT]
> A partir de mayo de 2020, los nuevos inquilinos no pueden configurar la vigencia de los tokens de actualización y de sesión.  Los inquilinos con una configuración existente pueden modificar las directivas de token de actualización y de sesión hasta el 30 de enero de 2021.  Azure Active Directory dejará de respetar la configuración existente de los tokens de sesión y la actualización en las directivas después del 30 de enero de 2021. Después de la retirada, todavía podrá configurar la vigencia de los tokens de acceso, SAML e identificador.
>
> Si necesita seguir definiendo el período de tiempo antes de que se pida al usuario que vuelva a iniciar sesión, configure la frecuencia de inicio de sesión en el acceso condicional. Para obtener más información sobre el acceso condicional, consulte [Configuración de la administración de las sesiones de autenticación con el acceso condicional](/azure/active-directory/conditional-access/howto-conditional-access-session-lifetime).
>
> Si no desea usar el acceso condicional después de la fecha de retirada, los tokens de actualización y de sesión se establecerán en la [configuración predeterminada](active-directory-configurable-token-lifetimes.md#configurable-token-lifetime-properties-after-the-retirement) en esa fecha y ya no podrá cambiar su vigencia.

### <a name="manage-an-organizations-default-policy"></a>Administrar una directiva predeterminada de una organización
En este ejemplo, crearemos una directiva que permita a sus usuarios iniciar sesión con menos frecuencia en toda su organización. Para ello, creamos una directiva de vigencia del token para tokens de actualización de un solo factor que se aplica en toda la organización. Esta directiva se aplicará a todas las aplicaciones de su organización y a todas las entidades de servicio que aún no tengan una directiva establecida en ella.

1. Cree una directiva de vigencia del token.

    1. Establezca el token de actualización de un solo factor en "hasta que se revoca". El token no expira hasta que se revoca el acceso. Cree la siguiente definición de directiva:

        ```powershell
        @('{
            "TokenLifetimePolicy":
            {
                "Version":1,
                "MaxAgeSingleFactor":"until-revoked"
            }
        }')
        ```

    1. Para crear la directiva, ejecute el cmdlet [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Para quitar cualquier espacio en blanco, ejecute el cmdlet [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        Get-AzureADPolicy -id | set-azureadpolicy -Definition @($((Get-AzureADPolicy -id ).Replace(" ","")))
        ```

    1. Para ver su nueva directiva y obtener el **ObjectID** de esta, ejecute el siguiente comando:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. Actualice la directiva.

    Puede decidir que la primera directiva que se establece en este ejemplo no sea tan estricta como exige el servicio. Para establecer que el token de actualización de un solo factor expire en dos días, ejecute el siguiente comando:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName $policy.DisplayName -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"2.00:00:00"}}')
    ```

### <a name="create-a-policy-for-a-native-app-that-calls-a-web-api"></a>Crear una directiva para una aplicación nativa que llama a una API web
En este ejemplo, va a crear una directiva que requerirá que los usuarios se autentiquen con menos frecuencia. La directiva también aumenta la cantidad de tiempo que un usuario puede estar inactivo antes de que este deba volver a autenticarse. La directiva se aplica a la API web. Cuando la aplicación nativa solicita la API web como recurso, se aplica esta directiva.

1. Cree una directiva de vigencia del token.

    1. Para crear una directiva estricta para una API web, ejecute el cmdlet [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxInactiveTime":"30.00:00:00","MaxAgeMultiFactor":"until-revoked","MaxAgeSingleFactor":"180.00:00:00"}}') -DisplayName "WebApiDefaultPolicyScenario" -IsOrganizationDefault $false -Type "TokenLifetimePolicy"
        ```

    1. Para ver la nueva directiva, ejecute el siguiente comando:

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. Asigne la directiva a la API web. También necesitará obtener el valor de **ObjectId** de la aplicación. Utilice el cmdlet [Get-AzureADApplication](/powershell/module/azuread/get-azureadapplication) para encontrar el valor de **ObjectId** de la aplicación o usar [Azure Portal](https://portal.azure.com/).

    Obtenga el valor de **ObjectId** de la aplicación y asigne la directiva:

    ```powershell
    # Get the application
    $app = Get-AzureADApplication -Filter "DisplayName eq 'Fourth Coffee Web API'"

    # Assign the policy to your web API.
    Add-AzureADApplicationPolicy -Id $app.ObjectId -RefObjectId $policy.Id
    ```

### <a name="manage-an-advanced-policy"></a>Administrar una directiva avanzada
En este ejemplo, va a crear algunas directivas para obtener información sobre cómo funciona el sistema de prioridad. También puede aprender a administrar varias directivas que se aplican a varios objetos.

1. Cree una directiva de vigencia del token.

    1. Para crear una directiva predeterminada de organización que establezca la vigencia del token de actualización de un solo factor en 30 días, ejecute el cmdlet [New-AzureADPolicy](/powershell/module/azuread/new-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        $policy = New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"30.00:00:00"}}') -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
        ```

    1. Para ver la nueva directiva, ejecute el cmdlet [Get-AzureADPolicy](/powershell/module/azuread/get-azureadpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        Get-AzureADPolicy -Id $policy.Id
        ```

1. Asigne la directiva a una entidad de servicio.

    Ahora tiene una directiva que se aplica a toda la organización. Es posible que quiera conservar esta directiva 30 días para una entidad de servicio específica, pero cambiar la directiva predeterminada de organización para que sea el límite superior de "hasta que se revoque".

    1. Para ver todas las entidades de servicio de la organización, utilice el cmdlet [Get-AzureADServicePrincipal](/powershell/module/azuread/get-azureadserviceprincipal).

    1. Cuando tenga la entidad de servicio, ejecute el cmdlet [Add-AzureADServicePrincipalPolicy](/powershell/module/azuread/add-azureadserviceprincipalpolicy?view=azureadps-2.0-preview&preserve-view=true):

        ```powershell
        # Get ID of the service principal
        $sp = Get-AzureADServicePrincipal -Filter "DisplayName eq '<service principal display name>'"

        # Assign policy to a service principal
        Add-AzureADServicePrincipalPolicy -Id $sp.ObjectId -RefObjectId $policy.Id
        ```

1. Establezca la marca `IsOrganizationDefault` en false:

    ```powershell
    Set-AzureADPolicy -Id $policy.Id -DisplayName "ComplexPolicyScenario" -IsOrganizationDefault $false
    ```

1. Cree una nueva directiva predeterminada de organización:

    ```powershell
    New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1,"MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "ComplexPolicyScenarioTwo" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"
    ```

    Ahora tiene la directiva original vinculada a la entidad de servicio y la nueva directiva establecida como su directiva predeterminada de organización. Es importante recordar que las directivas aplicadas a las entidades de servicio tienen prioridad sobre las directivas predeterminadas de organización.

## <a name="next-steps"></a>Pasos siguientes
Obtenga información sobre las [capacidades de administración de sesiones de autenticación](../conditional-access/howto-conditional-access-session-lifetime.md) en el acceso condicional de Azure AD.
