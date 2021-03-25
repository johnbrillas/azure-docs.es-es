---
title: Acciones de menú de MMC en StorSimple Snapshot Manager | Microsoft Docs
description: Describe cómo usar las acciones estándar de menú de Microsoft Management Console (MMC) en Administrador de instantáneas StorSimple.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 78ef81af-0d3a-4802-be54-ad192f9ac8a6
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 01064c3668b673baea7aedd9a65c92b03c48dc10
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2021
ms.locfileid: "90056010"
---
# <a name="use-the-mmc-menu-actions-in-storsimple-snapshot-manager"></a>Uso de las acciones del menú de MMC en Administrador de instantáneas StorSimple

## <a name="overview"></a>Información general
En Administrador de instantáneas StorSimple, verá las siguientes acciones en todos los menús de acción y todas las variaciones del panel **Acciones** .

* Ver
* Nueva ventana desde aquí 
* Actualizar 
* Exportar lista 
* Ayuda 

Estas acciones son parte de Microsoft Management Console (MMC) y no son específicas de Administrador de instantáneas StorSimple. Este tutorial describe estas acciones y explica cómo usar cada una de ellas en Administrador de instantáneas StorSimple.

## <a name="view"></a>Ver
Puede usar la opción **Ver** para cambiar la vista del panel **Resultados** y para cambiar la vista de la ventana de la consola. 

#### <a name="to-change-the-results-pane-view"></a>Para cambiar la vista del panel de resultados
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, haga clic con el botón secundario en un nodo o expanda el nodo y haga clic con el botón secundario en un elemento del panel **Resultados** y luego haga clic en la opción **Ver**. 
3. Para agregar o quitar las columnas que aparecen en el panel **Resultados**, haga clic en **Agregar o quitar columnas**. Aparecerá el cuadro de diálogo **Agregar o quitar columnas**.
   
    ![Incorporación o eliminación de columnas del panel de resultados](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Add_remove_columns.png) 
4. Complete el formulario de la siguiente manera:
   
   * Seleccione elementos en la lista de columnas **Disponibles** y haga clic en **Agregar** para agregarlas a la lista **Columnas mostradas**. 
   * Haga clic en los elementos de la lista **Columnas mostradas** y haga clic en **Quitar** para quitarlos de la lista. 
   * Seleccione un elemento en la lista de columnas **Mostradas** y haga clic en **Subir** o en **Bajar** para subir o bajar el elemento en la lista. 
   * Haga clic en **Restaurar valores predeterminados** para volver a la configuración predeterminada del panel **Resultados**. 
5. Cuando haya terminado con las selecciones, haga clic en **Aceptar**. 

#### <a name="to-change-the-console-window-view"></a>Para cambiar la vista de la ventana de consola
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, haga clic con el botón secundario en cualquier nodo, haga clic en **Ver** y luego haga clic en **Personalizar**. Aparece el cuadro de diálogo **Personalizar**.
   
    ![Personalización de la ventana de consola](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Customize.png) 
3. Active o desactive las casillas para mostrar u ocultar los elementos en la ventana de consola. Cuando haya terminado con las selecciones, haga clic en **Aceptar**.

## <a name="new-window-from-here"></a>Nueva ventana desde aquí
Puede usar la opción **Nueva ventana desde aquí** para abrir una nueva ventana de consola.

#### <a name="to-open-a-new-console-window"></a>Para abrir una nueva ventana de consola
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, haga clic con el botón secundario en cualquier nodo y luego haga clic en **Nueva ventana desde aquí**. 
   
    Aparecerá una nueva ventana que muestra solo el ámbito seleccionado. Por ejemplo, si haga clic con el botón secundario en el nodo **Directivas de copia de seguridad**, la nueva ventana le mostrará solo el nodo **Directivas de copia de seguridad** en el panel **Ámbito** y una lista de directivas de copia de seguridad definidas en el panel **Resultados**. Consulte el ejemplo siguiente.
   
    ![Nueva ventana desde aquí](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_NewWindow.png) 

## <a name="refresh"></a>Actualizar
Puede usar la acción **Actualizar** para actualizar la ventana de la consola.

#### <a name="to-update-the-console-window"></a>Para actualizar la ventana de consola
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, haga clic con el botón secundario en un nodo o expanda el nodo y haga clic con el botón secundario en un elemento del panel **Resultados** y luego haga clic en **Actualizar**. 

## <a name="export-list"></a>Exportar lista
Puede usar la acción **Exportar lista** para guardar una lista en un archivo de valores separados por comas (CSV). Por ejemplo, puede exportar la lista de directivas de copia de seguridad o el catálogo de copias de seguridad. Después, puede importar el archivo CSV en una aplicación de hoja de cálculo para su análisis.

#### <a name="to-save-a-list-in-a-comma-separated-value-csv-file"></a>Para guardar una lista en un archivo de valores separados por comas (CSV)
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple. 
2. En el panel **Ámbito**, haga clic con el botón secundario en un nodo o expanda el nodo y haga clic con el botón secundario en un elemento del panel **Resultados** y luego haga clic en **Exportar lista**. 
3. Aparecerá el cuadro de diálogo **Exportar lista**. Complete el formulario de la siguiente manera: 
   
   1. En el cuadro **Nombre de archivo**, escriba un nombre para el archivo CSV o haga clic en la flecha para seleccionarlo en la lista desplegable.
   2. En el cuadro **Guardar como tipo**, haga clic en la flecha y seleccione un tipo de archivo en la lista desplegable.
   3. Para guardar solo los elementos seleccionados, seleccione las filas y luego haga clic en la casilla **Guardar solo las filas seleccionadas**. Para guardar todas las listas exportadas, desactive la casilla **Guardar solo las filas seleccionadas** .
   4. Haga clic en **Save**(Guardar).
      
      ![Exportación de una lista como un archivo de valores separados por comas](./media/storsimple-snapshot-manager-mmc-menu/HCS_SSM_Export_List.png) 

## <a name="help"></a>Ayuda
Puede usar el menú **Ayuda** para ver la Ayuda en línea disponible para Administrador de instantáneas StorSimple y MMC.

#### <a name="to-view-available-online-help"></a>Para ver la Ayuda en línea disponible
1. Haga clic en el icono del escritorio para iniciar Administrador de instantáneas StorSimple.
2. En el panel **Ámbito**, haga clic con el botón secundario en un nodo o expanda el nodo y haga clic con el botón secundario en un elemento del panel **Resultados** y luego haga clic en **Ayuda**. 

## <a name="next-steps"></a>Pasos siguientes
* Obtenga más información sobre la [interfaz de usuario de Snapshot Manager de StorSimple](storsimple-use-snapshot-manager.md).
* Obtenga más información sobre el [uso de Snapshot Manager de StorSimple para administrar la solución de StorSimple](storsimple-snapshot-manager-admin.md).

