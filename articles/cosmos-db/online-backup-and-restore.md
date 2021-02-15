---
title: Copia de seguridad en línea y restauración de datos a petición en Azure Cosmos DB
description: En este artículo se describe cómo funcionan la copia de seguridad automática y la restauración de datos a petición. También se explica la diferencia entre los modos de copia de seguridad continua y periódica.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 10/13/2020
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 2629e9c6e048620d9490a1e091a16c138fd1e615
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2021
ms.locfileid: "99525439"
---
# <a name="online-backup-and-on-demand-data-restore-in-azure-cosmos-db"></a>Copias de seguridad en línea y restauración de datos a petición en Azure Cosmos DB
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB crea automáticamente copias de seguridad de los datos a intervalos regulares. Las copias de seguridad automáticas se crean sin afectar al rendimiento o a la disponibilidad de las operaciones de las bases de datos. Todas las copias de seguridad se almacenan por separado en un servicio de almacenamiento. Las copias de seguridad automáticas son útiles en escenarios en los se elimina accidentalmente o se actualiza su cuenta, base de datos o contenedor de Azure Cosmos y más tarde se requiere la recuperación de datos. Hay dos modos de copia de seguridad:

* **Modo de copia de seguridad periódica**: este modo es el modo de copia de seguridad predeterminado para todas las cuentas existentes. En este modo, la copia de seguridad se realiza a intervalos periódicos y los datos se restauran mediante la creación de una solicitud con el equipo de soporte técnico. En este modo, debe configurar un intervalo de copia de seguridad y la retención de la cuenta. El período de retención máximo se amplía a un mes. El intervalo mínimo de la copia de seguridad puede ser de una hora.  Para obtener más información, consulte el artículo [Modo de copia de seguridad periódica](configure-periodic-backup-restore.md).

* **Modo de copia de seguridad continua** (actualmente en versión preliminar pública): elija este modo al crear la cuenta de Azure Cosmos DB. Este modo permite realizar la restauración en cualquier punto temporal de los últimos 30 días. Para obtener más información, consulte la [Introducción al modo de copia de seguridad continua](continuous-backup-restore-introduction.md), configure la copia de seguridad continua con los artículos de [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md), la [CLI](continuous-backup-restore-command-line.md) y [Resource Manager](continuous-backup-restore-template.md).

  > [!NOTE]
  > Si configura una cuenta nueva con el modo de copia de seguridad continua, puede realizar la restauración de autoservicio a través de Azure Portal, PowerShell o la CLI. Si la cuenta está configurada en modo continuo, no se puede volver a cambiar al modo periódico. Actualmente, las cuentas existentes con el modo de copia de seguridad periódica no se pueden cambiar al modo continuo.  

## <a name="next-steps"></a>Pasos siguientes

A continuación, puede obtener información sobre cómo configurar y administrar los modos de copia de seguridad periódica y continua para su cuenta:

* Directiva de [configuración y administración de la copia de seguridad periódica](configure-periodic-backup-restore.md).
* ¿Qué es el modo de [copia de seguridad continua](continuous-backup-restore-introduction.md)?
* Configure y administre la copia de seguridad continua mediante [Azure Portal](continuous-backup-restore-portal.md), [PowerShell](continuous-backup-restore-powershell.md), la [CLI](continuous-backup-restore-command-line.md) o [Azure Resource Manager](continuous-backup-restore-template.md).
* [Administre los permisos](continuous-backup-restore-permissions.md) necesarios para restaurar datos con el modo de copia de seguridad continua.
