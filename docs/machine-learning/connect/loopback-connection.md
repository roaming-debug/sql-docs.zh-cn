---
title: Python 和 R 中的 SQL 环回连接
description: 了解如何使用环回连接通过 ODBC 连接回 SQL Server，以从基于 sp_execute_external_script 执行的 Python 或 R 脚本读取或写入数据。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/20/2020
ms.topic: how-to
author: Aniruddh25
ms.author: anmunde
ms.reviewer: dphansen
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 062c62eedaeed16215a538fca8303acb7e88343b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082498"
---
# <a name="loopback-connection-to-sql-server-from-a-python-or-r-script"></a>从 Python 或 R 脚本到 SQL Server 的环回连接
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

了解如何结合使用环回连接与[机器学习服务](../sql-server-machine-learning-services.md)通过 [ODBC](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md) 连接回 SQL Server，以从基于 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 执行的 Python 或 R 脚本读取或写入数据。 当无法使用 `sp_execute_external_script` 的 InputDataSet 和 OutputDataSet 参数时，可以使用此选项 。

## <a name="connection-string"></a>连接字符串

若要建立环回连接，需要使用正确的连接字符串。 常用的强制参数是 [ODBC 驱动程序](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)的名称、服务器地址和数据库名称。

### <a name="connection-string-on-windows"></a>Windows 上的连接字符串

若要在 Windows 的 SQL Server 上进行身份验证，Python 或 R 脚本可以使用 Trusted_Connection 连接字符串属性作为执行 sp_execute_external_script 的同一用户进行身份验证。

以下是 Windows 上的环回连接字符串示例：

``` 
"Driver=SQL Server;Server=.;Database=nameOfDatabase;Trusted_Connection=Yes;"
```

### <a name="connection-string-on-linux"></a>Linux 上的连接字符串

若要在 Linux 的 SQL Server 上进行身份验证，Python 或 R 脚本需要使用 ODBC 驱动程序的 ClientCertificate 和 ClientKey 属性来作为执行 `sp_execute_external_script` 的同一用户进行身份验证。 这需要使用[最新的 ODBC 驱动程序](../../connect/odbc/download-odbc-driver-for-sql-server.md)版本 17.4.1.1。

以下是 Linux 上的环回连接字符串示例：

```
"Driver=ODBC Driver 17 for SQL Server;Server=fe80::8012:3df5:0:5db1%eth0;Database=nameOfDatabase;ClientCertificate=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitecert.pem;ClientKey=file:/var/opt/mssql-extensibility/data/baeaac72-60b3-4fae-acfd-c50eff5d34a2/sqlsatellitekey.pem;TrustServerCertificate=Yes;Trusted_Connection=no;Encrypt=Yes"
```

服务器地址、客户端证书文件位置和客户端密钥文件位置对于每个 `sp_execute_external_script` 都是唯一的，可通过使用 API“rx_get_sql_loopback_connection_string()”（适用于 Python）或“rxGetSqlLoopbackConnectionString()”（适用于 R）来获取这些信息。

有关连接字符串属性的详细信息，请参阅 Microsoft ODBC Driver for SQL Server 的 [DSN 和连接字符串关键字和属性](../../connect/odbc/dsn-connection-string-attribute.md#new-connection-string-keywords-and-connection-attributes)。

## <a name="generate-connection-string-with-revoscalepy-for-python"></a>使用 revoscalepy 为 Python 生成连接字符串

可使用 [revoscalepy](../python/ref-py-revoscalepy.md) 中的 API“rx_get_sql_loopback_connection_string()”来为 Python 脚本中的环回连接生成正确的连接字符串。

它接受以下参数：

| 参数 | 描述 |
|-|-|
| name_of_database | 要建立连接的数据库的名称 |
| odbc_driver | odbc 驱动程序的名称 |

### <a name="examples"></a>示例

Windows 上的 SQL Server 的示例：

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="SQL Server", name_of_database="DBName")
print("Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Linux 上的 SQL Server 的示例：

```sql
EXECUTE sp_execute_external_script
@language = N'Python',
@script = N'
from revoscalepy import rx_get_sql_loopback_connection_string, RxSqlServerData, rx_data_step
loopback_connection_string = rx_get_sql_loopback_connection_string(odbc_driver="ODBC Driver 17 for SQL Server",
                                                                   name_of_database="DBName")
print("Loopback Connection String:{0}".format(loopback_connection_string))
data_set = RxSqlServerData(sql_query = "select col1, col2 from tableName",
                           connection_string = loopback_connection_string)
OutputDataSet = rx_data_step(data_set)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="generate-connection-string-with-revoscaler-for-r"></a>使用 RevoScaleR 为 R 生成连接字符串

可使用 [RevoScaleR](../r/ref-r-revoscaler.md) 中的 API“rxGetSqlLoopbackConnectionString()”来为 R 脚本中的环回连接生成正确的连接字符串。

它接受以下参数：

| 参数 | 描述 |
|-|-|
| nameOfDatabase | 要建立连接的数据库的名称 |
| odbcDriver | odbc 驱动程序的名称 |

### <a name="examples"></a>示例

Windows 上的 SQL Server 的示例：

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <- rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", odbcDriver ="SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName",
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

Linux 上的 SQL Server 的示例：

```sql
EXECUTE sp_execute_external_script
@language = N'R',
@script = N'
    loopbackConnectionString <-  rxGetSqlLoopbackConnectionString(nameOfDatabase="DBName", 
                                                                  odbcDriver ="ODBC Driver 17 for SQL Server")
    print(paste("Connection String:", loopbackConnectionString))
    dataSet <- RxSqlServerData(sqlQuery = "select col1, col2 from tableName", 
                               connectionString = loopbackConnectionString)
    OutputDataSet <- rxDataStep(dataSet)
'
WITH RESULT SETS ((col1 int, col2 int))
GO
```

## <a name="next-steps"></a>后续步骤

+ [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md)
+ [revoscalepy](../python/ref-py-revoscalepy.md)
+ [RevoScaleR](../r/ref-r-revoscaler.md)
