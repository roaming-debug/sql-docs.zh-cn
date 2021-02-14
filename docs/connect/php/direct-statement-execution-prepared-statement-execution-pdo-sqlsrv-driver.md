---
title: PDO_SQLSRV 驱动程序中的直接语句执行和准备的语句执行
description: 了解在使用 Microsoft PDO_SQLSRV Driver for PHP for SQL Server 时如何使用 PDO::SQLSRV_ATTR_DIRECT_QUERY 属性进行直接语句执行
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 05544ca6-1e07-486c-bf03-e8c2c25b3024
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4dc9177ed83e349b4cdd3d38da34285ee90a1f1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045387"
---
# <a name="direct-statement-execution-and-prepared-statement-execution-in-the-pdo_sqlsrv-driver"></a>PDO_SQLSRV 驱动程序中的直接语句执行和预定语句执行
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

本主题讨论如何使用 PDO::SQLSRV_ATTR_DIRECT_QUERY 属性指定直接语句执行而非默认执行，即已准备的语句执行。 如果使用参数绑定多次执行准备的语句，那么使用此语句可以提升性能。  
  
## <a name="remarks"></a>注解  
若要将 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句直接发送到服务器，而无需由驱动程序进行语句准备，可以使用 [PDO::setAttribute](../../connect/php/pdo-setattribute.md) 或在调用 [PDO::prepare](../../connect/php/pdo-prepare.md) 时设置 PDO::SQLSRV_ATTR_DIRECT_QUERY 属性（或作为传递到 [PDO::__construct](../../connect/php/pdo-construct.md) 的驱动程序选项）。 PDO::SQLSRV_ATTR_DIRECT_QUERY 的默认值为 False（即使用准备的语句执行）。  
  
如果使用 [PDO::query](../../connect/php/pdo-query.md)，不妨设置直接语句执行。 调用 [PDO::query](../../connect/php/pdo-query.md) 前，请先调用 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)，并将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 True。  每次调用 [PDO::query](../../connect/php/pdo-query.md) 都可以使用不同的 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置来执行。  
  
如果你使用 [PDO::prepare](../../connect/php/pdo-prepare.md) 和 [PDOStatement::execute](../../connect/php/pdostatement-execute.md) 通过绑定参数多次执行查询，准备的语句执行会优化重复查询的执行。  在这种情况下，调用 [PDO::prepare](../../connect/php/pdo-prepare.md)，同时在驱动程序选项数组参数中将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 False。 必要时，可以通过将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 False 来执行准备的语句。  
  
调用 [PDO::prepare](../../connect/php/pdo-prepare.md) 后，不能在执行准备的查询时更改 PDO::SQLSRV_ATTR_DIRECT_QUERY 的值。  
  
如果查询需要在上一个查询中设置的上下文，请通过将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 True 来执行查询。 例如，如果在查询中使用临时表，必须将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 True。  
  
下面的示例表明，如果需要上一个语句中的上下文，必须将 PDO::SQLSRV_ATTR_DIRECT_QUERY 设置为 True。 此示例使用临时表，这些表仅在直接执行查询时对程序中的后续语句可用。  
  
> [!NOTE]
> 如果查询用于调用存储过程，且此存储过程中使用了临时表，请改用 [PDO::exec](../../connect/php/pdo-exec.md)。

```  
<?php  
   $conn = new PDO('sqlsrv:Server=(local)', '', '');  
   $conn->setAttribute(constant('PDO::SQLSRV_ATTR_DIRECT_QUERY'), true);  
  
   $stmt1 = $conn->query("DROP TABLE #php_test_table");  
  
   $stmt2 = $conn->query("CREATE TABLE #php_test_table ([c1_int] int, [c2_int] int)");  
  
   $v1 = 1;  
   $v2 = 2;  
  
   $stmt3 = $conn->prepare("INSERT INTO #php_test_table (c1_int, c2_int) VALUES (:var1, :var2)");  
  
   if ($stmt3) {  
      $stmt3->bindValue(1, $v1);  
      $stmt3->bindValue(2, $v2);  
  
      if ($stmt3->execute())  
         echo "Execution succeeded\n";       
      else  
         echo "Execution failed\n";  
   }  
   else  
      var_dump($conn->errorInfo());  
  
   $stmt4 = $conn->query("DROP TABLE #php_test_table");  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[Microsoft Drivers for PHP for SQL Server 编程指南](../../connect/php/programming-guide-for-php-sql-driver.md)
  
