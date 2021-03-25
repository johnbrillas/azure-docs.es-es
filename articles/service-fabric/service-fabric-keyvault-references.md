---
title: 'Azure Service Fabric: uso de las referencias de KeyVault de la aplicación de Service Fabric'
description: En este artículo se explica cómo usar la compatibilidad con KeyVaultReference de Service Fabrica para los secretos de aplicación.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: a0e4ef0decae8cc9ab4dc5f8c69dfef854af81f3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98898603"
---
# <a name="keyvaultreference-support-for-azure-deployed-service-fabric-applications"></a>Compatibilidad de KeyVaultReference para aplicaciones de Service Fabric implementadas en Azure

Un desafío común al compilar aplicaciones en la nube es cómo distribuir de manera segura los secretos entre las aplicaciones. Por ejemplo, puede que desee implementar una clave de base de datos en la aplicación sin exponer la clave durante la canalización o al operador. La compatibilidad de KeyVaultReference con Service Fabric facilita la implementación de secretos en sus aplicaciones simplemente haciendo referencia a la URL del secreto almacenado en Key Vault. Service Fabric se encargará de obtener ese secreto en nombre de la identidad administrada de su aplicación y de activar la aplicación con el secreto.

> [!NOTE]
> La compatibilidad con KeyVaultReference para aplicaciones Service Fabric está en disponibilidad general (fuera de la versión preliminar) a partir de la versión 7.2 de Service Fabric, actualización acumulativa 5. Se recomienda que actualice a esta versión antes de usar esta característica.

> [!NOTE]
> KeyVaultReference para aplicaciones de Service Fabric solo admite secretos [con versiones](../key-vault/general/about-keys-secrets-certificates.md#objects-identifiers-and-versioning). No se admiten secretos sin versión. El almacén de claves debe estar en la misma suscripción que el clúster de Service Fabric. 

## <a name="prerequisites"></a>Requisitos previos

- Identidad administrada para aplicaciones de Service Fabric

    La compatibilidad de KeyVaultReference con Service Fabric utiliza la identidad administrada de una aplicación para obtener secretos en nombre de la aplicación, por lo que su aplicación debe implementarse a través de una identidad administrada y se le debe asignar una. Siga este [documento](concepts-managed-identity.md) para habilitar la identidad administrada para la aplicación.

- Almacén central de secretos (CSS).

    El Almacén central de secretos (CSS) es la caché de secretos local cifrada de Service Fabric. Esta característica usa CSS para proteger y conservar los secretos una vez que se capturan de Key Vault. También es necesario habilitar este servicio de sistema opcional para usar esta característica. Siga este [documento](service-fabric-application-secret-store.md) para habilitar y configurar CSS.

- Conceda permiso de acceso a keyvault a la identidad administrada de la aplicación

    Consulte este [documento](how-to-grant-access-other-resources.md) para ver cómo conceder acceso a keyvault a la identidad administrada. Además, tenga en cuenta que si usa una identidad administrada asignada por el sistema, la identidad administrada solo se crea después de la implementación de la aplicación. Esto puede crear condiciones de carrera en las que la aplicación intenta tener acceso al secreto antes de que se pueda conceder acceso al almacén a la identidad. El nombre de la identidad asignada por el sistema será `{cluster name}/{application name}/{service name}`.
    
## <a name="use-keyvaultreferences-in-your-application"></a>Uso de KeyVaultReferences en la aplicación
KeyVaultReferences se puede consumir de varias maneras.
- [Como una variable de entorno](#as-an-environment-variable)
- [Montada como archivo en el contenedor](#mounted-as-a-file-into-your-container)
- [Como referencia a una contraseña de repositorio de contenedor](#as-a-reference-to-a-container-repository-password)

### <a name="as-an-environment-variable"></a>Como una variable de entorno

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="MySecret" Type="KeyVaultReference" Value="<KeyVaultURL>"/>
</EnvironmentVariables>
```

```C#
string secret =  Environment.GetEnvironmentVariable("MySecret");
```

### <a name="mounted-as-a-file-into-your-container"></a>Montada como archivo en el contenedor

- Incorporación de una sección a settings.xml

    Defina el parámetro `MySecret` con el tipo `KeyVaultReference` y el valor `<KeyVaultURL>`.

    ```xml
    <Section Name="MySecrets">
        <Parameter Name="MySecret" Type="KeyVaultReference" Value="<KeyVaultURL>"/>
    </Section>
    ```

- Consulte la sección nueva en ApplicationManifest.xml en `<ConfigPackagePolicies>`.

    ```xml
    <ServiceManifestImport>
        <Policies>
        <IdentityBindingPolicy ServiceIdentityRef="MyServiceMI" ApplicationIdentityRef="MyApplicationMI" />
        <ConfigPackagePolicies CodePackageRef="Code">
            <!--Linux container example-->
            <ConfigPackage Name="Config" SectionName="MySecrets" EnvironmentVariableName="SecretPath" MountPoint="/var/secrets"/>
            <!--Windows container example-->
            <!-- <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="C:\secrets"/> -->
        </ConfigPackagePolicies>
        </Policies>
    </ServiceManifestImport>
    ```

- Uso de los secretos del código de servicio

    Cada parámetro incluido en `<Section  Name=MySecrets>` será un archivo en la carpeta a la que apunta EnvironmentVariable SecretPath. A continuación, el fragmento de código de C# muestra cómo leer MySecret en la aplicación.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "MySecret"))) 
    {
        string secret =  sr.ReadToEnd();
    }
    ```
    > [!NOTE] 
    > MountPoint controla la carpeta donde se montarán los archivos que contienen valores secretos.

### <a name="as-a-reference-to-a-container-repository-password"></a>Como referencia a una contraseña de repositorio de contenedor

```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="MyACRUser" Type="KeyVaultReference" Password="<KeyVaultURL>"/>
      </ContainerHostPolicies>
```

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de Azure KeyVault](../key-vault/index.yml)
* [Más información sobre el almacén central de secretos](service-fabric-application-secret-store.md)
* [Información sobre la Identidad administrada para las aplicaciones de Service Fabric](concepts-managed-identity.md)
