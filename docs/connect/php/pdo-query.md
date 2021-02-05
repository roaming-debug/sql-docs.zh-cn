---
title: PDO::query
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::query 函数的 API 参考。
ms.custom: ''
ms.date: 08/01/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: f6f5e6d4-8ca9-4f06-89ed-de65ad3952a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba25e0792271ccb1ffb77e32ff1673b72cef4c60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195247"
---
# <a name="pdoquery"></a>PDO::query
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

执行 SQL 查询并返回一个结果集作为 PDOStatement 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
PDOStatement PDO::query ($statement[, $fetch_style);  
```  
  
#### <a name="parameters"></a>参数  
*$statement*：要执行的 SQL 语句。  
  
$fetch_style：有关如何执行查询的说明（可选）。 有关更多详细信息，请参阅“备注”部分。PDO::query 中的  $fetch_style 可以替换为 PDO::fetch 中的 $fetch_style 。  
  
## <a name="return-value"></a>返回值  
如果调用成功，PDO::query 将返回一个 PDOStatement 对象。 如果调用失败，PDO::query 将引发一个 PDOException 对象或者返回 False，具体取决于 PDO::ATTR_ERRMODE 的设置。  
  
## <a name="exceptions"></a>例外  
PDOException。  
  
## <a name="remarks"></a>注解  
使用 PDO::query 执行的查询可以执行已准备的语句或直接执行语句，具体取决于 PDO::SQLSRV_ATTR_DIRECT_QUERY 的设置。 有关详细信息，请参阅 [PDO_SQLSRV 驱动程序中的直接语句执行和预定语句执行](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md)。  
  
PDO::SQLSRV_ATTR_QUERY_TIMEOUT 还会影响 PDO::exec 的行为；有关详细信息，请参阅 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)。  
  
可以为 fetch_style 指定以下选项。  
  
|Style|说明|  
|---------|---------------|  
|PDO::FETCH_COLUMN, num|在指定列中查询数据。 表中的第一列为列 0.|  
|PDO::FETCH_CLASS, 'classname', array( arglist )|创建一个类的实例并给类中的属性分配列名称。 如果类构造函数采用一个或多个参数，你还可以传递 *arglist*。|  
|PDO::FETCH_CLASS, 'classname'|给现有类中的属性分配列名称。|  
  
在再次调用 PDO::query 之前，请调用 PDOStatement::closeCursor 以释放与 PDOStatement 对象相关联的数据库资源。  
  
通过将 PDOStatement 对象设置为 NULL 来将其关闭。  
  
如果结果集中的所有数据都无法提取，下次调用 PDO::query 时将不会失败。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="query-example"></a>查询示例  
此示例显示多个查询。  
  
```  
<?php  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
$conn->setAttribute( PDO::SQLSRV_ATTR_QUERY_TIMEOUT, 1 );  
  
$query = 'select * from Person.ContactType';  
  
// simple query  
$stmt = $conn->query( $query );  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print_r( $row['Name'] ."\n" );  
}  
  
echo "\n........ query for a column ............\n";  
  
// query for one column  
$stmt = $conn->query( $query, PDO::FETCH_COLUMN, 1 );  
while ( $row = $stmt->fetch() ){  
   echo "$row\n";  
}  
  
echo "\n........ query with a new class ............\n";  
$query = 'select * from HumanResources.Department order by GroupName';  
// query with a class  
class cc {  
   function __construct( $arg ) {  
      echo "$arg";  
   }  
  
   function __toString() {  
      return $this->DepartmentID . "; " . $this->Name . "; " . $this->GroupName;  
   }  
}  
  
$stmt = $conn->query( $query, PDO::FETCH_CLASS, 'cc', array( "arg1 " ));  
  
while ( $row = $stmt->fetch() ){  
   echo "$row\n";  
}  
  
echo "\n........ query into an existing class ............\n";  
$c_obj = new cc( '' );  
$stmt = $conn->query( $query, PDO::FETCH_INTO, $c_obj );  
while ( $stmt->fetch() ){  
   echo "$c_obj\n";  
}  
  
$stmt = null;  
?>  
```

## <a name="sql_variant-example"></a>Sql_variant 示例
此代码示例演示如何创建 [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) 类型的表并提取插入的数据。

```
<?php
$server = 'serverName';
$dbName = 'databaseName';
$uid = 'yourUserName';
$pwd = 'yourPassword';

$conn = new PDO("sqlsrv:server=$server; database = $dbName", $uid, $pwd);
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  

try {
    $tableName = 'testTable';
    $query = "CREATE TABLE $tableName ([c1_int] sql_variant, [c2_varchar] sql_variant)";

    $stmt = $conn->query($query);
    unset($stmt);

    $query = "INSERT INTO [$tableName] (c1_int, c2_varchar) VALUES (1, 'test_data')";
    $stmt = $conn->query($query);
    unset($stmt);

    $query = "SELECT * FROM $tableName";
    $stmt = $conn->query($query);

    $result = $stmt->fetch(PDO::FETCH_ASSOC);
    print_r($result);
    
    unset($stmt);
    unset($conn);
} catch (Exception $e) {
    echo $e->getMessage();
}
?>
```

预期的输出为：

```
Array
(
    [c1_int] => 1
    [c2_varchar] => test_data
)
```

## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
