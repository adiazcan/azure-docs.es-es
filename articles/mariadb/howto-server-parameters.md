---
title: Configuración de parámetros del servidor en Azure Database for MariaDB
description: En este artículo se explica cómo configurar los parámetros del servidor MariaDB en Azure Database for MariaDB mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0576941c803dfed6359fe34b67370eaf3f99c7e7
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53538266"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Configuración de parámetros del servidor en Azure Database for MariaDB mediante Azure Portal

Azure Database for MariaDB admite la configuración de algunos parámetros del servidor. En este artículo se explica cómo configurar estos parámetros mediante Azure Portal. No es posible ajustar todos los parámetros del servidor.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Ir a Parámetros del servidor de Azure Portal

1. Inicie sesión en Azure Portal y luego localice su servidor de Azure Database for MariaDB.
2. En la sección **CONFIGURACIÓN**, haga clic en **Parámetros del servidor** para abrir la página Parámetros del servidor de Azure Database for MariaDB.
![Página Parámetros del servidor de Azure Portal](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Localice cualquier configuración que deba ajustar. Revise la columna **Descripción** para entender el propósito y los valores permitidos.
![Menú desplegable de enumeración](./media/howto-server-parameters/3-toggle_parameter.png)
4. Haga clic en **Guardar** para guardar los cambios.
![Guardar o descartar cambios](./media/howto-server-parameters/4-save_parameters.png)
5. Si ha guardado los nuevos valores para los parámetros, siempre puede revertir todos los elementos a los valores predeterminados; para ello, seleccione **Restablecer todos los valores predeterminados**.
![Restablecer todos los valores predeterminados](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Lista de parámetros configurables del servidor

La lista de parámetros del servidor admitidos crece constantemente. Use la pestaña Parámetros del servidor en Azure Portal para obtener la definición y configurar parámetros del servidor basados en los requisitos de la aplicación.

## <a name="non-configurable-server-parameters"></a>Parámetros no configurables del servidor

El grupo de búferes de InnoDB y el número máximo de conexiones no son configurables y están vinculados a su [plan de tarifa](concepts-pricing-tiers.md).

|**Plan de tarifa**| **Núcleos virtuales**|**Grupo de búferes InnoDB (MB)**| **Conexiones máximas**|
|---|---|---|---|--|
|Básica| 1| 1024| 50|
|Básica| 2| 2560| 100|
|Uso general| 2| 3584| 300|
|Uso general| 4| 7680| 625|
|Uso general| 8| 15360| 1250|
|Uso general| 16| 31232| 2.500|
|Uso general| 32| 62976| 5000|
|Memoria optimizada| 2| 7168| 600|
|Memoria optimizada| 4| 15360| 1250|
|Memoria optimizada| 8| 30720| 2.500|
|Memoria optimizada| 16| 62464| 5000|

Estos parámetros adicionales del servidor no son configurables en el sistema:

|**Parámetro**|**Valor fijo**|
| :------------------------ | :-------- |
|innodb_file_per_table en el nivel básico|Apagado|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Los demás parámetros del servidor que no aparecen aquí se establecen en sus valores predeterminados de MariaDB para [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/).

## <a name="working-with-the-time-zone-parameter"></a>Trabajo con el parámetro de zona horaria

### <a name="populating-the-time-zone-tables"></a>Relleno de las tablas de la zona horaria

Las tablas de la zona horaria del servidor se pueden rellenar mediante una llamada al procedimiento almacenado `az_load_timezone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench.

> [!NOTE]
> Si ejecuta el comando `az_load_timezone` de MySQL Workbench, es posible que primero tenga que desactivar el modo de actualización segura mediante `SET SQL_SAFE_UPDATES=0;`.

```sql
CALL mysql.az_load_timezone();
```

Para ver los valores de zonas horarias disponibles, ejecute el comando siguiente:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Establecimiento de la zona horaria de nivel global

La zona horaria de nivel global se puede establecer desde la página **Parámetros del servidor** en Azure Portal. En el ejemplo siguiente se establece la zona horaria global en el valor "US/Pacific".

![Establecimiento del parámetro de zona horaria](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Establecimiento de la zona horaria de nivel de sesión

La zona horaria de nivel de sesión se puede establecer mediante la ejecución del comando `SET time_zone` desde una herramienta como la línea de comandos de MySQL o MySQL Workbench. En el ejemplo siguiente se establece la zona horaria en **US/Pacific**.

```sql
SET time_zone = 'US/Pacific';
```

Consulte en la documentación de MariaDB las [Funciones de fecha y hora](https://mariadb.com/kb/en/library/convert_tz/).

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->
