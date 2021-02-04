---
description: sqlsrv_rollback
title: sqlsrv_rollback | Microsoft Docs
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_rollback
apitype: NA
helpviewer_keywords:
- transaction support
- API Reference, sqlsrv_rollback
- sqlsrv_rollback
ms.assetid: 6e6bac39-45af-428c-bc32-f773482562ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 19086d7efd3624396ea7561112245c5fe1ecfbd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201065"
---
# <a name="sqlsrv_rollback"></a>sqlsrv_rollback
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

回滚指定连接上的当前事务，并将连接返回至自动提交模式。 当前事务包括指定连接上的所有语句，这些语句在调用 [sqlsrv_begin_transaction](../../connect/php/sqlsrv-begin-transaction.md) 之后和调用 **sqlsrv_rollback** 或 [sqlsrv_commit](../../connect/php/sqlsrv-commit.md)之前执行。  
  
> [!NOTE]  
> 默认情况下，[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 处于自动提交模式。 这意味着，除非使用 **sqlsrv_begin_transaction** 开始事务。  
  
> [!NOTE]  
> 如果在不属于活动事务且已使用 **sqlsrv_rollback** transaction **sqlsrv_begin_transaction**，该调用将返回 **false** ，并向错误集合中添加一个 *未在事务中* 错误。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_rollback( resource $conn)  
```  
  
#### <a name="parameters"></a>参数  
*$conn*：其事务处于活动状态的连接。  
  
## <a name="return-value"></a>返回值  
布尔值：如果成功回滚事务，则为 **true** 。 否则为 **false**。  
  
## <a name="example"></a>示例  
作为事务的一部分，以下示例将执行两次查询。 如果两次查询均成功完成，将提交事务。 如果任一查询失败或这两次查询都失败，将回滚事务。  
  
该示例中的第一次查询向 AdventureWorks 数据库的 *Sales.SalesOrderDetail* 表格中插入了一个新销售订单。 该订单订购的是 5 套产品 ID 为 709 的产品。 第二次查询将产品 ID 为 709 的产品库存量减少 5 套。 这些查询包含在一个事务中，因为这两次查询均必须成功完成，数据库才能准确反映订单状态和产品供应情况。  
  
该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
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
     die( print_r( sqlsrv_errors(), true ));  
}  
  
/* Initiate transaction. */  
/* Exit script if transaction cannot be initiated. */  
if ( sqlsrv_begin_transaction( $conn) === false )  
{  
     echo "Could not begin transaction.\n";  
     die( print_r( sqlsrv_errors(), true ));  
}  
  
/* Initialize parameter values. */  
$orderId = 43659; $qty = 5; $productId = 709;  
$offerId = 1; $price = 5.70;  
  
/* Set up and execute the first query. */  
$tsql1 = "INSERT INTO Sales.SalesOrderDetail   
                     (SalesOrderID,   
                      OrderQty,   
                      ProductID,   
                      SpecialOfferID,   
                      UnitPrice)  
          VALUES (?, ?, ?, ?, ?)";  
$params1 = array( $orderId, $qty, $productId, $offerId, $price);  
$stmt1 = sqlsrv_query( $conn, $tsql1, $params1 );  
  
/* Set up and executee the second query. */  
$tsql2 = "UPDATE Production.ProductInventory   
          SET Quantity = (Quantity - ?)   
          WHERE ProductID = ?";  
$params2 = array($qty, $productId);  
$stmt2 = sqlsrv_query( $conn, $tsql2, $params2 );  
  
/* If both queries were successful, commit the transaction. */  
/* Otherwise, rollback the transaction. */  
if( $stmt1 && $stmt2 )  
{  
     sqlsrv_commit( $conn );  
     echo "Transaction was committed.\n";  
}  
else  
{  
     sqlsrv_rollback( $conn );  
     echo "Transaction was rolled back.\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_close( $conn);  
?>  
```  
  
为了重点介绍事务行为，上面的示例中未包含一些建议的错误处理。 对于生产应用程序，建议应检查对 **sqlsrv** 函数的任何调用是否存在错误并进行相应处理。  
  
> [!NOTE]  
> 不要使用嵌入式 Transact-SQL 来执行事务。 例如，不要以 Transact-SQL 查询的方式执行包含“BEGIN TRANSACTION”的语句来开始某一事务。 使用嵌入式 Transact-SQL 执行事务时，无法保证出现预期的事务行为。  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)

[如何：执行事务](../../connect/php/how-to-perform-transactions.md)

[Microsoft Drivers for PHP for SQL Server 概述](../../connect/php/overview-of-the-php-sql-driver.md) 
  
