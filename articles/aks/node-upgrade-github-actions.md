---
title: Administración de las actualizaciones de los nodos de AKS con Acciones de GitHub
titleSuffix: Azure Kubernetes Service
description: Aprenda cómo actualizar los nodos de AKS mediante Acciones de GitHub
services: container-service
ms.topic: article
ms.date: 11/27/2020
ms.openlocfilehash: 6876cf1e5044246492e249d8a61060cbeac46f96
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "98217964"
---
# <a name="apply-security-updates-to-azure-kubernetes-service-aks-nodes-automatically-using-github-actions"></a>Aplicación de actualizaciones de seguridad a los nodos de Azure Kubernetes Service (AKS) automáticamente mediante Acciones de GitHub

Las actualizaciones de seguridad son una parte fundamental del mantenimiento de la seguridad y el cumplimiento del clúster de AKS con las últimas correcciones para el sistema operativo subyacente. Estas actualizaciones incluyen las revisiones de seguridad del sistema operativo o las actualizaciones del kernel. Algunas de las actualizaciones requieren un reinicio del nodo para completar el proceso.

La ejecución de `az aks upgrade` proporciona una manera de aplicar actualizaciones sin tiempo de inactividad. El comando administra la aplicación de las actualizaciones más recientes a todos los nodos del clúster, el acordonamiento y el drenaje del tráfico a los nodos, el reinicio de los nodos y la habilitación del tráfico a los nodos actualizados. Si actualiza los nodos con un método diferente, AKS no los reiniciará automáticamente.

> [!NOTE]
> La principal diferencia entre `az aks upgrade` cuando se usa con la marca de `--node-image-only` es que, cuando se utiliza, solo se actualizarán las imágenes de nodo. Si se omite, se actualizarán las imágenes de nodo y la versión del plano de control de Kubernetes. Puede comprobar [los documentos para las actualizaciones administradas en los nodos][managed-node-upgrades-article] y [los documentos para las actualizaciones de clúster][cluster-upgrades-article] para ver información más detallada.

Todos los nodos de Kubernetes se ejecutan en una máquina virtual (VM) de Azure estándar. Estas máquinas virtuales pueden estar basadas en Windows o Linux. Las máquinas virtuales basadas en Linux utilizan una imagen de Ubuntu, con el sistema operativo configurado para buscar actualizaciones automáticamente cada noche.

Cuando se utiliza el comando `az aks upgrade`, la CLI de Azure crea un aumento de los nuevos nodos con las actualizaciones de kernel y de seguridad más recientes, estos nodos se acordonan inicialmente para evitar que las aplicaciones estén programadas en función de ellos hasta que se complete la actualización. Una vez finalizada, Azure acordona (hace que el nodo no esté disponible para la programación de nuevas cargas de trabajo), drena (mueve las cargas de trabajo existentes a otro nodo) los nodos más antiguos y desacordona los nuevos, con lo que se transfieren de forma efectiva todas las aplicaciones programadas a los nuevos nodos.

Este proceso es mejor que la actualización manual de kernels basados en Linux, ya que Linux requiere un reinicio cuando se instala una nueva actualización del kernel. Si actualiza el sistema operativo manualmente, también debe reiniciar la máquina virtual, acordonando y drenando manualmente todas las aplicaciones.

En este artículo se muestra cómo automatizar el proceso de actualización de los nodos de AKS. Utilizará Acciones de GitHub y la CLI de Azure para crear una tarea de actualización basada en `cron` que se ejecuta automáticamente.

## <a name="before-you-begin"></a>Antes de empezar

En este artículo se supone que ya tiene un clúster de AKS. Si necesita un clúster de AKS, consulte el inicio rápido de AKS [mediante la CLI de Azure][aks-quickstart-cli] o [mediante Azure Portal][aks-quickstart-portal].

También es preciso que esté instalada y configurada la versión 2.0.59 de la CLI de Azure u otra versión posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure][install-azure-cli].

En este artículo también se da por hecho que tiene una cuenta de [GitHub][github] en la que crear acciones.

## <a name="create-a-timed-github-action"></a>Creación de una acción de GitHub con hora

`cron` es una utilidad que permite ejecutar un conjunto de comandos, o trabajo, en una programación automatizada. Para crear un trabajo, y que actualice los nodos de AKS en una programación automatizada, necesitará un repositorio para hospedar las acciones. Normalmente, las acciones de GitHub se configuran en el mismo repositorio que la aplicación, pero puede utilizar cualquier repositorio. En este artículo, vamos a utilizar el [repositorio de su perfil][profile-repository]. Si no tiene ninguno, cree un repositorio con el mismo nombre que el nombre de usuario de GitHub.

1. Vaya al repositorio en GitHub.
1. Haga clic en la pestaña **Actions** (Acciones) de la parte superior de la página.
1. Si ya ha configurado un flujo de trabajo en este repositorio, se le redirigirá a la lista de ejecuciones completadas, en este caso, haga clic en el botón **New Workflow** (Nuevo flujo de trabajo). Si este es su primer flujo de trabajo en el repositorio, GitHub le presentará algunas plantillas de proyecto, haga clic en el vínculo **Set up a workflow yourself** (Configurar un flujo de trabajo personalmente) debajo del texto de la descripción.
1. Cambie el flujo de trabajo `name` y las etiquetas `on` de forma similar a lo siguiente. Las Acciones de GitHub utilizan la misma [sintaxis cron de POSIX][cron-syntax] que cualquier sistema basado en Linux. En esta programación, estamos indicando al flujo de trabajo que se ejecute cada 15 días a las 3 de la mañana.

    ```yml
    name: Upgrade cluster node images
    on:
      schedule:
        - cron: '0 3 */15 * *'
    ```

1. Cree un nuevo trabajo con lo siguiente. Este trabajo se denomina `upgrade-node`, se ejecuta en un agente de Ubuntu y se conectará a su cuenta de la CLI de Azure a fin de ejecutar los pasos necesarios para actualizar los nodos.

    ```yml
    name: Upgrade cluster node images

    on:
      schedule:
        - cron: '0 3 */15 * *'

    jobs:
      upgrade-node:
        runs-on: ubuntu-latest
    ```

## <a name="set-up-the-azure-cli-in-the-workflow"></a>Configuración de la CLI de Azure en el flujo de trabajo

En la clave `steps`, definirá todo el trabajo que ejecutará el flujo de trabajo para actualizar los nodos.

Descarga e inicio de sesión en la CLI de Azure

1. En el lado derecho de la pantalla Acciones de GitHub, busque la *barra de búsqueda de marketplace* y escriba **"Azure Login"** (Inicio de sesión de Azure).
1. Obtendrá como resultado una acción llamada **Azure Login** (Inicio de sesión de Azure) publicada **por Azure**:

      :::image type="content" source="media/node-upgrade-github-actions/azure-login-search.png" alt-text="Resultados de la búsqueda que muestran dos líneas, la primera acción se denomina &quot;Azure login&quot; (Inicio de sesión de Azure) y la segunda &quot;Azure Container Registry login&quot; (Inicio de sesión de Azure Container Registry)":::

1. Haga clic en **Azure Login** (Inicio de sesión de Azure). En la siguiente pantalla, haga clic en el **icono de copiar** en la parte superior derecha del ejemplo de código.

    :::image type="content" source="media/node-upgrade-github-actions/azure-login.png" alt-text="Panel de resultados de la acción Azure Login (Inicio de sesión de Azure) con el ejemplo de código siguiente, un cuadrado rojo alrededor de un icono de copiar destaca el punto del clic":::

1. Pegue lo siguiente bajo la clave `steps`:

      ```yml
      name: Upgrade cluster node images

      on:
        schedule:
          - cron: '0 3 */15 * *'

      jobs:
        upgrade-node:
          runs-on: ubuntu-latest

          steps:
            - name: Azure Login
              uses: Azure/login@v1.1
              with:
                creds: ${{ secrets.AZURE_CREDENTIALS }}
      ```

1. En la CLI de Azure, ejecute el siguiente comando para generar un nuevo nombre de usuario y contraseña.

    ```azurecli-interactive
    az ad sp create-for-rbac -o json
    ```

    La salida debe ser similar al siguiente json:

    ```output
    {
      "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "displayName": "azure-cli-xxxx-xx-xx-xx-xx-xx",
      "name": "http://azure-cli-xxxx-xx-xx-xx-xx-xx",
      "password": "xXxXxXxXx",
      "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. **En una nueva ventana del explorador** vaya al repositorio de GitHub y abra la pestaña **Settings** (Configuración) del repositorio. Haga clic en **Secrets** (Secretos) y, a continuación, en **New Repository Secret** (Nuevo secreto del repositorio).
1. En *Nombre*, use `AZURE_CREDENTIALS`.
1.  En *Value* (Valor), agregue todo el contenido de la salida del paso anterior en el que ha creado un nuevo nombre de usuario y contraseña.

    :::image type="content" source="media/node-upgrade-github-actions/azure-credential-secret.png" alt-text="Formulario que muestra AZURE_CREDENTIALS como título del secreto y la salida del comando ejecutado pegado como JSON":::

1. Haga clic en **Add Secret** (Agregar secreto).

La CLI utilizada por la acción se registrará en su cuenta de Azure y estará preparada para ejecutar comandos.

Para crear los pasos a fin de ejecutar los comandos de la CLI de Azure:

1. Vaya a la **página de búsqueda** en el *marketplace de GitHub* en el lado derecho de la pantalla y busque *Azure CLI Action* (Acción de la CLI de Azure). Elija *Azure CLI Action by Azure* (Acción de la CLI de Azure por Azure).

    :::image type="content" source="media/node-upgrade-github-actions/azure-cli-action.png" alt-text="Resultado de la búsqueda de &quot;Azure CLI Action&quot; (Acción de la CLI de Azure) con el primer resultado que se muestra realizado por Azure":::

1. Haga clic en el botón de copiar en el *resultado de marketplace de GitHub* y pegue el contenido de la acción en el editor principal, debajo del paso *Azure Login* (Inicio de sesión de Azure), de forma similar a lo siguiente:

    ```yml
    name: Upgrade cluster node images

    on:
      schedule:
        - cron: '0 3 */15 * *'

    jobs:
      upgrade-node:
        runs-on: ubuntu-latest

        steps:
          - name: Azure Login
            uses: Azure/login@v1.1
            with:
              creds: ${{ secrets.AZURE_CREDENTIALS }}
          - name: Upgrade node images
            uses: Azure/cli@v1.0.0
            with:
              inlineScript: az aks upgrade -g {resourceGroupName} -n {aksClusterName} --node-image-only --yes
    ```

    > [!TIP]
    > Puede desacoplar los parámetros `-g` y `-n` del comando agregándolos a secretos similares a los pasos anteriores. Reemplace los marcadores de posición `{resourceGroupName}` y `{aksClusterName}` por sus homólogos secretos, por ejemplo, `${{secrets.RESOURCE_GROUP_NAME}}` y `${{secrets.AKS_CLUSTER_NAME}}`

1. Cambie el nombre del archivo `upgrade-node-images`.
1. Haga clic en **Start Commit** (Iniciar confirmación), agregue un título de mensaje y guarde el flujo de trabajo.

Una vez creada la confirmación, el flujo de trabajo se guardará y estará preparado para su ejecución.

> [!NOTE]
> Para actualizar un solo grupo de nodos en lugar de todos los grupos de nodos del clúster, agregue el parámetro `--name` al comando `az aks nodepool upgrade` para especificar el nombre del grupo de nodos. Por ejemplo:
> ```azurecli-interactive
> az aks nodepool upgrade -g {resourceGroupName} --cluster-name {aksClusterName} --name {{nodePoolName}} --node-image-only
> ```

## <a name="run-the-github-action-manually"></a>Ejecución manual de la acción de GitHub

Puede ejecutar el flujo de trabajo manualmente, además de la ejecución programada, agregando un nuevo desencadenador de `on` denominado `workflow_dispatch`. El archivo terminado debería tener un aspecto similar al YAML siguiente:

```yml
name: Upgrade cluster node images

on:
  schedule:
    - cron: '0 3 */15 * *'
  workflow_dispatch:

jobs:
  upgrade-node:
    runs-on: ubuntu-latest

    steps:
      - name: Azure Login
        uses: Azure/login@v1.1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}

      # Code for upgrading one or more node pools
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte las [notas de la versión de AKS](https://github.com/Azure/AKS/releases) para más información sobre las imágenes de nodo más recientes.
- Obtenga información sobre cómo actualizar la versión de Kubernetes con [Actualización de un clúster de AKS][cluster-upgrades-article].
- Obtenga más información sobre varios grupos de nodos y cómo actualizar los grupos de nodos con [Creación y administración de varios grupos de nodos][use-multiple-node-pools].
- Más información sobre los [grupos de nodos del sistema][system-pools].
- Para más información sobre cómo ahorrar costos mediante instancias de acceso puntual, consulte el artículo sobre la [incorporación de un grupo de nodos de acceso puntual a AKS][spot-pools].

<!-- LINKS - external -->
[github]: https://github.com
[profile-repository]: https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-your-github-profile/about-your-profile
[cron-syntax]: https://pubs.opengroup.org/onlinepubs/9699919799/utilities/crontab.html#tag_20_25_07

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[managed-node-upgrades-article]: node-image-upgrade.md
[cluster-upgrades-article]: upgrade-cluster.md
[system-pools]: use-system-pools.md
[spot-pools]: spot-node-pool.md
[use-multiple-node-pools]: use-multiple-node-pools.md
