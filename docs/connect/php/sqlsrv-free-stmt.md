---
description: sqlsrv_free_stmt
title: sqlsrv_free_stmt | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_free_stmt
apitype: NA
helpviewer_keywords:
- sqlsrv_free_stmt
- API Reference, sqlsrv_free_stmt
ms.assetid: 3c71f432-36ad-41e1-8ac7-587c82539448
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 011793bd0a76d3aa2984caa34b73800fd2e60040
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154176"
---
# <a name="sqlsrv_free_stmt"></a>sqlsrv_free_stmt
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

释放与指定语句相关联的所有资源。 在调用此函数后，无法再次使用该语句。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_free_stmt( resource $stmt)  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：要关闭的语句。  
  
## <a name="return-value"></a>返回值  
除非使用无效参数调用该函数，否则布尔值为 **true** 。 如果使用无效参数调用该函数，将返回 **False** 。  
  
> [!NOTE]  
> 对于此函数，**Null** 是有效参数。 这允许在脚本中调用该函数多次。 例如，当在错误条件中释放某个语句，并且在脚本结尾再次释放时，第二次对 sqlsrv_free_stmt 的调用将返回 true，因为第一次对 sqlsrv_free_stmt 的调用（在错误条件中）将语句资源设置为 null。  
  
## <a name="example"></a>示例  
以下示例创建某个语句资源、执行一次简单的查询，并调用 **sqlsrv_free_stmt** 来释放与该语句相关联的所有资源。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
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
  
$stmt = sqlsrv_query( $conn, "SELECT * FROM Person.Contact");  
if( $stmt )  
{  
     echo "Statement executed.\n";  
}  
else  
{  
     echo "Query could not be executed.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/*-------------------------------  
     Process query results here.  
-------------------------------*/  
  
/* Free the statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  

[sqlsrv_cancel](../../connect/php/sqlsrv-cancel.md)  
  
