---
title: PDOStatement::errorInfo
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDOStatement::errorInfo 函数的 API 参考。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: e45bebe8-ea4c-49b6-93db-cf1ae65f530c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0474b0c1225a248b47f0892ac940f2e58ab0efa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206360"
---
# <a name="pdostatementerrorinfo"></a>PDOStatement::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

检索语句句柄上最新操作的扩展错误信息。  
  
## <a name="syntax"></a>语法  

```
array PDOStatement::errorInfo();
```
  
## <a name="return-value"></a>返回值  
语句句柄上有关最新操作的错误信息的数组。 该数组包含以下字段：  
  
-   SQLSTATE 错误代码  
  
-   特定于驱动程序的错误代码  
  
-   特定于驱动程序的错误消息  
  
如果没有错误或如果未设置 SQLSTATE，特定于驱动程序的字段将为 NULL。  
  
## <a name="remarks"></a>注解  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
在此示例中，SQL 语句具有一个错误，随后将报告该错误。  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks", "", "");  
$stmt = $conn->prepare('SELECT * FROM Person.Addressx');  
  
$stmt->execute();  
print_r ($stmt->errorInfo());  
?>  
```

## <a name="additional-odbc-messages"></a>其他 ODBC 消息

发生异常时，ODBC 驱动程序可能会返回多个错误来帮助诊断问题。 但是，PDOStatement::errorInfo 始终只显示第一个错误。 为了响应此 [Bug 报告](https://bugs.php.net/bug.php?id=78196)，已更新 [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) 和 [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php)，以指示驱动程序应至少显示以下三个字段：
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

从 5.9.0 开始，PDOStatement::errorInfo 的默认行为是显示其他 ODBC 错误（如果可用）。 请参阅 [PDO::errorInfo](../../connect/php/pdo-errorinfo.md) 了解更多详细信息。
  
## <a name="see-also"></a>另请参阅  
[PDOStatement 类](../../connect/php/pdostatement-class.md)

[PDO::errorInfo](../../connect/php/pdo-errorinfo.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
