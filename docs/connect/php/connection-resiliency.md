---
title: 空闲连接复原
description: 了解空闲连接复原的概念及其在 Microsoft Drivers for PHP for SQL Server 中的行为方式。
ms.date: 03/04/2021
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 07223e87c82a64fe17cd1b2969b5c93227c70af2
ms.sourcegitcommit: 0bcda4ce24de716f158a3b652c9c84c8f801677a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102247489"
---
# <a name="idle-connection-resiliency"></a>空闲连接复原
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

[连接复原](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)是指在一定约束条件下可以重新建立断开的空闲连接的原则。 如果与 Microsoft SQL Server 连接断开，连接复原可便于客户端自动尝试重新建立连接。 连接复原是数据源属性；只有 SQL Server 2014 及更高版本和 Azure SQL 数据库才支持连接复原。

连接复原是通过下面两个可以添加到连接字符串的连接关键字来实现的：ConnectRetryCount  和 ConnectRetryInterval  。

|关键字|值|默认|说明|
|-|-|-|-|
|**ConnectRetryCount**| 介于 0 和 255 之间（含限值）的整数|1|在连接断开时最多尝试重新建立多少次连接后放弃。 在连接断开时，默认尝试一次重新建立连接。 值 0 表示不会尝试重新建立连接。|
|**ConnectRetryInterval**| 介于 1 和 60 之间（含限值）的整数|10| 尝试重新建立连接的时间间隔（以秒为单位）。 应用程序在检测到连接断开后立即尝试重新连接，然后在等待 ConnectRetryInterval  秒后重试。 如果 ConnectRetryCount  等于 0，则忽略此关键字。

如果 ConnectRetryCount  与 ConnectRetryInterval  的乘积大于 LoginTimeout  ，则在达到 LoginTimeout  后，客户端会立即停止尝试连接；如果不大于，它会继续尝试重新连接，直到达到 ConnectRetryCount  。

#### <a name="remarks"></a>备注

连接复原在连接处于空闲状态时应用。 例如，在执行事务期间发生的故障不会触发重新连接尝试，而是按预期失败。 以下情况称为不可恢复的会话状态，不会触发重新连接尝试：

* 临时表
* 全局和局部游标
* 事务上下文和会话级别事务锁
* 应用程序锁
* EXECUTE AS/REVERT 安全性上下文
* OLE 自动化句柄
* 准备的 XML 句柄
* 跟踪标志

## <a name="example"></a>示例

下面的代码连接到数据库，并执行查询。 它通过终止会话来断开连接，并尝试使用断开的连接进行新查询。 本示例使用 [AdventureWorks](/previous-versions/sql/sql-server-2008/ms124501(v=sql.100)) 示例数据库。

此示例在断开连接之前指定缓冲的游标。 如果未指定缓冲的游标，就不会重新建立连接，因为会有活动的服务器端游标，导致连接在断开时不处于空闲状态。 不过，在这种情况下，可以先调用 sqlsrv_free_stmt()，再断开连接以腾出游标，这样就可以成功地重新建立连接。

```php
<?php
// This function breaks the connection by determining its session ID and killing it.
// A separate connection is used to break the main connection because a session
// cannot kill itself. The sleep() function ensures enough time has passed for KILL
// to finish ending the session.
function BreakConnection( $conn, $conn_break )
{
    $stmt1 = sqlsrv_query( $conn, "SELECT @@SPID" );
    if ( sqlsrv_fetch( $stmt1 ) )
    {
        $spid=sqlsrv_get_field( $stmt1, 0 );
    }

    $stmt2 = sqlsrv_prepare( $conn_break, "KILL ".$spid );
    sqlsrv_execute( $stmt2 );
    sleep(1);
}

// Connect to the local server using Windows authentication and specify
// AdventureWorks as the database in use. Specify values for
// ConnectRetryCount and ConnectRetryInterval as well.
$databaseName = 'AdventureWorks2014';
$serverName = '(local)';
$connectionInfo = array( "Database"=>$databaseName, "ConnectRetryCount"=>10, "ConnectRetryInterval"=>10 );

$conn = sqlsrv_connect( $serverName, $connectionInfo );
if( $conn === false)  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}

// A separate connection that will be used to break the main connection $conn
$conn_break = sqlsrv_connect( $serverName, array( "Database"=>$databaseName) );

// Create a statement to retrieve the contents of a table
$stmt1 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Employee",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt1 === false )
{
     echo "Error in statement 1.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 1 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt1 );
    echo $rowcount." rows in result set.\n";
}

// Now break the connection $conn
BreakConnection( $conn, $conn_break );

// Create another statement. The connection will be reestablished.
$stmt2 = sqlsrv_query( $conn, "SELECT * FROM HumanResources.Department",
                       array(), array( "Scrollable"=>"buffered" ) );
if( $stmt2 === false )
{
     echo "Error in statement 2.\n";
     die( print_r( sqlsrv_errors(), true ));
}
else
{
    echo "Statement 2 successful.\n";
    $rowcount = sqlsrv_num_rows( $stmt2 );
    echo $rowcount." rows in result set.\n";
}

sqlsrv_close( $conn );
sqlsrv_close( $conn_break );
?>
```
预期输出：
```
Statement 1 successful.
290 rows in result set.
Statement 2 successful.
16 rows in result set.
```

## <a name="see-also"></a>另请参阅
[Windows ODBC 驱动程序中的连接弹性](../odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
