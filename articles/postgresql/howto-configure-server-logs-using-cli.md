---
title: Configuración y acceso a los registros del servidor para PostgreSQL con la CLI de Azure
description: En este artículo se describe cómo configurar los registros de servidor de Azure Database for PostgreSQL, y obtener acceso a ellos, mediante la línea de comandos de la CLI de Azure.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 02/28/2018
ms.openlocfilehash: 938aadaa1d17135808cbeb34adac5139bd61f833
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545029"
---
# <a name="configure-and-access-server-logs-by-using-azure-cli"></a>Configuración y acceso a los registros del servidor con la CLI de Azure
Puede descargar los registros de error del servidor de PostgreSQL mediante la interfaz de la línea de comandos (CLI de Azure). Sin embargo, no se admite el acceso a los registros de transacciones. 

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- [Un servidor de Azure Database for PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Utilidad de línea de comandos [CLI de Azure](/cli/azure/install-azure-cli) o Azure Cloud Shell en el explorador

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Configuración del registro para Azure Database for PostgreSQL
Puede configurar el servidor para tener acceso a los registros de consulta y los registros de error. Los registros de error pueden contener información de vaciado automático, de conexión y de puntos de comprobación.
1. Active el registro.
2. Para habilitar el registro de consultas, actualice **log\_statement**  y  **log\_min\_duration\_statement**.
3. Actualice el período de retención.

Para obtener más información, consulte cómo [Personalización de los parámetros de configuración del servidor con la CLI de Azure](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Enumeración de registros del servidor de Azure Database for PostgreSQL
Para mostrar la lista de archivos de registro disponibles para el servidor, ejecute el comando [az postgres server-logs list](/cli/azure/postgres/server-logs#az_postgres_server_logs_list).

Puede enumerar archivos de registro del servidor **mydemoserver.postgres.database.azure.com** en el grupo de recursos **myresourcegroup**. A continuación, dirija la lista de archivos de registro a un archivo de texto denominado **log\_files\_list.txt**.
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mydemoserver > log_files_list.txt
```
## <a name="download-logs-locally-from-the-server"></a>Descarga de registros del servidor en el sistema local
Con el comando [az postgres server-logs download](/cli/azure/postgres/server-logs#az_postgres_server_logs_download), puede descargar archivos de registro individuales para su servidor. 

Use el ejemplo siguiente para descargar el archivo de registro específico para el servidor **mydemoserver.postgres.database.azure.com** en el grupo de recursos **myresourcegroup** a su entorno local.
```azurecli-interactive
az postgres server-logs download --name 20170414-mydemoserver-postgresql.log --resource-group myresourcegroup --server mydemoserver
```
## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información acerca de los registros del servidor, consulte [Registros del servidor en Azure Database for PostgreSQL](concepts-server-logs.md).
- Para obtener más información sobre los parámetros del servidor, consulte cómo [Personalización de los parámetros de configuración del servidor con la CLI de Azure](howto-configure-server-parameters-using-cli.md).
