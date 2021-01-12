---
title: Cómo usar puntos de conexión privados con el servicio Voz
titleSuffix: Azure Cognitive Services
description: Aprenda a usar el servicio Voz con los puntos de conexión privados que proporciona Azure Private Link
services: cognitive-services
author: alexeyo26
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/15/2020
ms.author: alexeyo
ms.openlocfilehash: f905582615b16780fae179ba6a21bd4343bd47f3
ms.sourcegitcommit: 90caa05809d85382c5a50a6804b9a4d8b39ee31e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755810"
---
# <a name="use-speech-service-through-a-private-endpoint"></a>Uso del servicio Voz mediante un punto de conexión privado

[Azure Private Link](../../private-link/private-link-overview.md) le permite conectarse a los servicios de Azure mediante un [punto de conexión privado](../../private-link/private-endpoint-overview.md).
Un punto de conexión privado es una dirección IP privada a la que solo se puede acceder desde una [red virtual](../../virtual-network/virtual-networks-overview.md) y una subred específicas.

En este artículo se explica cómo configurar y usar Private Link y los puntos de conexión privados con los servicios de voz de Azure Cognitive.

> [!NOTE]
> En este artículo se explican los detalles para configurar y usar Private Link con los servicios de voz de Azure Cognitive. Antes de continuar, vea cómo [usar redes virtuales con Cognitive Services](../cognitive-services-virtual-networks.md).

Realice las tareas siguientes para usar el servicio Voz mediante un punto de conexión privado:

1. [Crear un nombre de dominio personalizado del recurso de voz](#create-a-custom-domain-name)
2. [Crear y configurar los puntos de conexión privados](#enable-private-endpoints)
3. [Ajustar aplicaciones y soluciones existentes](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled)

Para quitar puntos de conexión privados más adelante pero seguir usando el recurso Voz, va a realizar las tareas de [esta sección](#use-speech-resource-with-custom-domain-name-without-private-endpoints).

## <a name="create-a-custom-domain-name"></a>Creación de un nombre de dominio personalizado

Los puntos de conexión privados requieren [Nombres de subdominio personalizados para Azure Cognitive Services](../cognitive-services-custom-subdomains.md). Siga las instrucciones que se indican a continuación para crear uno para el recurso Voz.

> [!CAUTION]
> Un recurso Voz con un nombre de dominio personalizado habilitado tiene otra forma de interactuar con el servicio Voz.
> Lo más probable es que tenga que ajustar el código de la aplicación para los escenarios de [punto de conexión privado habilitado](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) y [**sin** punto de conexión privado habilitado](#use-speech-resource-with-custom-domain-name-without-private-endpoints).
>
> Cuando se habilita un nombre de dominio personalizado, la operación [**no es reversible**](../cognitive-services-custom-subdomains.md#can-i-change-a-custom-domain-name). La única manera de volver a establecer el [nombre regional](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) es crear un recurso de voz nuevo.
>
> Si el recurso Voz tiene muchos modelos y proyectos personalizados asociados creados mediante [Speech Studio](https://speech.microsoft.com/), se recomienda **encarecidamente** probar la configuración en un recurso de prueba antes de modificar el usado en producción.

# <a name="azure-portal"></a>[Azure Portal](#tab/portal)

Siga estos pasos para crear un nombre de dominio personalizado mediante Azure Portal:

1. Vaya a [Azure Portal](https://portal.azure.com/) e inicie sesión en la cuenta de Azure.
1. Seleccione el recurso Voz necesario.
1. En el grupo **Administración de recursos** del panel de navegación izquierdo, haga clic en **Redes**.
1. En la pestaña **Firewalls y redes virtuales**, haga clic en **Generar nombre de dominio personalizado**. En el lado derecho aparece un panel con instrucciones para crear un subdominio personalizado único para el recurso.
1. En el panel Generar nombre de dominio personalizado, escriba una parte del nombre de dominio personalizado. El dominio personalizado completo tiene el siguiente aspecto: `https://{your custom name}.cognitiveservices.azure.com`. 
    **Después de crear un nombre de dominio personalizado, _no se puede_ cambiar. Vuelva a leer la alerta de precaución anterior.** Después de escribir el nombre de dominio personalizado, haga clic en **Guardar**.
1. Una vez finalizada la operación, en el grupo **Administración de recursos**, haga clic en **Claves y punto de conexión**. Confirme que el nuevo nombre de punto de conexión del recurso empieza de esta manera:

    `https://{your custom name}.cognitiveservices.azure.com`

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Para crear un nombre de dominio personalizado con PowerShell, confirme que el equipo tiene la versión 7.x o posterior de PowerShell con la versión 5.1.0 o posterior del módulo de Azure PowerShell. Siga estos pasos para ver las versiones de estas herramientas:

1. En una ventana de PowerShell, escriba:

    `$PSVersionTable`

    Confirme que el valor de PSVersion es mayor que 7.x. Para actualizar PowerShell, siga las instrucciones de [Instalación de distintas versiones de PowerShell](/powershell/scripting/install/installing-powershell).

1. En una ventana de PowerShell, escriba:

    `Get-Module -ListAvailable Az`

    Si no aparece nada, o si la versión del módulo de Azure PowerShell es inferior a 5.1.0, siga las instrucciones de [Instalación del módulo de Azure PowerShell](/powershell/azure/install-Az-ps) para actualizar.

Antes de continuar, ejecute `Connect-AzAccount` para crear una conexión con Azure.

## <a name="verify-custom-domain-name-is-available"></a>Comprobación de la disponibilidad del nombre de dominio personalizado

Debe comprobar si el dominio personalizado que quiere usar está disponible. Siga estos pasos para confirmar que el dominio está disponible mediante la operación [Check Domain Availability](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) (Comprobar disponibilidad de dominio) de la API de REST de Cognitive Services.

> [!TIP]
> El código siguiente **NO** funciona en Azure Cloud Shell.

```azurepowershell
$subId = "Your Azure subscription Id"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
Set-AzContext -SubscriptionId $subId

# Prepare OAuth token to use in request to Cognitive Services REST API.
$Context = Get-AzContext
$AccessToken = (Get-AzAccessToken -TenantId $Context.Tenant.Id).Token
$token = ConvertTo-SecureString -String $AccessToken -AsPlainText -Force

# Prepare and send the request to Cognitive Services REST API.
$uri = "https://management.azure.com/subscriptions/" + $subId + `
    "/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18"
$body = @{
subdomainName = $subdomainName
type = "Microsoft.CognitiveServices/accounts"
}
$jsonBody = $body | ConvertTo-Json
Invoke-RestMethod -Method Post -Uri $uri -ContentType "application/json" -Authentication Bearer `
    -Token $token -Body $jsonBody | Format-List
```
Si el nombre que quiere usar está disponible, recibirá una respuesta similar a la siguiente:
```azurepowershell
isSubdomainAvailable : True
reason               :
type                 :
subdomainName        : my-custom-name
```
Si el nombre ya está en uso, obtendrá la siguiente respuesta:
```azurepowershell
isSubdomainAvailable : False
reason               : Sub domain name 'my-custom-name' is already used. Please pick a different name.
type                 :
subdomainName        : my-custom-name
```
## <a name="create-your-custom-domain-name"></a>Creación del nombre de dominio personalizado

Para habilitar el nombre de dominio personalizado en el recurso de voz seleccionado, use el cmdlet [Set-AzCognitiveServicesAccount](/powershell/module/az.cognitiveservices/set-azcognitiveservicesaccount).

> [!CAUTION]
> Una vez que el código siguiente se ejecute correctamente, se creará un nombre de dominio personalizado para el recurso Voz.
> Recuerde que **no se puede** cambiar el nombre. Vea más información en la alerta **Advertencia** anterior.

```azurepowershell
$resourceGroup = "Resource group name where Speech resource is located"
$speechResourceName = "Your Speech resource name"
$subdomainName = "custom domain name"

# Select the Azure subscription that contains Speech resource.
# You can skip this step if your Azure account has only one active subscription.
$subId = "Your Azure subscription Id"
Set-AzContext -SubscriptionId $subId

# Set the custom domain name to the selected resource.
# CAUTION: THIS CANNOT BE CHANGED OR UNDONE!
Set-AzCognitiveServicesAccount -ResourceGroupName $resourceGroup `
    -Name $speechResourceName -CustomSubdomainName $subdomainName
```

# <a name="azure-cli"></a>[CLI de Azure](#tab/azure-cli)

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

- En esta sección necesitará la versión más reciente de la CLI de Azure. Si usa Azure Cloud Shell, ya está instalada la versión más reciente.

## <a name="verify-the-custom-domain-name-is-available"></a>Comprobación de la disponibilidad del nombre de dominio personalizado

Debe comprobar si el dominio personalizado que quiere usar es gratuito. Usaremos el método de [Comprobación de disponibilidad de dominio](/rest/api/cognitiveservices/accountmanagement/checkdomainavailability/checkdomainavailability) de la API de REST de Cognitive Services.

Copie el bloque de código siguiente, inserte el nombre de dominio personalizado que prefiera y guárdelo en el archivo `subdomain.json`.

```json
{
    "subdomainName": "custom domain name",
    "type": "Microsoft.CognitiveServices/accounts"
}
```

Copie el archivo en la carpeta actual o cárguelo en Azure Cloud Shell y ejecute el siguiente comando. (Reemplace `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` con el id. de su suscripción de Azure).

```azurecli-interactive
az rest --method post --url "https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/providers/Microsoft.CognitiveServices/checkDomainAvailability?api-version=2017-04-18" --body @subdomain.json
```
Si el nombre que quiere usar está disponible, recibirá una respuesta similar a la siguiente:
```azurecli
{
  "isSubdomainAvailable": true,
  "reason": null,
  "subdomainName": "my-custom-name",
  "type": null
}
```

Si el nombre ya está en uso, obtendrá la siguiente respuesta:
```azurecli
{
  "isSubdomainAvailable": false,
  "reason": "Sub domain name 'my-custom-name' is already used. Please pick a different name.",
  "subdomainName": "my-custom-name",
  "type": null
}
```
## <a name="enable-custom-domain-name"></a>Habilitación de un nombre de dominio personalizado

Para habilitar el nombre de dominio personalizado en el recurso de voz seleccionado, use el comando [az cognitiveservices account update](/cli/azure/cognitiveservices/account#az_cognitiveservices_account_update).

Seleccione la suscripción de Azure que contenga el recurso de voz. Si la cuenta de Azure tiene solo una suscripción activa, puede omitir este paso. (Reemplace `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` con el id. de su suscripción de Azure).
```azurecli-interactive
az account set --subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
Establezca el nombre de dominio personalizado en el recurso seleccionado. Reemplace los valores del parámetro de ejemplo por los reales y ejecute el comando siguiente.

> [!CAUTION]
> Después de ejecutar correctamente el comando siguiente, se creará un nombre de dominio personalizado para el recurso de voz. Recuerde que **no se puede** cambiar el nombre. Vea más información en la alerta de precaución anterior.

```azurecli
az cognitiveservices account update --name my-speech-resource-name --resource-group my-resource-group-name --custom-domain my-custom-name
```

**_

## <a name="enable-private-endpoints"></a>Habilitación de puntos de conexión privados

Habilite el punto de conexión privado mediante Azure Portal, Azure PowerShell o la CLI de Azure.

Se recomienda usar la [zona DNS privada](../../dns/private-dns-overview.md) conectada a la instancia de Virtual Network que tenga las actualizaciones necesarias para los puntos de conexión privados que creamos de forma predeterminada durante el proceso de aprovisionamiento. Sin embargo, si usa su propio servidor DNS, puede que tenga que realizar cambios adicionales en la configuración de DNS. Consulte la sección [DNS para puntos de conexión privados](#dns-for-private-endpoints). Lo mejor es decidir la estrategia de DNS _ *antes de** realizar el aprovisionamiento de los puntos de conexión privados de un recurso de voz de producción. También se recomienda realizar pruebas preliminares, especialmente si usa su propio servidor DNS.

Consulte los artículos siguientes para crear puntos de conexión privados. Estos artículos usan una aplicación web como un recurso de ejemplo para habilitarla con puntos de conexión privados. En cambio, use los siguientes parámetros:

| Setting             | Value                                    |
|---------------------|------------------------------------------|
| Tipo de recurso       | **Microsoft.CognitiveServices/accounts** |
| Recurso            | **\<your-speech-resource-name>**         |
| Recurso secundario de destino | **account**                              |

- [Inicio rápido: Creación de un punto de conexión privado mediante Azure Portal](../../private-link/create-private-endpoint-portal.md)
- [Creación de un punto de conexión privado mediante Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)
- [Creación de un punto de conexión privado mediante la CLI de Azure](../../private-link/create-private-endpoint-cli.md)

### <a name="dns-for-private-endpoints"></a>DNS de puntos de conexión privados

Familiarícese con los principios generales de [DNS para puntos de conexión privados en los recursos de Cognitive Services](../cognitive-services-virtual-networks.md#dns-changes-for-private-endpoints). A continuación, compruebe que la configuración de DNS funciona correctamente (consulte las siguientes subsecciones).

#### <a name="mandatory-check-dns-resolution-from-the-virtual-network"></a>(Comprobación obligatoria). Resolución de DNS en Virtual Network

Usaremos `my-private-link-speech.cognitiveservices.azure.com` como nombre DNS del recurso de voz de ejemplo para esta sección.

Inicie sesión en una máquina virtual que esté ubicada en la red virtual a la que haya adjuntado el punto de conexión privado. Abra el símbolo del sistema de Windows o el shell de Bash, ejecute `nslookup` y confirme que resuelve correctamente el nombre de dominio personalizado del recurso:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  168.63.129.16

Non-authoritative answer:
Name:    my-private-link-speech.privatelink.cognitiveservices.azure.com
Address:  172.28.0.10
Aliases:  my-private-link-speech.cognitiveservices.azure.com
```
Compruebe que la dirección IP resuelta corresponde a la dirección del punto de conexión privado.

#### <a name="optional-check-dns-resolution-from-other-networks"></a>(Comprobación opcional). Resolución de DNS desde otras redes

Esta comprobación es necesaria si tiene previsto usar el recurso Voz con punto de conexión privado habilitado en el modo "híbrido", donde ha habilitado o bien la opción de acceso *Todas las redes* o *Redes y puntos de conexión privados seleccionados* en la sección *Redes* del recurso. Si tiene previsto acceder al recurso con solo un punto de conexión privado, puede omitir esta sección.

En esta sección se usa `my-private-link-speech.cognitiveservices.azure.com` como nombre DNS del recurso Voz de ejemplo.

En cualquier equipo conectado a una red desde donde permita el acceso al recurso, abra el símbolo del sistema de Windows o el shell de Bash, ejecute el comando `nslookup` y confirme que resuelve correctamente el nombre de dominio personalizado del recurso:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    vnetproxyv1-weu-prod.westeurope.cloudapp.azure.com
Address:  13.69.67.71
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          my-private-link-speech.privatelink.cognitiveservices.azure.com
          westeurope.prod.vnet.cog.trafficmanager.net
```

Tenga en cuenta que la dirección IP resuelta apunta a un punto de conexión proxy de red virtual que envía el tráfico de red al punto de conexión privado del recurso de Cognitive Services. El comportamiento será diferente en el caso de un recurso con un nombre de dominio personalizado pero *sin* puntos de conexión privados. Vea [esta sección](#dns-configuration) para obtener más información.

## <a name="adjust-existing-applications-and-solutions"></a>Ajustar aplicaciones y soluciones existentes

Un recurso de voz con el dominio personalizado habilitado usa una manera diferente de interactuar con los servicios de voz. Esto se aplica a un recurso de voz con el dominio personalizado habilitado y que [tenga](#use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled) o que [no tenga](#use-speech-resource-with-custom-domain-name-without-private-endpoints) puntos de conexión privados. En la sección actual se proporciona la información necesaria para ambos casos.

### <a name="use-speech-resource-with-custom-domain-name-and-private-endpoint-enabled"></a>Uso del recurso Voz con el nombre de dominio personalizado y el punto de conexión privado habilitados

Un recurso de voz con el nombre de dominio personalizado y el punto de conexión privado habilitados usa una manera diferente de interactuar con los servicios de voz. En esta sección se explica cómo usar este tipo de recurso con la API de REST de los servicios de voz y el [SDK de voz](speech-sdk.md).

> [!NOTE]
> Tenga en cuenta que un recurso de voz sin puntos de conexión privados, pero con el **nombre de dominio personalizado** habilitado también tiene una forma especial de interactuar con los servicios de voz; sin embargo, esta manera difiere del escenario de un recurso de voz con un punto de conexión privado habilitado. Si tiene este tipo de recurso (por ejemplo, si tenía un recurso con puntos de conexión privados, pero después decidió quitarlos), asegúrese de familiarizarse con la [sección correspondiente](#use-speech-resource-with-custom-domain-name-without-private-endpoints).

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-rest-api"></a>Recurso de voz con un nombre de dominio personalizado y un punto de conexión privado. Uso con la API de REST

Usaremos `my-private-link-speech.cognitiveservices.azure.com` como nombre DNS del recurso de voz de ejemplo (dominio personalizado) para esta sección.

##### <a name="note-on-speech-services-rest-api"></a>Nota sobre la API de REST de los servicios de voz

Los servicios de voz tienen la API de REST para las opciones de [conversión de voz en texto](rest-speech-to-text.md) y de [conversión de texto a voz](rest-text-to-speech.md). Debe tener en cuenta lo siguiente para el escenario con el punto de conexión privado habilitado.

La conversión de texto a voz tiene dos API de REST diferentes. Cada API sirve para un propósito diferente, usa puntos de conexión diferentes y requiere un enfoque diferente cuando se usa para iniciar sesión en el escenario con el punto de conexión privado habilitado.

Las API de REST de conversión de voz en texto son:
- [La API de REST de conversión de voz en texto v3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30) se usa para realizar [transcripciones por lotes](batch-transcription.md) y para el [Habla personalizada](custom-speech-overview.md). La versión 3.0 es la [sucesora de la versión 2.0](/azure/cognitive-services/speech-service/migrate-v2-to-v3).
- [La API de REST de conversión de voz en texto para audios breves](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio)se usa para la transcripción en línea. 

El uso de la API de REST de conversión de voz en texto para audios breves y la API de REST de conversión de texto a voz en el escenario del punto de conexión privado es el mismo y es equivalente al [caso del SDK de voz](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) que se describe más adelante en este artículo. 

La API de REST de conversión de voz en texto v3.0 usa un conjunto diferente de puntos de conexión y, por lo tanto, requiere un enfoque diferente para el escenario con el punto de conexión privado habilitado.

Ambos casos se describen en las subsecciones siguientes.


##### <a name="speech-to-text-rest-api-v30"></a>API de REST de conversión de voz en texto v3.0

Normalmente, los recursos de voz usan los [puntos de conexión regionales de Cognitive Services](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) para comunicarse con la [API de REST de conversión de voz en texto v3.0](rest-speech-to-text.md#speech-to-text-rest-api-v30). Estos recursos tienen el siguiente formato de nomenclatura: <p/>`{region}.api.cognitive.microsoft.com`

Esta es la dirección URL de una solicitud de ejemplo:

```http
https://westeurope.api.cognitive.microsoft.com/speechtotext/v3.0/transcriptions
```
Después de habilitar el dominio personalizado para un recurso de voz (lo cual es necesario para los puntos de conexión privados), dicho recurso usará el siguiente patrón de nombres DNS para el punto de conexión básico de la API de REST: <p/>`{your custom name}.cognitiveservices.azure.com`

Esto significa que, en nuestro ejemplo, el nombre del punto de conexión de la API de REST será: <p/>`my-private-link-speech.cognitiveservices.azure.com`

Asimismo, la dirección URL de la solicitud de ejemplo anterior se debe convertir en:
```http
https://my-private-link-speech.cognitiveservices.azure.com/speechtotext/v3.0/transcriptions
```
Esta dirección URL debe ser accesible desde la instancia de Virtual Network que tenga el punto de conexión privado adjunto (siempre que la [resolución de DNS sea correcta](#mandatory-check-dns-resolution-from-the-virtual-network)).

Por lo general, después de habilitar el nombre de dominio personalizado para un recurso de voz, debe reemplazar el nombre de host en todas las direcciones URL de solicitud con el nuevo nombre de host del dominio personalizado. El resto de partes de la solicitud (como la ruta de acceso `/speechtotext/v3.0/transcriptions` en el ejemplo anterior) siguen siendo las mismas.

> [!TIP]
> Algunos clientes desarrollaron aplicaciones que usan la parte de la región del nombre DNS del punto de conexión regional (por ejemplo, para enviar la solicitud al recurso de voz implementado en una región de Azure determinada).
>
> El nombre de dominio personalizado del recurso de voz **no** contiene información acerca de la región en la que se implementa el recurso. Por lo tanto, la lógica de la aplicación descrita anteriormente **no** funcionará y deberá modificarla.

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>API de REST de conversión de voz en texto para audios breves y API de REST de conversión de texto a voz

La [API de REST de conversión de voz en texto para audios breves](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) y la [API de REST de conversión de texto a voz](rest-text-to-speech.md) usan dos tipos de puntos de conexión:
- [Puntos de conexión regionales de Cognitive Services](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) para comunicarse con la API de REST de Cognitive Services y así obtener un token de autorización.
- Puntos de conexión especiales para el resto de operaciones.

La descripción detallada de estos puntos de conexión especiales y cómo se debe transformar su dirección URL en un recurso de voz con el punto de conexión privado habilitado, se proporciona en [esta subsección](#general-principle) de la sección "Uso con el SDK de voz". El mismo principio descrito para el SDK es aplicable a las API de REST de conversión de texto a voz v1.0 y de conversión de voz en texto.

Familiarícese con el material de la subsección mencionado en el párrafo anterior y consulte el ejemplo siguiente. (En el ejemplo se describe la API de REST de conversión texto a voz; el uso de la API de REST de conversión de voz en texto para audios breves es exactamente igual).

> [!NOTE]
> Cuando se usa la **API de REST de conversión de voz en texto para audios breves** en escenarios de puntos de conexión privados, es necesario usar el token de autorización que [pasa a través del](rest-speech-to-text.md#request-headers) [encabezado](rest-speech-to-text.md#request-headers) `Authorization`; pasar la clave de la suscripción de voz al punto de conexión especial a través del encabezado `Ocp-Apim-Subscription-Key` **no** funcionará y generará el error 401.

**Ejemplo de uso de la API de REST de conversión de texto a voz.**

Usaremos el Oeste de Europa como región de Azure de ejemplo y `my-private-link-speech.cognitiveservices.azure.com` como un nombre DNS del recurso de voz de ejemplo (dominio personalizado). El nombre de dominio personalizado `my-private-link-speech.cognitiveservices.azure.com` en nuestro ejemplo pertenece al recurso de voz creado en la región del Oeste de Europa.

Para obtener la lista de las voces admitidas en la región, es necesario realizar las dos operaciones siguientes:

- Obtención del token de autorización:
```http
https://westeurope.api.cognitive.microsoft.com/sts/v1.0/issuetoken
```
- Con el token, obtenga la lista de voces:
```http
https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list
```
(Consulte más detalles sobre los pasos anteriores en la [documentación de la API de REST de conversión de texto a voz](rest-text-to-speech.md)).

En cuanto al recurso de voz con el punto de conexión privado habilitado, es necesario modificar las direcciones URL del punto de conexión para realizar la misma secuencia de la operación. La misma secuencia tendrá el siguiente aspecto:
- Obtención del token de autorización mediante:
```http
https://my-private-link-speech.cognitiveservices.azure.com/v1.0/issuetoken
```
(consulte la explicación detallada en la subsección anterior de la [API de REST de conversión de voz en texto v3.0](#speech-to-text-rest-api-v30)).
- Uso del token obtenido para obtener la lista de voces mediante:
```http
https://my-private-link-speech.cognitiveservices.azure.com/tts/cognitiveservices/voices/list
```
(consulte la explicación detallada en la subsección del [Principio general](#general-principle)de la sección "Uso con el SDK de voz" que tiene más adelante).

#### <a name="speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk"></a>Recurso de voz con un nombre de dominio personalizado y un punto de conexión privado. Uso con el SDK de voz

El uso del SDK de voz con el nombre de dominio personalizado y los recursos de voz con el punto de conexión privado habilitado requiere la revisión del código de la aplicación y de los cambios probables. Estamos trabajando para proporcionarle una compatibilidad más fluida del escenario del punto de conexión privado.

Usaremos `my-private-link-speech.cognitiveservices.azure.com` como nombre DNS del recurso de voz de ejemplo (dominio personalizado) para esta sección.

##### <a name="general-principle"></a>Principio general

Normalmente, en los escenarios de SDK (así como en los escenarios de la API de REST de conversión de texto a voz) los recursos de voz usan los puntos de conexión regionales especiales para las distintas ofertas de servicio. El formato del nombre DNS para estos puntos de conexión es: </p>`{region}.{speech service offering}.speech.microsoft.com`

Ejemplo: </p>`westeurope.stt.speech.microsoft.com`

Todos los valores posibles de la región (el primer elemento del nombre DNS) se enumeran [aquí](regions.md); en la siguiente tabla se presenta el valor posible de la oferta de servicios de voz (el segundo elemento del nombre DNS):

| Valor del nombre DNS | Oferta de servicios de voz                                    |
|----------------|-------------------------------------------------------------|
| `commands`     | [Comandos personalizados](custom-commands.md)                       |
| `convai`       | [Transcripción de conversaciones](conversation-transcription.md) |
| `s2s`          | [Traducción de voz](speech-translation.md)                 |
| `stt`          | [Voz a texto](speech-to-text.md)                         |
| `tts`          | [Texto a voz](text-to-speech.md)                         |
| `voice`        | [Voz personalizada](how-to-custom-voice.md)                      |

Por lo tanto, el ejemplo anterior (`westeurope.stt.speech.microsoft.com`) representa el punto de conexión de conversión de voz en texto en el Oeste de Europa.

Los puntos de conexión con un punto de conexión privado habilitado se comunican con los servicios de voz a través de un proxy especial y, debido a ello, **las direcciones URL del punto de conexión deben cambiarse**. Se aplica el siguiente principio: una dirección URL del punto de conexión "estándar" sigue el patrón de: <p/>`{region}.{speech service offering}.speech.microsoft.com/{URL path}`

Se debe cambiar a: <p/>`{your custom name}.cognitiveservices.azure.com/{speech service offering}/{URL path}`

**Ejemplo 1.** La aplicación se comunica mediante la siguiente dirección URL (reconocimiento de voz mediante el modelo base para inglés de EE. UU. en el Oeste de Europa): 
```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Para usarlo en el escenario con el punto de conexión privado habilitado cuando el nombre de dominio personalizado del recurso de voz es `my-private-link-speech.cognitiveservices.azure.com`, esta dirección URL debe modificarse de la siguiente manera:
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```

Echemos un vistazo con más detalle:
- El nombre de host `westeurope.stt.speech.microsoft.com` se reemplaza por el nombre de host del dominio personalizado `my-private-link-speech.cognitiveservices.azure.com`.
- El segundo elemento del nombre DNS original (`stt`) se convierte en el primer elemento de la ruta acceso de la dirección URL y precede a la ruta de acceso original; esto es, la dirección URL original `/speech/recognition/conversation/cognitiveservices/v1?language=en-US` se convierte en `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US`.
 
**Ejemplo 2.** La aplicación se comunica mediante la siguiente dirección URL (síntesis de voz mediante el modelo de voz personalizado en el Oeste de Europa): 
```http
https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```
Para usarlo en el escenario con el punto de conexión privado habilitado cuando el nombre de dominio personalizado del recurso de voz es `my-private-link-speech.cognitiveservices.azure.com`, esta dirección URL debe modificarse de la siguiente manera: 
```http
https://my-private-link-speech.cognitiveservices.azure.com/voice/cognitiveservices/v1?deploymentId=974481cc-b769-4b29-af70-2fb557b897c4
```

Se aplica el mismo principio que en el ejemplo 1, pero el elemento clave esta vez es `voice`.

##### <a name="modifying-applications"></a>Modificación de aplicaciones

Para aplicar el principio descrito en la sección anterior al código de la aplicación, debe hacer dos cosas importantes:

- Determinar la dirección URL del punto de conexión que usa la aplicación.
- Modificar la dirección URL del punto de conexión tal y como se describe en la sección anterior y crear la instancia de clase `SpeechConfig` usando explícitamente esta dirección URL modificada.

###### <a name="determine-application-endpoint-url"></a>Determine la dirección URL del punto de conexión de la aplicación

- [Habilite el registro de la aplicación](how-to-use-logging.md) y ejecútelo para generar el registro.
- En el archivo de registro, busque `SPEECH-ConnectionUrl`. La cadena contendrá el parámetro `value`, que a su vez contendrá la dirección URL completa que estaba usando la aplicación.

Ejemplo de una línea de archivo de registro con la dirección URL del punto de conexión:
```
(114917): 41ms SPX_DBG_TRACE_VERBOSE:  property_bag_impl.cpp:138 ISpxPropertyBagImpl::LogPropertyAndValue: this=0x0000028FE4809D78; name='SPEECH-ConnectionUrl'; value='wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?traffictype=spx&language=en-US'
```
Por lo tanto, la dirección URL que usó la aplicación en este ejemplo es:
```
wss://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```
###### <a name="create-speechconfig-instance-using-full-endpoint-url"></a>Cree la instancia `SpeechConfig` mediante la dirección URL completa del punto de conexión

Modifique el punto de conexión que determinó en la sección anterior, tal como se describe en el [Principio general](#general-principle) anterior.

A continuación, debe modificar el modo de crear la instancia `SpeechConfig`. Lo más probable es que la aplicación esté usando algo parecido a esto en este momento:
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
Esto no funcionará en el recurso de voz con el punto de conexión privado habilitado, debido a los cambios en el nombre de host y la dirección URL descritos en las secciones anteriores. Si intenta ejecutar la aplicación existente sin ninguna modificación mediante la clave de un recurso con un punto de conexión privado habilitado, obtendrá un error de autenticación (401).

Para que funcione, debe modificar la manera de crear una instancia de clase `SpeechConfig` y usar la inicialización "desde el punto de conexión" / "con el punto de conexión". Supongamos que tenemos definidas las dos variables siguientes:
- `subscriptionKey`, que contiene la clave del recurso de voz con el punto de conexión privado habilitado.
- `endPoint`, que contiene la dirección URL completa y **modificada** del punto de conexión (para ello debe usar el tipo que necesite el lenguaje de programación correspondiente). En nuestro ejemplo, esta variable debe contener:
```
wss://my-private-link-speech.cognitiveservices.azure.com/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US
```
A continuación, necesitamos crear una instancia de clase `SpeechConfig` similar a la siguiente:
```csharp
var config = SpeechConfig.FromEndpoint(endPoint, subscriptionKey);
```
```cpp
auto config = SpeechConfig::FromEndpoint(endPoint, subscriptionKey);
```
```java
SpeechConfig config = SpeechConfig.fromEndpoint(endPoint, subscriptionKey);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(endpoint=endPoint, subscription=subscriptionKey)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithEndpoint:endPoint subscription:subscriptionKey];
```
> [!TIP]
> Los parámetros de consulta especificados en el URI del punto de conexión no cambian, aunque los hayan establecido otras API. Por ejemplo, si el idioma de reconocimiento se define en el URI como el parámetro de consulta "language=en-US" y también se establece en "ru-RU" mediante la propiedad correspondiente, la configuración de idioma del URI tiene prioridad y el idioma efectivo será "en-US". Otras API solo pueden establecer los parámetros que no se especifiquen en el URI del punto de conexión.

Después de realizar esta modificación, la aplicación debe funcionar con los recursos de voz privados habilitados. Estamos trabajando para proporcionarle una compatibilidad más fluida del escenario del punto de conexión privado.

### <a name="use-speech-resource-with-custom-domain-name-without-private-endpoints"></a>Uso del recurso Voz con un nombre de dominio personalizado sin puntos de conexión privados

En este artículo hemos señalado varias veces que habilitar un dominio personalizado para un recurso de voz es **irreversible**, y que este recurso usará una manera diferente de comunicarse con los servicios de voz usando para ello otros servicios "habituales" (es decir, aquellos que usan [nombres de punto de conexión regional](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints)).

En esta sección se explica cómo usar un recurso de voz con el nombre de dominio personalizado habilitado, pero **sin** ningún punto de conexión privado, con la API de REST de servicios de voz y el [SDK de voz](speech-sdk.md). Puede tratarse de un recurso que se usó una vez en un escenario de punto de conexión privado, pero del cual se eliminaron los puntos de conexión privados.

#### <a name="dns-configuration"></a>Configuración de DNS

Recuerde que el nombre DNS de dominio personalizado del recurso de voz con un punto de conexión privado habilitado se [resuelve desde redes públicas](#optional-check-dns-resolution-from-other-networks). En este caso, tenga en cuenta que la dirección IP resuelta está dirigida hacia un punto de conexión de proxy de red virtual que se usa para enviar el tráfico de red al punto de conexión privado que tenga habilitado el recurso de Cognitive Services.

Sin embargo, cuando se quitan **todos** los puntos de conexión privados de recursos (o justo después de habilitar el nombre de dominio personalizado), se vuelve a aprovisionar el registro CNAME del recurso de voz, que apunta a la dirección IP del [punto de conexión regional de Cognitive Services](../cognitive-services-custom-subdomains.md#is-there-a-list-of-regional-endpoints) correspondiente.

Así pues, el resultado del comando `nslookup` debe tener el siguiente aspecto:
```dos
C:\>nslookup my-private-link-speech.cognitiveservices.azure.com
Server:  UnKnown
Address:  fe80::1

Non-authoritative answer:
Name:    apimgmthskquihpkz6d90kmhvnabrx3ms3pdubscpdfk1tsx3a.cloudapp.net
Address:  13.93.122.1
Aliases:  my-private-link-speech.cognitiveservices.azure.com
          westeurope.api.cognitive.microsoft.com
          cognitiveweprod.trafficmanager.net
          cognitiveweprod.azure-api.net
          apimgmttmdjylckcx6clmh2isu2wr38uqzm63s8n4ub2y3e6xs.trafficmanager.net
          cognitiveweprod-westeurope-01.regional.azure-api.net
```
Compárelo con el resultado de [esta sección](#optional-check-dns-resolution-from-other-networks).

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-rest-api"></a>Recurso de voz con el nombre de dominio personalizado sin los puntos de conexión privados. Uso con la API de REST

##### <a name="speech-to-text-rest-api-v30"></a>API de REST de conversión de voz en texto v3.0

El uso de la API de REST de conversión de voz en texto v3.0 es totalmente igual al caso de los [recursos de voz con los puntos de conexión privados habilitados](#speech-to-text-rest-api-v30).

##### <a name="speech-to-text-rest-api-for-short-audio-and-text-to-speech-rest-api"></a>API de REST de conversión de voz en texto para audios breves y API de REST de conversión de texto a voz

En este caso, tanto el uso de la API de REST de conversión de voz en texto para audios breves como el uso de la API de REST de conversión de texto a voz no tienen diferencia alguna en el caso general; sin embargo, hay una excepción para la API de REST de conversión de voz en texto para audios breves (consulte la nota a continuación). Ambas API deben usarse tal y como se describe en la documentación de la [API de REST de conversión de voz en texto para audios breves](rest-speech-to-text.md#speech-to-text-rest-api-for-short-audio) y de la [API de REST de conversión de texto a voz](rest-text-to-speech.md).

> [!NOTE]
> Cuando se usa la **API de REST de conversión de voz en texto para audios breves** en escenarios de dominios personalizados, es necesario usar el token de autorización que [pasa a través del](rest-speech-to-text.md#request-headers) [encabezado](rest-speech-to-text.md#request-headers) `Authorization`; pasar la clave de la suscripción de voz al punto de conexión especial a través del encabezado `Ocp-Apim-Subscription-Key` **no** funcionará y generará el error 401.

#### <a name="speech-resource-with-custom-domain-name-without-private-endpoints-usage-with-speech-sdk"></a>Recurso de voz con el nombre de dominio personalizado sin los puntos de conexión privados. Uso con el SDK de voz

El uso del SDK de voz con el nombre de dominio personalizado y los recursos de voz **sin** los puntos de conexión privados, requiere la revisión del código de la aplicación y de los cambios probables. Tenga en cuenta que estos cambios son **diferentes** en comparación con el caso de un [recurso de voz con el punto de conexión privado habilitado](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk). Estamos trabajando para proporcionarle una compatibilidad más fluida del escenario del punto de conexión privado y del dominio personalizado.

Usaremos `my-private-link-speech.cognitiveservices.azure.com` como nombre DNS del recurso de voz de ejemplo (dominio personalizado) para esta sección.

En la sección sobre el [recurso de voz con un punto de conexión privado habilitado](#speech-resource-with-custom-domain-name-and-private-endpoint-usage-with-speech-sdk) le hemos explicado cómo determinar la dirección URL del punto de conexión que se ha usado, cómo modificarla y hacer que funcione a través de la inicialización de la instancia de clase `SpeechConfig` "desde el punto de conexión"/"con el punto de conexión".

Sin embargo, si intenta ejecutar la misma aplicación después de quitar todos los puntos de conexión privados (lo que le permitirá obtener un tiempo para reaprovisionar el registro DNS correspondiente), obtendrá un error de servicio interno (404). El motivo es el [registro DNS](#dns-configuration), ya que ahora está dirigido al punto de conexión regional de Cognitive Services en lugar del proxy de red virtual; por ello, no se encuentran las rutas de dirección URL como `/stt/speech/recognition/conversation/cognitiveservices/v1?language=en-US`, y se devuelve el error "No encontrado" (404).

Si "revierte" la aplicación a la creación de instancias "estándar" de `SpeechConfig` en el estilo de
```csharp
var config = SpeechConfig.FromSubscription(subscriptionKey, azureRegion);
```
la aplicación terminará con el error de autenticación (401).

##### <a name="modifying-applications"></a>Modificación de aplicaciones

Para habilitar la aplicación para el escenario del recurso de voz con un nombre de dominio personalizado que no tenga puntos de conexión privados, debe hacer lo siguiente:
- Solicitar el token de autorización a través de la API de REST de Cognitive Services.
- Crear una instancia de `SpeechConfig` clase con el método "del token de autorización"/"con el token de autorización". 

###### <a name="request-authorization-token"></a>Solicite el token de autorización

Consulte [este artículo](../authentication.md#authenticate-with-an-authentication-token) sobre cómo obtener el token a través de la API de REST de Cognitive Services. 

Use el nombre de dominio personalizado en la dirección URL del punto de conexión que se encuentra en nuestro ejemplo; esta dirección URL es:
```http
https://my-private-link-speech.cognitiveservices.azure.com/sts/v1.0/issueToken
```
> [!TIP]
> Puede encontrar esta dirección URL en la sección *Claves y puntos de conexión* (en el grupo de *administración de recursos*) del recurso de voz en Azure Portal.

###### <a name="create-speechconfig-instance-using-authorization-token"></a>Cree una instancia `SpeechConfig` mediante el token de autorización

Debe crear una instancia de clase `SpeechConfig` mediante el token de autorización que obtuvo en la sección anterior. Supongamos que tenemos definidas las variables siguientes:

- `token`, que contiene el token de autorización obtenido en la sección anterior.
- `azureRegion`, que contiene el nombre de la [región](regions.md) del recurso de voz (ejemplo: `westeurope`).
- `outError` (solo para el caso [Objective C](/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithauthorizationtokenregionerror)).

A continuación, necesitamos crear una instancia de clase `SpeechConfig` similar a la siguiente:
```csharp
var config = SpeechConfig.FromAuthorizationToken(token, azureRegion);
```
```cpp
auto config = SpeechConfig::FromAuthorizationToken(token, azureRegion);
```
```java
SpeechConfig config = SpeechConfig.fromAuthorizationToken(token, azureRegion);
```
```python
import azure.cognitiveservices.speech as speechsdk
speech_config = speechsdk.SpeechConfig(auth_token=token, region=azureRegion)
```
```objectivec
SPXSpeechConfiguration *speechConfig = [[SPXSpeechConfiguration alloc] initWithAuthorizationToken:token region:azureRegion error:outError];
```
> [!NOTE]
> El autor de la llamada debe asegurarse de que el token de autorización es válido. Antes de que expire el token de autorización, el autor de la llamada debe actualizarlo llamando a este establecedor con un nuevo token válido. A medida que se copien los valores de configuración al crear un nuevo reconocedor o sintetizador, el nuevo valor de token no se aplicará a los reconocedores que ya se hayan creado. En el caso de los reconocedores o sintetizadores que ya se hayan creado, debe establecer el token de autorización del reconocedor o sintetizador correspondiente para actualizar el token. De lo contrario, los reconocedores o sintetizadores detectarán errores durante el reconocimiento o la síntesis.

Después de realizar esta modificación, la aplicación debe funcionar con los recursos de voz que tengan el nombre de dominio personalizado habilitado y sin puntos de conexión privados. Estamos trabajando para proporcionarle una compatibilidad más fluida del escenario del punto de conexión privado o del dominio personalizado.

## <a name="pricing"></a>Precios

Para más información sobre los precios, consulte [Precios de Azure Private Link](https://azure.microsoft.com/pricing/details/private-link).

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre [Azure Private Link](../../private-link/private-link-overview.md).
* Más información sobre el [SDK de voz](speech-sdk.md).
* Más información sobre la [API de REST de conversión de voz en texto](rest-speech-to-text.md).
* Más información sobre la [API de REST de conversión de texto a voz](rest-text-to-speech.md).
