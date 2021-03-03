---
title: 'Tutorial: Preparación de una aplicación web para Azure Communication Services (Node.js)'
titleSuffix: An Azure Communication Services tutorial
description: Obtenga información sobre cómo crear una aplicación web de línea de base que admita Azure Communication Services.
author: nmurav
services: azure-communication-services
ms.author: nmurav
ms.date: 01/03/2012
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: cd535227b421f4fb56dac3afb37033e3d77f75f7
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2021
ms.locfileid: "101691542"
---
# <a name="tutorial-prepare-a-web-app-for-azure-communication-services-nodejs"></a>Tutorial: Preparación de una aplicación web para Azure Communication Services (Node.js)

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Communication Services le permite agregar comunicaciones en tiempo real a las aplicaciones. En este tutorial aprenderá a configurar una aplicación web que admita Azure Communication Services. Este es un tutorial de introducción destinado a los nuevos desarrolladores que desean empezar a trabajar con comunicaciones en tiempo real.

Al final de este tutorial, tendrá una aplicación web de línea de base configurada con las bibliotecas cliente de Azure Communication Services que puede usar para comenzar a crear la solución de comunicaciones en tiempo real.

No dude en visitar la página de GitHub de [Azure Communication Services](https://github.com/Azure/communication) para proporcionar comentarios.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Configuración del entorno de desarrollo
> * Configuración de un servidor web local
> * Incorporación de los paquetes de Azure Communication Services al sitio web
> * Publicación del sitio web en Azure Static Web Apps

## <a name="prerequisites"></a>Requisitos previos

- Una cuenta de Azure con una suscripción activa. Para más información, consulte [Creación de una cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Tenga en cuenta que la cuenta gratuita le proporciona 200 $ en créditos de Azure para probar cualquier combinación de servicios.
- [Visual Studio Code](https://code.visualstudio.com/): lo usaremos para editar código en el entorno de desarrollo local.
- [webpack](https://webpack.js.org/): se usará para agrupar y hospedar el código de forma local.
- [Node.js](https://nodejs.org/en/): se usará para instalar y administrar las dependencias, como las bibliotecas cliente de Azure Communication Services y webpack.
- [nvm y npm](/windows/nodejs/setup-on-windows) para llevar a cabo el control de versiones.
- La [Extensión de Azure Storage](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) para Visual Studio Code. Esta extensión es necesaria para publicar la aplicación en Azure Storage. [Más información sobre el hospedaje de sitios web estáticos en Azure Storage](../../storage/blobs/storage-blob-static-website.md).
- La [extensión de Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). La extensión permite implementar sitios web (similar a la anterior), pero con la opción de configurar de forma totalmente administrada la integración continua y entrega continua (CI/CD).
- La [extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para crear sus propias aplicaciones sin servidor. Por ejemplo, puede hospedar la aplicación de autenticación en Azure Functions.
- Un recurso activo de Communication Services y una cadena de conexión. [Cree un recurso de Communication Services](../quickstarts/create-communication-resource.md).
- Un token de acceso de usuario. Consulte [Inicio rápido: Creación y administración de tokens de acceso](../quickstarts/access-tokens.md?pivots=programming-language-javascript) o [Creación de un servicio de autenticación de confianza mediante Azure Functions](./trusted-service-tutorial.md) para obtener instrucciones.


## <a name="configure-your-development-environment"></a>Configuración del entorno de desarrollo

El entorno de desarrollo local se configurará de la siguiente manera:

:::image type="content" source="./media/step-one-pic-one.png" alt-text="Arquitectura del entorno del desarrollador":::


### <a name="install-nodejs-nvm-and-npm"></a>Instalación de Node.js, nvm y npm

Utilizaremos Node.js para descargar e instalar varias dependencias que necesitamos para la aplicación cliente. Lo usaremos para generar archivos estáticos que luego hospedaremos en Azure, por lo que no tiene que preocuparse por configurarlos en el servidor.

Los desarrolladores de Windows pueden seguir el tutorial [Configuración del entorno de desarrollo de Node.js directamente en Windows](/windows/nodejs/setup-on-windows) para configurar Node, nvm y npm. 

Hemos probado este tutorial con la versión LTS 12.20.0. Después de instalar nvm, use el siguiente comando de PowerShell para implementar la versión que desea usar:

```PowerShell
nvm list available
nvm install 12.20.0
nvm use 12.20.0
```

:::image type="content" source="./media/step-one-pic-two.png" alt-text="Trabajo con nvm para implementar Node.js":::

### <a name="configure-visual-studio-code"></a>Configuración de Visual Studio Code

Puede descargar [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas compatibles](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

### <a name="create-a-workspace-for-your-azure-communication-services-projects"></a>Creación de un área de trabajo para los proyectos de Azure Communication Services

Cree una nueva carpeta para almacenar los archivos del proyecto, por ejemplo: `C:\Users\Documents\ACS\CallingApp`. En Visual Studio Code, haga clic en "File" > "Add Folder to Workspace" (Archivo > Agregar carpeta al área de trabajo) y agregue la carpeta al área de trabajo.

:::image type="content" source="./media/step-one-pic-three.png" alt-text="Creación de un área de trabajo nueva":::

Vaya a "Explorador" en Visual Studio Code en el panel izquierdo y verá la carpeta "CallingApp" en el área de trabajo "Sin título".

:::image type="content" source="./media/step-one-pic-four.png" alt-text="Explorador":::

No dude en actualizar el nombre del área de trabajo si lo desea. Para validar la versión de Node.js, haga clic con el botón derecho en la carpeta "CallingApp" y seleccione "Open in Integrated Terminal" (Abrir en el terminal integrado).

:::image type="content" source="./media/step-one-pic-five.png" alt-text="Apertura de un terminal":::

En el terminal, escriba el siguiente comando para validar la versión de node.js instalada en el paso anterior:

```JavaScript
node --version
```

:::image type="content" source="./media/step-one-pic-six.png" alt-text="Validación de la versión de Node.js":::

### <a name="install-azure-extensions-for-visual-studio-code"></a>Instalación de las extensiones de Azure para Visual Studio Code

Instale la [extensión de Azure Storage](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) mediante el marketplace de Visual Studio o con Visual Studio Code (Ver > Extensiones > Azure Storage).

:::image type="content" source="./media/step-one-pic-seven.png" alt-text="Instalación de la extensión de Azure Storage 1":::

Siga los mismos pasos para las extensiones de [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) y [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice).


## <a name="set-up-a-local-webserver"></a>Configuración de un servidor web local

### <a name="create-a-new-npm-package"></a>Creación de un nuevo paquete de npm

En el terminal, desde la ruta de acceso de la carpeta del área de trabajo, escriba:

``` console
npm init -y
```

Este comando inicializa un nuevo paquete de npm y agrega el archivo `package.json` a la carpeta raíz del proyecto.

:::image type="content" source="./media/step-one-pic-eight.png" alt-text="package.json":::

Puede encontrar documentación adicional sobre el comando npm init [aquí](https://docs.npmjs.com/cli/v6/commands/npm-init).

### <a name="install-webpack"></a>Instalación de webpack

[webpack](https://webpack.js.org/) permite agrupar código en archivos estáticos que se pueden implementar en Azure. También tiene un servidor de desarrollo, que vamos a configurar para usarlo con el ejemplo de llamada.

Para instalar webpack, escriba lo siguiente en el terminal:

``` Console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Este tutorial se ha probado con las versiones especificadas anteriormente. Al especificar `-dev`, se indica al administrador de paquetes que esta dependencia es para fines de desarrollo y no se debe incluir en el código que se implementa en Azure.

Verá que se han agregado dos nuevos paquetes al archivo `package.json` como "devDependencies". Los paquetes se instalarán en el directorio `./CallingApp/node_modules/`.

:::image type="content" source="./media/step-one-pic-ten.png" alt-text="Configuración de webpack":::

### <a name="configure-the-development-server"></a>Configuración del servidor de desarrollo

La ejecución de una aplicación estática (como el archivo `index.html`) desde el explorador usa el protocolo `file://`. Para que los módulos de npm funcionen correctamente, se necesitará el protocolo HTTP mediante el uso de webpack como servidor de desarrollo local.

Crearemos dos configuraciones: una para desarrollo y otra para producción. Los archivos preparados para producción estarán minificados, lo que significa que se quitarán los caracteres y los espacios en blanco sin usar. Esto es adecuado para escenarios de producción en los que se debe minimizar la latencia o cuando se debe ofuscar el código.

Usaremos la herramienta `webpack-merge` para trabajar con [diferentes archivos de configuración para webpack](https://webpack.js.org/guides/production/).

Comencemos con el entorno de desarrollo. En primer lugar, es necesario instalar `webpack merge`. Ejecute lo siguiente en su terminal:

```Console
npm install --save-dev webpack-merge
```

En el archivo `package.json`, puede ver una dependencia más agregada a la sección "devDependencies".

En el paso siguiente, es necesario crear un nuevo archivo `webpack.common.js` y agregar el código siguiente:

```JavaScript
const path = require('path');
module.exports ={
    entry: './app.js',
    output: {
        filename:'app.js',
        path: path.resolve(__dirname, 'dist'),
    }     
}
```

A continuación, agregaremos dos archivos más, uno para cada configuración:

* webpack.dev.js
* webpack.prod.js

En el paso siguiente, es necesario modificar el archivo `webpack.dev.js`. Agregue el siguiente código al archivo:

```JavaScript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
    mode: 'development',
    devtool: 'inline-source-map',
});
```
En esta configuración, se importan los parámetros comunes desde `webpack.common.js`, se combinan los dos archivos, se establece el modo en "development" (desarrollo) y se configura SourceMap como "inline-source-map".

El modo de desarrollo indica a webpack que no minifique los archivos y no genere archivos de producción optimizados. Puede encontrar documentación detallada sobre los modos de webpack [aquí](https://webpack.js.org/configuration/mode/).

Las opciones del mapa de origen se enumeran [aquí](https://webpack.js.org/configuration/devtool/#root). La configuración del mapa de origen facilita la depuración mediante el explorador.

:::image type="content" source="./media/step-one-pic-11.png" alt-text="Configuración de webpack":::

Para ejecutar el servidor de desarrollo, vaya a `package.json` y agregue el código siguiente en la sección scripts:

```JavaScript
    "build:dev": "webpack-dev-server --config webpack.dev.js"
```

El archivo debe tener ahora el siguiente aspecto:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

Ha agregado el comando que se puede usar desde npm. 

:::image type="content" source="./media/step-one-pic-12.png" alt-text="Modificación de package.json":::

### <a name="testing-the-development-server"></a>Prueba del servidor de desarrollo

 En Visual Studio Code, cree tres archivos en el proyecto:

* `index.html`
* `app.js`
* `app.css` (opcional, le permite aplicar estilos a la aplicación)

Pegue lo siguiente en `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My first ACS application</title>
    <link rel="stylesheet" href="./app.css"/>
    <script src="./app.js" defer></script>
</head>
<body>
    <h1>Hello from ACS!</h1>
</body>
</html>
```
:::image type="content" source="./media/step-one-pic-13.png" alt-text="Archivo HTML":::

Agregue el siguiente código a `app.js`:

```JavaScript
alert('Hello world alert!');
console.log('Hello world console!');
```
Agregue el siguiente código a `app.css`:

```CSS
html {
    font-family: sans-serif;
  }
```
No se olvide de guardar. Si un archivo no se ha guardado, aparecen puntos blancos junto a su nombre en el Explorador de archivos.

 :::image type="content" source="./media/step-one-pic-14.png" alt-text="Archivo app.js con el código JS":::

Al abrir esta página, debería ver que el mensaje se muestra con una alerta y dentro de la consola del explorador.

:::image type="content" source="./media/step-one-pic-15.png" alt-text="Archivo app.css":::

Use el siguiente comando de terminal para probar la configuración de desarrollo:

```Console
npm run build:dev
```

La consola mostrará el lugar en el que se ejecuta el servidor. De forma predeterminada, es `http://localhost:8080`. El comando build:dev es el comando que agregamos al archivo `package.json` anteriormente.

 :::image type="content" source="./media/step-one-pic-16.png" alt-text="Inicio de un servidor de desarrollo":::
 
 Vaya a la dirección en el explorador y debería ver la página y la alerta configuradas en los pasos anteriores.
 
  :::image type="content" source="./media/step-one-pic-17.png" alt-text="Página HTML":::
  
 
Mientras se ejecuta el servidor, puede cambiar el código, y el servidor y la página HTML se recargarán automáticamente. 

A continuación, vaya al archivo `app.js` en Visual Studio Code y elimine `alert('Hello world alert!');`. Guarde el archivo y compruebe que la alerta desaparece del explorador.

Para detener el servidor, puede ejecutar `Ctrl+C` en el terminal. Para iniciar el servidor, escriba `npm run build:dev` en cualquier momento.

## <a name="add-the-azure-communication-services-packages"></a>Incorporación de los paquetes de Azure Communication Services

Use el comando `npm install` para instalar la biblioteca cliente de llamadas de Azure Communication Services para JavaScript.

```Console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Esta acción agregará los paquetes comunes y de llamada de Azure Communication Services como dependencias del paquete. Verá dos nuevos paquetes que se han agregado al archivo `package.json`. Puede encontrar más información sobre el comando `npm install` [aquí](https://docs.npmjs.com/cli/v6/commands/npm-install).

:::image type="content" source="./media/step-one-pic-nine.png" alt-text="Instalación de los paquetes de Azure Communication Services":::

El equipo de Azure Communication Services proporciona estos paquetes e incluye las bibliotecas de autenticación y de llamada. El comando "--save" indica que la aplicación depende de estos paquetes para su uso en producción y se incluirá en la sección `dependencies` del archivo `package.json`. Al compilar la aplicación para producción, los paquetes se incluirán en el código de producción.


## <a name="publish-your-website-to-azure-static-websites"></a>Publicación del sitio web en Azure Static Web Apps

### <a name="create-a-configuration-for-production-deployment"></a>Creación de una configuración para la implementación de producción

Agregue el código siguiente al archivo `webpack.prod.js`:

```JavaScript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
 ```

Tenga en cuenta que esta configuración se combinará con el archivo webpack.common.js (en el que se especifica el archivo de entrada y dónde se almacenan los resultados) y se establecerá el modo en "production" (producción).
 
En `package.json`, agregue el código siguiente:

```JavaScript
"build:prod": "webpack --config webpack.prod.js" 
```

El archivo debería tener este aspecto:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js",
    "build:prod": "webpack --config webpack.prod.js" 
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@azure/communication-calling": "^1.0.0-beta.3",
    "@azure/communication-common": "^1.0.0-beta.3"
  },
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3",
    "webpack-merge": "^5.7.3"
  }
}
```

 :::image type="content" source="./media/step-one-pic-20.png" alt-text="Archivos configurados":::


En el terminal, ejecute:

```Console
npm run build:prod
```

El comando creará la carpeta `dist` y el archivo estático `app.js` listo para la producción. 

 :::image type="content" source="./media/step-one-pic-21.png" alt-text="Compilación de producción":::
 
 
### <a name="deploy-your-app-to-azure-storage"></a>Implementación de la aplicación en Azure Storage
 
Copie los archivos `index.html` y `app.css` en la carpeta `dist`.

En la carpeta `dist`, cree un nuevo archivo y asígnele el nombre `404.html`. Copie el código de marcado siguiente en ese archivo:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./app.css"/>
    <title>Document</title>
</head>
<body>
    <h1>The page does not exists.</h1>
</body>
</html>
```

Guarde el archivo (Ctrl + S).

Haga clic con el botón derecho y seleccione Deploy to Static Website via Azure Storage (Implementar en sitio web estático mediante Azure Storage).

:::image type="content" source="./media/step-one-pic-22.png" alt-text="Inicio de la implementación en Azure":::
 
En el campo `Select subscription` (Seleccionar suscripción), seleccione "Sign in to Azure" (Iniciar sesión en Azure) (o"Create a Free Azure Account" [Crear una cuenta de Azure gratis] si no ha creado una suscripción antes).
 
:::image type="content" source="./media/step-one-pic-23.png" alt-text="Inicio de sesión en Azure":::
 
Seleccione `Create new Storage Account` > `Advanced`:

 :::image type="content" source="./media/step-one-pic-24.png" alt-text="Creación del grupo de la cuenta de almacenamiento":::
 
 Proporcione el nombre de la cuenta de almacenamiento:
 
 :::image type="content" source="./media/step-one-pic-25.png" alt-text="Adición de un nombre para la cuenta":::
 
Si es necesario, cree un nuevo grupo de recursos:
 
  :::image type="content" source="./media/step-one-pic-26.png" alt-text="Creación de un nuevo grupo":::
  
  Responda "Yes" (Sí) a la pregunta "Would you like to enable static website hosting?" (¿Desea habilitar el hospedaje de sitios web estáticos?).
  
  :::image type="content" source="./media/step-one-pic-27.png" alt-text="Selección de la opción para habilitar el hospedaje de sitios web estáticos":::
  
Acepte el nombre de archivo predeterminado en "Enter the index document name" (Especificar el nombre del documento de índice), ya que hemos creado el archivo `index.html`.

Escriba `404.html` para "Enter the 404 error document path" (Especificar la ruta de acceso del documento de error 404).  
  
Seleccione la ubicación de la aplicación. La ubicación que seleccione definirá el procesador multimedia que se usará en la futura aplicación de llamadas en las llamadas de grupo. 

Azure Communication Services selecciona el procesador multimedia en función de la ubicación de la aplicación.

:::image type="content" source="./media/step-one-pic-28.png" alt-text="Seleccionar ubicación":::
  
Espere hasta que se creen el recurso y el sitio web. 
 
Haga clic en "Browse to website" (Ir al sitio web):

:::image type="content" source="./media/step-one-pic-29.png" alt-text="Implementación completada":::
 
Desde las herramientas de desarrollo del explorador, puede inspeccionar el código fuente y ver el archivo, preparado para producción.
 
:::image type="content" source="./media/step-one-pic-30.png" alt-text="Sitio web":::

Vaya a [Azure Portal](https://portal.azure.com/#home), seleccione el grupo de recursos, seleccione la aplicación que ha creado y vaya a `Settings` > `Static website` (Configuración > Sitio web estático). Puede ver que los sitios web estáticos están habilitados y anotar el punto de conexión principal, el documento de índice y el documento de la ruta de acceso de error.

:::image type="content" source="./media/step-one-pic-31.png" alt-text="Selección del sitio web estático":::

En "Blob service", seleccione "Contenedores" y verá dos contenedores creados, uno para los registros ($logs) y otro para el contenido del sitio web ($web).

:::image type="content" source="./media/step-one-pic-32.png" alt-text="Configuración del contenedor":::

Si va a `$web`, verá los archivos que ha creado en Visual Studio y que se han implementado en Azure. 

:::image type="content" source="./media/step-one-pic-33.png" alt-text="Implementación":::

Puede volver a implementar la aplicación desde Visual Studio Code en cualquier momento.

Ya está listo para crear su primera aplicación web de Azure Communication Services.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de la llamada de voz a una aplicación](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Puede que también le interese:

- [Incorporación de chat a una aplicación](../quickstarts/chat/get-started.md)
- [Creación de tokens de acceso de usuario](../quickstarts/access-tokens.md)
- [Información sobre la arquitectura de cliente y servidor](../concepts/client-and-server-architecture.md)
- [Información sobre la autenticación](../concepts/authentication.md)