---
title: PDO::errorInfo
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::errorInfo 函数的 API 参考。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 46b57275d92f4eb6acb64c276e3b34ea67efb429
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202030"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

检索数据库句柄上最近操作的扩展错误信息。  
  
## <a name="syntax"></a>语法  
  
```  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>返回值  
有关数据库句柄上最近操作的错误信息的阵列。 该数组包含以下字段：  
  
-   SQLSTATE 错误代码。  
  
-   特定于驱动程序的错误代码。  
  
-   特定于驱动程序的错误消息。  
  
如果没有错误或如果未设置 SQLSTATE，则特定于驱动程序的字段为 NULL。  
  
## <a name="remarks"></a>注解  
PDO::errorInfo 仅对直接在数据库上执行的操作检索错误信息。 在使用 PDO::prepare 或 PDO::query 创建 PDOStatement 实例时，使用 PDOStatement::errorInfo。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
在此示例中，列名拼写不正确（是 `Cityx` 而不是 `City`）导致了错误，随后报告了该错误。  
  
```php
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  

## <a name="additional-odbc-messages"></a>其他 ODBC 消息

发生异常时，ODBC 驱动程序可能会返回多个错误来帮助诊断问题。 但是，PDO::errorInfo 始终只显示第一个错误。 为了响应此 [Bug 报告](https://bugs.php.net/bug.php?id=78196)，已更新 [PDO::errorInfo](https://www.php.net/manual/en/pdo.errorinfo.php) 和 [PDOStatement::errorInfo](https://www.php.net/manual/en/pdostatement.errorinfo.php)，以指示驱动程序应至少显示以下三个字段：
```
0   SQLSTATE error code (a five characters alphanumeric identifier defined in the ANSI SQL standard).
1   Driver specific error code.
2   Driver specific error message.
```

从 5.9.0 开始，PDO::errorInfo 的默认行为是显示其他 ODBC 错误（如果可用）。 例如：

```php
<?php  
try {
    $conn = new PDO("sqlsrv:server=$server;", $uid, $pwd);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
    $stmt = $conn->prepare("SET NOCOUNT ON; USE $database; SELECT 1/0 AS col1");
    $stmt->execute();
} catch (PDOException $e) {
    var_dump($e->errorInfo);
}
?>  
```  

运行上述脚本应会引发异常，输出如下所示：

```
array(6) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
  [3]=>
  string(5) "22012"
  [4]=>
  int(8134)
  [5]=>
  string(87) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Divide by zero error encountered."
}
```

如果用户喜欢先前的方法，则可以使用新的配置选项 `pdo_sqlsrv.report_additional_errors` 将其关闭。 只需在任何 php 脚本的开头添加以下行：

```
ini_set('pdo_sqlsrv.report_additional_errors', 0);
```

在这种情况下，当运行相同的示例脚本时，显示的错误信息将如下所示：

```
array(3) {
  [0]=>
  string(5) "01000"
  [1]=>
  int(5701)
  [2]=>
  string(91) "[Microsoft][ODBC Driver 17 for SQL Server][SQL Server]Changed database context to 'tempdb'."
}
```

必要时，用户可以选择将以下行添加到 php.ini 文件，以便在其所有 php 脚本中关闭此功能：

```
pdo_sqlsrv.report_additional_errors = 0
```

## <a name="warnings-and-errors"></a>警告和错误

从 5.9.0 开始，ODBC 警告将不再记录为错误。 也就是说，前缀为“01”的[错误代码](https://docs.microsoft.com/sql/odbc/reference/appendixes/appendix-a-odbc-error-codes)将记录为警告。 换句话说，如果用户只想记录错误，请按如下所示更新 php.ini：

```
[pdo_sqlsrv]  
pdo_sqlsrv.log_severity = 1
```

在这种情况下，日志文件将不会包含任何警告消息。 请检查 pdo_sqlsrv 用户[日志记录](https://docs.microsoft.com/sql/connect/php/logging-activity#logging-activity-using-the-pdo_sqlsrv-driver)的工作方式。

## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  

[PDOStatement::errorInfo](../../connect/php/pdostatement-errorinfo.md)
