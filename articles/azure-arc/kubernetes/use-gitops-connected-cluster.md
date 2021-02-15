---
title: Implementación de configuraciones mediante GitOps en clústeres de Kubernetes habilitados para Arc (versión preliminar)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Uso de GitOps para configurar un clúster de Kubernetes habilitado para Azure Arc (versión preliminar)
keywords: GitOps, Kubernetes, K8s, Azure, Arc, Azure Kubernetes Service, AKS, contenedores
ms.openlocfilehash: 72dc42fffb3653de81477fa504c11b9b0328d2eb
ms.sourcegitcommit: 7e117cfec95a7e61f4720db3c36c4fa35021846b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99988706"
---
# <a name="deploy-configurations-using-gitops-on-arc-enabled-kubernetes-cluster-preview"></a>Implementación de configuraciones mediante GitOps en clústeres de Kubernetes habilitados para Arc (versión preliminar)

GitOps, con respecto a Kubernetes, es la práctica de declarar el estado deseado de configuración de Kubernetes (implementaciones, espacios de nombres, etc.) en un repositorio de Git seguida de una implementación basada en sondeo e incorporación de cambios de estas configuraciones en el clúster mediante un operador. En este documento se habla de la configuración de estos flujos de trabajo en clústeres de Kubernetes habilitados para Azure Arc.

La conexión entre el clúster y un repositorio de Git se crea en Azure Resource Manager como un recurso de extensión `Microsoft.KubernetesConfiguration/sourceControlConfigurations`. Las propiedades del recurso `sourceControlConfiguration` representan dónde y cómo deben fluir los recursos de Kubernetes desde Git al clúster. Los datos de `sourceControlConfiguration` se almacenan cifrados en reposo en una base de datos de Azure Cosmos DB para garantizar su confidencialidad.

El elemento `config-agent` que se ejecuta en el clúster es responsable de inspeccionar los recursos de extensión de `sourceControlConfiguration` nuevos o actualizados en el recurso de Kubernetes habilitado para Azure Arc, de implementar un operador de Flux para inspeccionar el repositorio de Git para cada instancia de `sourceControlConfiguration` y de aplicar las actualizaciones realizadas en `sourceControlConfiguration`. Se pueden crear varios recursos `sourceControlConfiguration` en el mismo clúster de Kubernetes habilitado para Azure Arc para lograr varios inquilinos. Puede crear cada elemento `sourceControlConfiguration` con un ámbito de `namespace` diferente para limitar las implementaciones a los espacios de nombres correspondientes.

El repositorio de Git puede contener manifiestos en formato YAML en los que se describe cualquier recurso de Kubernetes válido, incluidos espacios de nombres, ConfigMaps, implementaciones, DaemonSets, etc.  También puede contener gráficos de Helm para implementar aplicaciones. Un conjunto común de escenarios incluye la definición de una configuración de línea de base para la organización, que podría incluir roles y enlaces de Azure comunes, agentes de supervisión o registro, o bien servicios para todo el clúster.

Se puede usar el mismo patrón para administrar una colección mayor de clústeres, que se puede implementar en entornos heterogéneos. Por ejemplo, puede tener un repositorio que defina la configuración de línea de base para la organización y aplicarla a decenas de clústeres de Kubernetes a la vez. [Azure Policy puede automatizar](use-azure-policy.md) la creación de un elemento `sourceControlConfiguration` con un conjunto específico de parámetros en todos los recursos de Kubernetes habilitados para Azure Arc en un ámbito (suscripción o grupo de recursos).

En esta guía de introducción se describirá la aplicación de un conjunto de configuraciones con ámbito de administración de clústeres.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster conectado de Kubernetes habilitado para Azure Arc. Si necesita un clúster conectado, vea el [inicio rápido sobre conexión de un clúster](./connect-cluster.md).

## <a name="create-a-configuration"></a>Creación de una configuración

El [repositorio de ejemplo](https://github.com/Azure/arc-k8s-demo) que se utiliza en este documento está estructurado en torno al rol de un operador de clúster que quiere aprovisionar algunos espacios de nombres, implementar una carga de trabajo común y proporcionar una configuración específica del equipo. Mediante este repositorio se crean los siguientes recursos en el clúster:

**Espacios de nombres:** `cluster-config`, `team-a`, `team-b`
**Implementación:** `cluster-config/azure-vote`
**ConfigMap:** `team-a/endpoints`

`config-agent` sondea Azure en busca de elementos `sourceControlConfiguration` nuevos o actualizados cada 30 segundos, que es el tiempo máximo que tarda `config-agent` en seleccionar una configuración nueva o actualizada.
Si va a asociar un repositorio privado a `sourceControlConfiguration`, asegúrese de realizar también los pasos de [Aplicación de la configuración desde un repositorio de Git privado](#apply-configuration-from-a-private-git-repository).

### <a name="using-azure-cli"></a>Uso de la CLI de Azure

Use la extensión de la CLI de Azure para `k8sconfiguration`, a fin de vincular el clúster conectado al [repositorio de Git de ejemplo](https://github.com/Azure/arc-k8s-demo). A esta configuración se le asignará el nombre `cluster-config`, se le indicará al agente que implemente el operador en el espacio de nombres `cluster-config` y se concederán permisos `cluster-admin` al operador.

```azurecli
az k8sconfiguration create --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --operator-instance-name cluster-config --operator-namespace cluster-config --repository-url https://github.com/Azure/arc-k8s-demo --scope cluster --cluster-type connectedClusters
```

**Salida:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Pending",
    "lastConfigApplied": "0001-01-01T00:00:00",
    "message": "{\"OperatorMessage\":null,\"ClusterState\":null}",
    "messageLevel": "3"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": null,
  "id": "/subscriptions/<sub id>/resourceGroups/<group name>/providers/Microsoft.Kubernetes/connectedClusters/<cluster name>/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "",
  "repositoryUrl": "https://github.com/Azure/arc-k8s-demo",
  "resourceGroup": "MyRG",
  "sshKnownHostsContents": "",
  "systemData": {
    "createdAt": "2020-11-24T21:22:01.542801+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-11-24T21:22:01.542801+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
  },
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
  ```

#### <a name="use-a-public-git-repo"></a>Uso de un repositorio público de Git

| Parámetro | Formato |
| ------------- | ------------- |
| --repository-url | http[s]://servidor/repositorio[.git] o git://servidor/repositorio[.git]

#### <a name="use-a-private-git-repo-with-ssh-and-flux-created-keys"></a>Uso de un repositorio de Git privado con claves SSH y creadas por Flux

| Parámetro | Formato | Notas
| ------------- | ------------- | ------------- |
| --repository-url | ssh://user@server/repo[.git] o user@server:repo[.git] | Se puede sustituir `git@` por `user@`

> [!NOTE]
> La clave pública generada por Flux se debe agregar a la cuenta de usuario del proveedor de servicios de Git. Si la clave se agrega al repositorio en lugar de a > la cuenta de usuario, use `git@` en lugar de `user@` en la dirección URL. [Ver más detalles](#apply-configuration-from-a-private-git-repository)

#### <a name="use-a-private-git-repo-with-ssh-and-user-provided-keys"></a>Uso de un repositorio de Git privado con claves SSH y proporcionadas por el usuario

| Parámetro | Formato | Notas |
| ------------- | ------------- | ------------- |
| --repository-url  | ssh://user@server/repo[.git] o user@server:repo[.git] | Se puede sustituir `git@` por `user@` |
| --ssh-private-key | Clave codificada en Base64 en [formato PEM](https://aka.ms/PEMformat) | La clave se proporciona directamente |
| --ssh-private-key-file | Ruta de acceso completa al archivo local | Se proporciona la ruta de acceso completa al archivo local que contiene la clave en formato PEM

> [!NOTE]
> Proporcione una clave privada propia de forma directa o en un archivo. La clave debe estar en [formato PEM](https://aka.ms/PEMformat) y terminar con el carácter de nueva línea (\n).  La clave pública asociada se debe agregar a la cuenta de usuario del proveedor de servicios de Git. Si la clave se agrega al repositorio en lugar de a la cuenta de usuario, use `git@` en lugar de `user@`. [Ver más detalles](#apply-configuration-from-a-private-git-repository)

#### <a name="use-a-private-git-host-with-ssh-and-user-provided-known-hosts"></a>Uso de un repositorio de Git privado con SSH y hosts conocidos proporcionados por el usuario

| Parámetro | Formato | Notas |
| ------------- | ------------- | ------------- |
| --repository-url  | ssh://user@server/repo[.git] o user@server:repo[.git] | Se puede sustituir `git@` por `user@` |
| --ssh-known-hosts | Con codificación base64 | Contenido de hosts conocidos proporcionado directamente |
| --ssh-known-hosts-file | Ruta de acceso completa al archivo local | Contenido de hosts conocidos proporcionado en un archivo local

> [!NOTE]
> El operador Flux mantiene una lista de hosts de Git comunes en su archivo de hosts conocidos para autenticar el repositorio de Git antes de establecer la conexión SSH. Si usa un repositorio de Git no común o un host de Git propio, es posible que tenga que proporcionar la clave de host para asegurarse de que Flux puede identificar el repositorio. Puede proporcionar el contenido de los hosts conocidos directamente o en un archivo. [Vea la especificación de formato de contenido de los hosts conocidos](https://aka.ms/KnownHostsFormat).
> Puede usarlo junto con uno de los escenarios de clave SSH descritos anteriormente.

#### <a name="use-a-private-git-repo-with-https"></a>Uso de un repositorio de Git privado con HTTPS

| Parámetro | Formato | Notas |
| ------------- | ------------- | ------------- |
| --repository-url | https://server/repo [.git] | HTTPS con autenticación básica |
| --https-user | Cadena sin codificar o codificada en Base64 | Nombre de usuario HTTPS |
| --https-key | Cadena sin codificar o codificada en Base64 | Token de acceso personal o contraseña de HTTPS

> [!NOTE]
> La autenticación privada de la versión Helm de HTTPS solo se admite con la versión del gráfico del operador Helm > = 1.2.0.  De forma predeterminada se usa la versión 1.2.0.
> La autenticación privada de la versión Helm de HTTPS no se admite actualmente para los clústeres administrados de Azure Kubernetes Services.
> Si necesita Flux para acceder al repositorio de Git mediante el proxy, tendrá que actualizar los agentes de Azure Arc con la configuración de proxy. [Más información](./connect-cluster.md#connect-using-an-outbound-proxy-server)

#### <a name="additional-parameters"></a>Parámetros adicionales

Para personalizar la configuración, estos son otros parámetros que puede usar:

`--enable-helm-operator`: *Opcional* modificador para habilitar la compatibilidad con las implementaciones de gráficos de Helm.

`--helm-operator-params`: *Opcional* valores de gráfico para el operador de Helm (si está habilitado).  Por ejemplo, "--set helm.versions=v3".

`--helm-operator-version`: *Opcional* versión del gráfico para el operador de Helm (si está habilitado). Use la versión "1.2.0" o posterior. Valor predeterminado: "1.2.0".

`--operator-namespace`: *Opcional* nombre para el espacio de nombres del operador. Valor predeterminado: "default". Máx. 23 caracteres.

`--operator-params`: *Opcional* parámetros para el operador. Se debe proporcionar entre comillas simples. Por ejemplo: ```--operator-params='--git-readonly --sync-garbage-collection --git-branch=main' ```

Opciones admitidas en --operator-params

| Opción | Descripción |
| ------------- | ------------- |
| --git-branch  | Rama del repositorio de Git que se va a usar para los manifiestos de Kubernetes. El valor predeterminado es "master". Los repositorios más recientes tienen una rama raíz denominada "main", en cuyo caso debe establecer --git-branch=main. |
| --git-path  | Ruta de acceso relativa dentro del repositorio de Git para que Flux localice manifiestos de Kubernetes. |
| --git-readonly | El repositorio de Git se considerará de solo lectura; Flux no intentará escribir en él. |
| --manifest-generation  | Si está habilitado, Flux buscará .flux.yaml y ejecutará Kustomize u otros generadores de manifiestos. |
| --git-poll-interval  | Período en el que se sondea el repositorio de Git en busca de confirmaciones nuevas. El valor predeterminado es "5m" (5 minutos). |
| --sync-garbage-collection  | Si está habilitado, Flux eliminará los recursos que ha creado, pero que ya no están presentes en Git. |
| --git-label  | Etiqueta para realizar el seguimiento del progreso de la sincronización, que se usa para etiquetar la rama de Git.  El valor predeterminado es "flux-sync". |
| --git-user  | Nombre de usuario para la confirmación de Git. |
| --git-email  | Correo electrónico que se va a usar para la confirmación de Git. |

* Si no se establece "--git-user" o "--git-email" (lo que significa que no quiere que Flux escriba en el repositorio), --git-readonly se establecerá de forma automática (si todavía no lo ha establecido).

Para obtener más información, vea la [documentación de Flux](https://aka.ms/FluxcdReadme).

> [!TIP]
> Es posible crear un elemento sourceControlConfiguration en Azure Portal en la pestaña **GitOps** del recurso de Kubernetes habilitado para Azure Arc.

## <a name="validate-the-sourcecontrolconfiguration"></a>Validación de sourceControlConfiguration

Con la CLI de Azure, valide que `sourceControlConfiguration` se ha creado correctamente.

```azurecli
az k8sconfiguration show --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

Tenga en cuenta que el recurso `sourceControlConfiguration` se actualiza con el estado de cumplimiento, los mensajes y la información de depuración.

**Salida:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
{
  "complianceStatus": {
    "complianceState": "Installed",
    "lastConfigApplied": "2020-12-10T18:26:52.801000+00:00",
    "message": "...",
    "messageLevel": "Information"
  },
  "configurationProtectedSettings": {},
  "enableHelmOperator": false,
  "helmOperatorProperties": {
    "chartValues": "",
    "chartVersion": ""
  },
  "id": "/subscriptions/<sub id>/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1/providers/Microsoft.KubernetesConfiguration/sourceControlConfigurations/cluster-config",
  "name": "cluster-config",
  "operatorInstanceName": "cluster-config",
  "operatorNamespace": "cluster-config",
  "operatorParams": "--git-readonly",
  "operatorScope": "cluster",
  "operatorType": "Flux",
  "provisioningState": "Succeeded",
  "repositoryPublicKey": "...",
  "repositoryUrl": "git://github.com/Azure/arc-k8s-demo.git",
  "resourceGroup": "AzureArcTest",
  "sshKnownHostsContents": null,
  "systemData": {
    "createdAt": "2020-12-01T03:58:56.175674+00:00",
    "createdBy": null,
    "createdByType": null,
    "lastModifiedAt": "2020-12-10T18:30:56.881219+00:00",
    "lastModifiedBy": null,
    "lastModifiedByType": null
},
  "type": "Microsoft.KubernetesConfiguration/sourceControlConfigurations"
}
```

Cuando se crea o actualiza un recurso `sourceControlConfiguration`, se realizan varias operaciones en segundo plano:

1. El `config-agent` de Azure Arc está supervisando a Azure Resource Manager para detectar configuraciones nuevas o actualizadas (`Microsoft.KubernetesConfiguration/sourceControlConfigurations`) y observa la nueva configuración con estado `Pending`.
1. El `config-agent` lee las propiedades de configuración y crea el espacio de nombres de destino.
1. `controller-manager` de Azure Arc prepara una cuenta de servicio de Kubernetes con el permiso adecuado (con ámbito `cluster` o `namespace`) y, después, implementa una instancia de `flux`.
1. Si está usando la opción de SSH con claves generadas por Flux, `flux` genera una clave SSH y registra la clave pública.
1. `config-agent` notifica el estado al recurso `sourceControlConfiguration` en Azure.

Mientras se produce el proceso de aprovisionamiento, `sourceControlConfiguration` se desplazará a través de algunos cambios de estado. Supervise el progreso con el comando `az k8sconfiguration show ...` anterior:

1. `complianceStatus` -> `Pending`: representa los estados inicial y en curso.
1. `complianceStatus` -> `Installed`: `config-agent` ha podido configurar correctamente el clúster e implementar `flux` sin errores
1. `complianceStatus` -> `Failed`: `config-agent` ha detectado un error al implementar `flux`, los detalles deben estar disponibles en el cuerpo de respuesta de `complianceStatus.message`

## <a name="apply-configuration-from-a-private-git-repository"></a>Aplicación de la configuración desde un repositorio de Git privado

Si usa un repositorio de Git privado, debe configurar la clave pública SSH en el repositorio. Puede configurar la clave pública en el repositorio de Git específico o en el usuario de Git que tenga acceso al repositorio. La clave pública SSH será la que proporcione o la que genere Flux.

**Obtención de una clave pública propia**

Si ha generado claves SSH propias, ya dispone de las claves públicas y privadas.

**Obtención de la clave pública mediante la CLI de Azure (útil si Flux genera las claves)**

```console
$ az k8sconfiguration show --resource-group <resource group name> --cluster-name <connected cluster name> --name <configuration name> --cluster-type connectedClusters --query 'repositoryPublicKey' 
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAREDACTED"
```

**Obtención de la clave pública mediante Azure Portal (útil si Flux genera las claves)**

1. En Azure Portal, vaya al recurso de clúster conectado.
2. En la página del recurso, seleccione "GitOps" y vea la lista de configuraciones de este clúster.
3. Seleccione la configuración que usa el repositorio de Git privado.
4. En la parte inferior de la ventana contextual que se abre, copie la **clave pública del repositorio**.

Si usa GitHub, use alguna de las dos opciones siguientes:

**Opción 1: Adición de la clave pública a la cuenta de usuario (se aplica a todos los repositorios de la cuenta)**

1. Abra GitHub y haga clic en el icono del perfil en la esquina superior derecha de la página.
2. Haga clic en **Configuración**.
3. Haga clic en **SSH and GPG keys** (Claves SSH y GPG).
4. Haga clic en **New SSH key** (Nueva clave SSH).
5. Proporcione un título.
6. Pegue la clave pública (menos las comillas).
7. Haga clic en **Add SSH key** (Agregar clave SSH).

**Opción 2: Adición de la clave pública como clave de implementación al repositorio de Git (solo se aplica a este repositorio)**

1. Abra GitHub, navegue hasta repositorio, después a **Settings** (Configuración) y luego a **Deploy keys** (Implementar claves).
2. Haga clic en **Add deploy key** (Agregar clave de implementación).
3. Proporcione un título.
4. Active **Allow write access** (Permitir acceso de escritura).
5. Pegue la clave pública (menos las comillas).
6. Haga clic en **Agregar clave**.

**Si usa un repositorio de Azure DevOps, agregue la clave a las claves SSH**

1. En **User Settings** (Configuración de usuario) en la parte superior derecha (junto a la imagen de perfil), haga clic en **SSH public keys** (Claves públicas de SSH).
1. Seleccione **+ New Key**(+ Nueva clave).
1. Proporcione un nombre.
1. Pegue la clave pública sin las comillas.
1. Haga clic en **Agregar**.

## <a name="validate-the-kubernetes-configuration"></a>Validación de la configuración de Kubernetes

Una vez que `config-agent` ha instalado la instancia de `flux`, los recursos que se mantienen en el repositorio de Git deben empezar a fluir al clúster. Compruebe que se hayan creado los espacios de nombres, las implementaciones y los recursos:

```console
kubectl get ns --show-labels
```

**Salida:**

```console
NAME              STATUS   AGE    LABELS
azure-arc         Active   24h    <none>
cluster-config    Active   177m   <none>
default           Active   29h    <none>
itops             Active   177m   fluxcd.io/sync-gc-mark=sha256.9oYk8yEsRwWkR09n8eJCRNafckASgghAsUWgXWEQ9es,name=itops
kube-node-lease   Active   29h    <none>
kube-public       Active   29h    <none>
kube-system       Active   29h    <none>
team-a            Active   177m   fluxcd.io/sync-gc-mark=sha256.CS5boSi8kg_vyxfAeu7Das5harSy1i0gc2fodD7YDqA,name=team-a
team-b            Active   177m   fluxcd.io/sync-gc-mark=sha256.vF36thDIFnDDI2VEttBp5jgdxvEuaLmm7yT_cuA2UEw,name=team-b
```

Se puede ver que se han creado los espacios de nombres `team-a`, `team-b`, `itops` y `cluster-config`.

El operador `flux` se ha implementado en el espacio de nombres `cluster-config`, como indica `sourceControlConfig`:

```console
kubectl -n cluster-config get deploy  -o wide
```

**Salida:**

```console
NAME             READY   UP-TO-DATE   AVAILABLE   AGE   CONTAINERS   IMAGES                         SELECTOR
cluster-config   1/1     1            1           3h    flux         docker.io/fluxcd/flux:1.16.0   instanceName=cluster-config,name=flux
memcached        1/1     1            1           3h    memcached    memcached:1.5.15               name=memcached
```

## <a name="further-exploration"></a>Exploración adicional

Puede explorar los demás recursos implementados como parte del repositorio de configuración:

```console
kubectl -n team-a get cm -o yaml
kubectl -n itops get all
```

## <a name="delete-a-configuration"></a>Eliminación de una configuración

Elimine `sourceControlConfiguration` mediante la CLI de Azure o Azure Portal.  Después de iniciar el comando de eliminación, el recurso `sourceControlConfiguration` se eliminará inmediatamente en Azure, y la eliminación completa de todos los objetos asociados del clúster se debe producir en un plazo de 10 minutos.  Si `sourceControlConfiguration` está en un estado de error cuando se elimina, la eliminación completa de los objetos asociados puede tardar hasta una hora.

> [!NOTE]
> Después de crear un elemento sourceControlConfiguration con ámbito de espacio de nombres, los usuarios con el enlace de rol `edit` en el espacio de nombres pueden implementar cargas de trabajo en este espacio de nombres. Cuando este `sourceControlConfiguration` con ámbito de espacio de nombres se elimina, el espacio de nombres se deja intacto y no se elimina para evitar la interrupción de estas otras cargas de trabajo.  Si es necesario, puede eliminar este espacio de nombres manualmente con kubectl.
> Los cambios en el clúster que sean el resultado de implementaciones desde el repositorio de Git sometido a seguimiento no se eliminan cuando se elimina `sourceControlConfiguration`.

```azurecli
az k8sconfiguration delete --name cluster-config --cluster-name AzureArcTest1 --resource-group AzureArcTest --cluster-type connectedClusters
```

**Salida:**

```console
Command group 'k8sconfiguration' is in preview. It may be changed/removed in a future release.
```

## <a name="next-steps"></a>Pasos siguientes

- [Uso de Helm con configuración de control de código fuente](./use-gitops-with-helm.md)
- [Uso de Azure Policy para controlar la configuración del clúster](./use-azure-policy.md)