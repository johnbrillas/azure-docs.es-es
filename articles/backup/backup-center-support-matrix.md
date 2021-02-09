---
title: Matriz de compatibilidad del Centro de copia de seguridad
description: En este artículo se resumen los escenarios que admite el Centro de copia de seguridad para cada tipo de carga de trabajo
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: 672f0e127f173260f25978497198fd7b554aa390
ms.sourcegitcommit: 100390fefd8f1c48173c51b71650c8ca1b26f711
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98893599"
---
# <a name="support-matrix-for-backup-center"></a>Matriz de compatibilidad del Centro de copia de seguridad

El Centro de copia de seguridad proporciona un panel único para que las empresas [controlen, supervisen, operen y analicen las copias de seguridad a gran escala](backup-center-overview.md). En este artículo se resumen los escenarios que admite el Centro de copia de seguridad para cada tipo de carga de trabajo.

## <a name="supported-scenarios"></a>Escenarios admitidos

| **Categoría** | **Escenario**  | **Cargas de trabajo compatibles**  | **Límites** |
| -------------| ------------- | ----------------------- |------------|
| Supervisión   | Ver todos los trabajos | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | <li> Siete días válidos de trabajos disponibles listos para usar. <br> <li> Cada filtro o menú desplegable admite un máximo de 1000 elementos. Así pues, el Centro de copia de seguridad se puede usar para supervisar un máximo de 1000 suscripciones y 1000 almacenes en los inquilinos. |
| Supervisión | Ver todas las instancias de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Lo mismo que antes. |
| Supervisión | Ver todas las directivas de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Lo mismo que antes. |
| Supervisión | Ver todos los almacenes | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Lo mismo que antes. |
| Acciones | Configuración de la copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para la [copia de seguridad de la máquina virtual de Azure](./backup-support-matrix-iaas.md) y la [copia de seguridad del servidor Azure Database for PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Acciones | Restaurar la instancia de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para la [copia de seguridad de la máquina virtual de Azure](./backup-support-matrix-iaas.md) y la [copia de seguridad del servidor Azure Database for PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Acciones | Crear almacén | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para el [almacén de Recovery Services](./backup-support-matrix.md#vault-support) |
| Acciones | Creación de una directiva de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para el [almacén de Recovery Services](./backup-support-matrix.md#vault-support) |
| Acciones | Ejecución de una copia de seguridad a petición para una instancia de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para la [copia de seguridad de la máquina virtual de Azure](./backup-support-matrix-iaas.md) y la [copia de seguridad del servidor Azure Database for PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Acciones | Detención de la copia de seguridad de una instancia de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL <br><br> <li> SQL en Azure VM <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files | Consulte las matrices de compatibilidad para la [copia de seguridad de la máquina virtual de Azure](./backup-support-matrix-iaas.md) y la [copia de seguridad del servidor Azure Database for PostgreSQL](backup-azure-database-postgresql.md#support-matrix) |
| Información detallada | Ver informes de copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> SQL en la máquina virtual de Azure <br><br> <li> SAP HANA en la máquina virtual de Azure <br><br> <li> Azure Files <br><br> <li> System Center Data Protection Manager <br><br> <li> Agente de Azure Backup (MARS) <br><br> <li> Servidor de Azure Backup (MABS) | Consulte los [escenarios admitidos para Informes de Backup](./configure-reports.md#supported-scenarios) |
| Gobernanza | Ver y asignar directivas de Azure integradas y personalizadas en la categoría "Copia de seguridad" | N/D | N/D |
| Gobernanza | Ver orígenes de datos no configurados para la copia de seguridad | <li> Máquina virtual de Azure <br><br> <li> Servidor de Azure Database for PostgreSQL | N/D |

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

| **Categoría** | **Escenario**  |
|--------------|---------------|
| Supervisión | Ver alertas a gran escala |
| Acciones | Configurar almacenes a gran escala |
| Acciones | Ejecutar trabajo de restauración entre regiones desde el Centro de copia de seguridad |

## <a name="next-steps"></a>Pasos siguientes

* [Revise la matriz de compatibilidad para Azure Backup](./backup-support-matrix.md)
* [Revise la matriz de compatibilidad para la copia de seguridad de máquinas virtuales de Azure](./backup-support-matrix-iaas.md)
* [Revise la matriz de compatibilidad para la copia de seguridad del servidor Azure Database for PostgreSQL](backup-azure-database-postgresql.md#support-matrix)