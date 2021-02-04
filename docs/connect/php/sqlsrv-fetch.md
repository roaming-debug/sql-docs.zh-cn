---
description: sqlsrv_fetch
title: sqlsrv_fetch | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch
- API Reference, sqlsrv_fetch
- retrieving data, as a single field
ms.assetid: a5a640a1-6e7d-452e-8b66-850a4dc2ce89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1ab164eb61bcde26862d47c544e366a296117e5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154384"
---
# <a name="sqlsrv_fetch"></a>sqlsrv_fetch
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

使结果集的下一行可供读取。 [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md) 可用于读取行的字段。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_fetch( resource $stmt[, row[, ]offset])  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：对应于已执行语句的语句资源。  
  
> [!NOTE]  
> 必须执行语句，才可以检索结果。 有关执行语句的信息，请参阅 [sqlsrv_query](../../connect/php/sqlsrv-query.md) 和 [sqlsrv_execute](../../connect/php/sqlsrv-execute.md)。  
  
row [可选]：下面的值之一，用于指定要在使用可滚动游标的结果集中访问的行：  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
有关这些值的详细信息，请参阅 [指定游标类型和选择行](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)。  
  
*offset* [可选]：结合使用 SQLSRV_SCROLL_ABSOLUTE 和 SQLSRV_SCROLL_RELATIVE 以指定要检索的行。 结果集中的第一个记录为 0。  
  
## <a name="return-value"></a>返回值  
如果已成功检索结果集的下一行，将返回 **True** 。 如果结果集中没有更多结果，将返回 **null** 。 如果出现错误，将返回 **False** 。  
  
## <a name="example"></a>示例  
以下示例使用 **sqlsrv_fetch** 检索包含产品审核和审阅者名称的数据行。 若要从结果集中检索数据，请使用 [sqlsrv_get_field](../../connect/php/sqlsrv-get-field.md)。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
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
Comments are of SQL Server type nvarchar. */  
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
if( sqlsrv_fetch( $stmt ) === false)  
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
while( !feof( $stream ))  
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
[检索数据](../../connect/php/retrieving-data.md)  

[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  
  
