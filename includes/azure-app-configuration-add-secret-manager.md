---
ms.openlocfilehash: b1be5e903994f0a2c4a3d457f0c2bb5572a889c3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98663036"
---
## <a name="add-secret-manager"></a>Incorporación de Secret Manager

Una herramienta denominada Administrador de secretos almacena información confidencial para el trabajo de desarrollo fuera de su árbol de proyecto. Este enfoque ayuda a evitar el uso compartido accidental de secretos de la aplicación en el código fuente. Complete los pasos siguientes para habilitar el uso del Administrador de secretos en el proyecto de ASP.NET Core:

#### <a name="net-5x"></a>[.NET 5.x](#tab/core5x)

Desplácese hasta el directorio raíz del proyecto y ejecute el siguiente comando para habilitar el almacenamiento de secretos en el proyecto:

```dotnetcli
dotnet user-secrets init
```

Se agrega un elemento `UserSecretsId` que contiene un GUID al archivo *.csproj*:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>net5.0</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```

#### <a name="net-core-3x"></a>[.NET Core 3.x](#tab/core3x)

Desplácese hasta el directorio raíz del proyecto y ejecute el siguiente comando para habilitar el almacenamiento de secretos en el proyecto:

```dotnetcli
dotnet user-secrets init
```

Se agrega un elemento `UserSecretsId` que contiene un GUID al archivo *.csproj*:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    
    <PropertyGroup>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

</Project>
```

#### <a name="net-core-2x"></a>[.NET Core 2.x](#tab/core2x)

1. Abra el archivo *.csproj*.

1. Agregue un elemento `UserSecretsId` al archivo *.csproj* como se muestra aquí. Puede usar el mismo identificador único global, o bien puede reemplazar este valor por el suyo propio.

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">
    
        <PropertyGroup>
            <TargetFramework>netcoreapp2.1</TargetFramework>
            <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
        </PropertyGroup>
    
        <ItemGroup>
            <PackageReference Include="Microsoft.AspNetCore.App" />
            <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
        </ItemGroup>
    
    </Project>
    ```
    
1. Guarde el archivo *.csproj*.

---

> [!TIP]
> Para más información sobre el Administrador de secretos, consulte [Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core](/aspnet/core/security/app-secrets).
