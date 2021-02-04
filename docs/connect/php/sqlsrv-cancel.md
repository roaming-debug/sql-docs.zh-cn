---
description: sqlsrv_cancel
title: sqlsrv_cancel | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_cancel
apitype: NA
helpviewer_keywords:
- sqlsrv_cancel
- API Reference, sqlsrv_cancel
ms.assetid: 75798c9b-f711-445d-9b8f-ba4d405ca50a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0d93f74d4b2df35e83ad4fbd72f0b0dcbd91e216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190272"
---
# <a name="sqlsrv_cancel"></a>sqlsrv_cancel
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

取消语句。 这意味着，将丢弃该语句的所有挂起结果。 在调用该函数后，如果已使用 [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) 准备好语句，可重新执行该语句。 如果已使用与该语句关联的所有结果，则无需调用该函数。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_cancel( resource $stmt)  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：要取消的语句。  
  
## <a name="return-value"></a>返回值  
布尔值：如果操作成功，则为 **true** 。 否则为 **false**。  
  
## <a name="example"></a>示例  
下面的示例面向 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库来执行查询，随后在变量 $salesTotal 达到指定数值后，使用结果并对结果进行计数。 之后将丢弃其余的查询结果。 该示例假定已在本地计算机上安装了 SQL Server 和 AdventureWorks 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```  
<?php  
/* Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Prepare and execute the query. */  
$tsql = "SELECT OrderQty, UnitPrice FROM Sales.SalesOrderDetail";  
$stmt = sqlsrv_prepare( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
if( sqlsrv_execute( $stmt ) === false)  
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Initialize tracking variables. */  
$salesTotal = 0;  
$count = 0;  
  
/* Count and display the number of sales that produce revenue  
of $100,000. */  
while( ($row = sqlsrv_fetch_array( $stmt)) && $salesTotal <=100000)  
{  
     $qty = $row[0];  
     $price = $row[1];  
     $salesTotal += ( $price * $qty);  
     $count++;  
}  
echo "$count sales accounted for the first $$salesTotal in revenue.\n";  
  
/* Cancel the pending results. The statement can be reused. */  
sqlsrv_cancel( $stmt);  
?>  
```  
  
## <a name="comments"></a>注释  
使用 [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md) 和 [sqlsrv_execute](../../connect/php/sqlsrv-execute.md) 组合准备和执行的语句可在调用 sqlsrv_cancel 后通过 sqlsrv_execute 重新执行。 使用 [sqlsrv_query](../../connect/php/sqlsrv-query.md) 执行的语句无法在调用 sqlsrv_cancel 后重新执行。  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)

[连接到服务器](../../connect/php/connecting-to-the-server.md)

[检索数据](../../connect/php/retrieving-data.md)

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)

[sqlsrv_free_stmt](../../connect/php/sqlsrv-free-stmt.md)

  
