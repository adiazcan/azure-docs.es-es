---
title: Notas de la versión de Azure SQL Data Warehouse | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 11/11/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: a4737f4308f49547178bb216b90589c596f604c9
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54118445"
---
# <a name="azure-sql-data-warehouse-release-notes"></a>Notas de la versión de Azure SQL Data Warehouse

Azure SQL Data Warehouse es un almacenamiento de datos empresarial (EDW) en la nube empresarial que aprovecha el procesamiento paralelo masivo (MPP) para ejecutar rápidamente consultas complejas en petabytes de datos. SQL Data Warehouse se usa como un componente clave de una solución de macrodatos. Importe macrodatos en SQL Data Warehouse con consultas T-SQL de PolyBase simples y, después, use la potencia de MPP para realizar análisis de alto rendimiento. Al realizar la integración y el análisis, el almacenamiento de datos pasará a ser la versión única de certeza con la que puede contar su empresa para obtener información.

Haga clic en los siguientes vínculos para más información sobre las nuevas características y mejoras que puede esperar en la versión más reciente de Azure SQL Data Warehouse. Es probable que reciba estas actualizaciones de servicio durante su programación de mantenimiento identificada.

- [Diciembre de 2018](./release-notes-december-2018.md)
- [Octubre de 2018](./release-notes-october-2018.md)
- [Septiembre de 2018](./release-notes-september-2018.md)
- [Agosto de 2018](./release-notes-august-2018.md)
- [Julio de 2018](./release-notes-july-2018.md)
- [Junio de 2018](./release-notes-june-2018.md)
- [Mayo de 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>Comprobación de la versión de código que se ha aplicado al almacenamiento de datos

Para confirmar qué versión se ha aplicado a su almacén de datos, conéctese a su almacén de datos mediante SSMS y ejecute la siguiente sintaxis para devolver la versión actual de SQL Data Warehouse.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Salida de ejemplo: ![Versión de SQL Data Warehouse](./media/release-notes/sql_data_warehouse_version.png)

Use la fecha identificada para confirmar qué versión se ha aplicado a su instancia de Azure SQL Data Warehouse. 


## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) sobre cómo ver una programación de mantenimiento. 
- [Obtenga más información](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) sobre cómo cambiar una programación de mantenimiento.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health
