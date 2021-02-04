---
description: sqlsrv_begin_transaction
title: sqlsrv_begin_transaction | Microsoft Docs
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_begin_transaction
apitype: NA
helpviewer_keywords:
- sqlsrv_begin_transaction
- transaction support
- API Reference, sqlsrv_begin_transaction
ms.assetid: 0b223bc8-4047-4329-9cbf-d350ab0fb886
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8b64d0d37ca716d11a9b1a945480fe8a19d238dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190273"
---
# <a name="sqlsrv_begin_transaction"></a>sqlsrv_begin_transaction
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

在指定的连接上开始事务。 当前事务包括指定连接上的所有语句，这些语句在调用 **sqlsrv_begin_transaction** 之后和调用 [sqlsrv_rollback](../../connect/php/sqlsrv-rollback.md) 或 [sqlsrv_commit](../../connect/php/sqlsrv-commit.md)之前执行。  
  
> [!NOTE]  
> 默认情况下，[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 处于自动提交模式。 这意味着，除非使用 **sqlsrv_begin_transaction** 开始事务。  
  
> [!NOTE]  
> 如果在连接上启动事务后调用 **sqlsrv_begin_transaction** ，但未通过调用 **sqls_rvcommit** 或 **sqlsrv_rollback** 完成，该调用将返回 **false** ，并且错误集合中将添加一个 *Already in Transaction* 错误。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_begin_transaction( resource $conn)  
```  
  
#### <a name="parameters"></a>参数  
*$conn*：与事务相关联的连接。  
  
## <a name="return-value"></a>返回值  
布尔值：如果成功开始事务，则为 **true** 。 否则为 **false**。  
  
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
if ( sqlsrv_begin_transaction( $conn ) === false )  
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
  
/* Set up and execute the second query. */  
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
  
