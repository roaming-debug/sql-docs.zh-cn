---
title: 如何：使用 SQLSRV 驱动程序检索输出参数
description: 了解如何通过用于 SQL Server 的 PHP 的 Microsoft SQLSRV 驱动程序使用和检索存储过程的输出参数。
ms.custom: ''
ms.date: 04/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- stored procedure support
ms.assetid: 1157bab7-6ad1-4bdb-a81c-662eea3e7fcd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6f0e165c04e303a5cb65046b013df8814bfbf515
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100038359"
---
# <a name="how-to-retrieve-output-parameters-using-the-sqlsrv-driver"></a>如何：使用 SQLSRV 驱动程序检索输出参数
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本主题演示如何调用一个参数已在其中定义为输出参数的存储过程。 在检索输出参数或输入/输出参数时，必须在可以访问返回的参数值前使用存储过程返回的所有结果。  
  
> [!NOTE]  
>  无法将已初始化或更新为 **null**、 **DateTime** 或流类型的变量用作输出参数。  
  
在流类型（例如 SQLSRV_SQLTYPE_VARCHAR('max')）用作输出参数时，将会发生数据截断。 不支持将流类型用作输出参数。 对于非流类型，数据截断会在未指定输出参数长度或输出参数的指定长度不够大时发生。  
  
## <a name="example-1"></a>示例 1
以下示例调用可返回某个指定员工的从年初至今的销售的存储过程。 PHP 变量 *$lastName* 是输入参数，而 *$salesYTD* 是输出参数。  
  
> [!NOTE]  
> 将 *$salesYTD* 初始化为 0.0 会将返回的 PHPTYPE 设置为 **浮点型**。 为确保数据类型完整性，应在调用存储过程之前初始化输出参数，或应指定所需的 PHPTYPE。 有关指定 PHPTYPE 的信息，请参阅 [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md)。  
  
由于存储过程仅返回一个结果，所以在执行存储过程后 *$salesYTD* 会立即包含输出参数的返回值。  
  
> [!NOTE]  
> 建议使用规范语法来调用存储过程。 有关规范语法的详细信息，请参阅[调用存储过程](../../relational-databases/native-client-odbc-stored-procedures/calling-a-stored-procedure.md)。  
  
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
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Drop the stored procedure if it already exists. */  
$tsql_dropSP = "IF OBJECT_ID('GetEmployeeSalesYTD', 'P') IS NOT NULL  
                DROP PROCEDURE GetEmployeeSalesYTD";  
$stmt1 = sqlsrv_query( $conn, $tsql_dropSP);  
if( $stmt1 === false )  
{  
     echo "Error in executing statement 1.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Create the stored procedure. */  
$tsql_createSP = " CREATE PROCEDURE GetEmployeeSalesYTD  
                   @SalesPerson nvarchar(50),  
                   @SalesYTD money OUTPUT  
                   AS  
                   SELECT @SalesYTD = SalesYTD  
                   FROM Sales.SalesPerson AS sp  
                   JOIN HumanResources.vEmployee AS e   
                   ON e.EmployeeID = sp.SalesPersonID  
                   WHERE LastName = @SalesPerson";  
$stmt2 = sqlsrv_query( $conn, $tsql_createSP);  
if( $stmt2 === false )  
{  
     echo "Error in executing statement 2.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*--------- The next few steps call the stored procedure. ---------*/  
  
/* Define the Transact-SQL query. Use question marks (?) in place of  
 the parameters to be passed to the stored procedure */  
$tsql_callSP = "{call GetEmployeeSalesYTD( ?, ? )}";  
  
/* Define the parameter array. By default, the first parameter is an  
INPUT parameter. The second parameter is specified as an OUTPUT  
parameter. Initializing $salesYTD to 0.0 sets the returned PHPTYPE to  
float. To ensure data type integrity, output parameters should be  
initialized before calling the stored procedure, or the desired  
PHPTYPE should be specified in the $params array.*/  
$lastName = "Blythe";  
$salesYTD = 0.0;  
$params = array(   
                 array($lastName, SQLSRV_PARAM_IN),  
                 array(&$salesYTD, SQLSRV_PARAM_OUT)  
               );  
  
/* Execute the query. */  
$stmt3 = sqlsrv_query( $conn, $tsql_callSP, $params);  
if( $stmt3 === false )  
{  
     echo "Error in executing statement 3.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Display the value of the output parameter $salesYTD. */  
echo "YTD sales for ".$lastName." are ". $salesYTD. ".";  
  
/*Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt1);  
sqlsrv_free_stmt( $stmt2);  
sqlsrv_free_stmt( $stmt3);  
sqlsrv_close( $conn);  
?>  
```  

> [!NOTE]
> 将输出参数绑定到 bigint 类型时，如果该值可能超出[整数](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)范围，则需要将其 SQL 字段类型指定为 SQLSRV_SQLTYPE_BIGINT。 否则，可能会导致“值超出范围”异常。

## <a name="example-2"></a>示例 2
此代码示例演示如何将大型 bigint 值作为输出参数进行绑定。  

```
<?php
$serverName = "(local)";
$connectionInfo = array("Database"=>"testDB");  
$conn = sqlsrv_connect($serverName, $connectionInfo);  
if ($conn === false) {  
    echo "Could not connect.\n";  
    die(print_r(sqlsrv_errors(), true));  
}  

// Assume the stored procedure spTestProcedure exists, which retrieves a bigint value of some large number
// e.g. 9223372036854
$bigintOut = 0;
$outSql = "{CALL spTestProcedure (?)}";
$stmt = sqlsrv_prepare($conn, $outSql, array(array(&$bigintOut, SQLSRV_PARAM_OUT, null, SQLSRV_SQLTYPE_BIGINT)));
sqlsrv_execute($stmt);
echo "$bigintOut\n";   // Expect 9223372036854

sqlsrv_free_stmt($stmt);  
sqlsrv_close($conn);  

?>
```

## <a name="see-also"></a>另请参阅  
[如何：使用 SQLSRV 驱动程序指定参数方向](../../connect/php/how-to-specify-parameter-direction-using-the-sqlsrv-driver.md)

[如何：使用 SQLSRV 驱动程序检索输入和输出参数](../../connect/php/how-to-retrieve-input-and-output-parameters-using-the-sqlsrv-driver.md)

[检索数据](../../connect/php/retrieving-data.md)  
  
