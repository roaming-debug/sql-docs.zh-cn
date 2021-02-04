---
description: sqlsrv_get_field
title: sqlsrv_get_field | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_get_field
apitype: NA
helpviewer_keywords:
- sqlsrv_get_field
- API Reference, sqlsrv_get_field
- retrieving data, as a single field
ms.assetid: fa17cc56-fb38-433b-a40d-65642f04dc23
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3fd330fdfc43ae11f4958418afc1504a0e44502f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154055"
---
# <a name="sqlsrv_get_field"></a>sqlsrv_get_field
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

从当前行的指定字段中检索数据。 必须按顺序访问字段数据。 例如，在访问第二个字段中的数据后，不能访问第一个字段中的数据。  
  
## <a name="syntax"></a>语法  
  
```  
sqlsrv_get_field( resource $stmt, int $fieldIndex [, int $getAsType])  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：对应于已执行语句的语句资源。  
  
*$fieldIndex*：要检索的字段索引。 索引从零开始。  
  
$getAsType [可选]：一个 SQLSRV 常量 (SQLSRV_PHPTYPE_*)，可确定返回数据的 PHP 数据类型。 若要了解受支持的数据类型，请参阅[常量 (Microsoft Drivers for PHP for SQL Server)](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)。 如果没有指定返回类型，将返回默认 PHP 类型。 有关默认 PHP 类型的信息，请参阅 [Default PHP Data Types](../../connect/php/default-php-data-types.md)。 有关指定默认 PHP 数据类型的信息，请参阅 [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md)。  
  
## <a name="return-value"></a>返回值  
字段数据。 使用 *$getAsType* 参数可以指定返回数据的 PHP 数据类型。 如果没有指定返回数据类型，将返回默认 PHP 数据类型。 有关默认 PHP 类型的信息，请参阅 [Default PHP Data Types](../../connect/php/default-php-data-types.md)。 有关指定默认 PHP 数据类型的信息，请参阅 [How to: Specify PHP Data Types](../../connect/php/how-to-specify-php-data-types.md)。  
  
## <a name="remarks"></a>注解  
sqlsrv_fetch 和 sqlsrv_get_field 的组合提供了对数据的只进访问权限。  
  
sqlsrv_fetch/sqlsrv_get_field 的组合仅将结果集行的一个字段加载到脚本内存中，并允许 PHP 返回类型规范 。 （有关如何指定 PHP 返回类型的信息，请参阅[如何：指定 PHP 数据类型](../../connect/php/how-to-specify-php-data-types.md)。）此函数组合还允许流式检索数据。 （若要了解流式检索数据的相关信息，请参阅[使用 SQLSRV 驱动程序流式检索数据](../../connect/php/retrieving-data-as-a-stream-using-the-sqlsrv-driver.md)。）  
  
## <a name="example"></a>示例  
以下示例检索的数据行包含产品审核和审阅者名称。 若要从结果集中检索数据，请使用 **sqlsrv_get_field**。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Set up and execute the query. Note that both ReviewerName and  
Comments are of the SQL Server nvarchar type. */  
$tsql = "SELECT ReviewerName, Comments   
         FROM Production.ProductReview  
         WHERE ProductReviewID=1";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in statement preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Make the first row of the result set available for reading. */  
if( sqlsrv_fetch( $stmt ) === false )  
{  
     echo "Error in retrieving row.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Note: Fields must be accessed in order.  
Get the first field of the row. Note that no return type is  
specified. Data will be returned as a string, the default for  
a field of type nvarchar.*/  
$name = sqlsrv_get_field( $stmt, 0);  
echo "$name: ";  
  
/*Get the second field of the row as a stream.  
Because the default return type for a nvarchar field is a  
string, the return type must be specified as a stream. */  
$stream = sqlsrv_get_field( $stmt, 1,   
                            SQLSRV_PHPTYPE_STREAM( SQLSRV_ENC_CHAR));  
while( !feof( $stream))  
{   
    $str = fread( $stream, 10000);  
    echo $str;  
}  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  

[检索数据](../../connect/php/retrieving-data.md)  

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  
  
