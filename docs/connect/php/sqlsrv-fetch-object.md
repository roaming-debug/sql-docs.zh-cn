---
description: sqlsrv_fetch_object
title: sqlsrv_fetch_object | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_fetch_object
apitype: NA
helpviewer_keywords:
- sqlsrv_fetch_object
- API Reference, sqlsrv_fetch_object
- retrieving data, as an object
ms.assetid: 4ce2df2c-083a-4a4d-a1e2-e866e63707d5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6b173c2676f25b4866e1981611c1375e4cd1bf6b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99154471"
---
# <a name="sqlsrv_fetch_object"></a>sqlsrv_fetch_object
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

将下一行数据检索为 PHP 对象。  
  
## <a name="syntax"></a>语法  
  
```  
sqlsrv_fetch_object( resource $stmt [, string $className [, array $ctorParams[, row[, ]offset]]])  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：对应于已执行语句的语句资源。  
  
$className [可选]：指定要实例化的类名称的字符串。 如果不指定 *$className* 参数的值，将实例化 PHP **stdClass** 的实例。  
  
$ctorParams [可选]：含有传递到通过 $className 参数指定的类构造函数的值的阵列。 如果指定类的构造函数接受参数值，在调用 *$ctorParams* object **sqlsrv_fetch_object** 参数。  
  
*row* [可选]：以下值之一，用于指定要在使用可滚动游标的结果集中访问的行。 （如果指定了 row，则必须显式指定 $className 和 $ctorParams，即使必须将 $className 和 $ctorParams 指定为 null    。）  
  
-   SQLSRV_SCROLL_NEXT  
  
-   SQLSRV_SCROLL_PRIOR  
  
-   SQLSRV_SCROLL_FIRST  
  
-   SQLSRV_SCROLL_LAST  
  
-   SQLSRV_SCROLL_ABSOLUTE  
  
-   SQLSRV_SCROLL_RELATIVE  
  
有关这些值的详细信息，请参阅 [指定游标类型和选择行](../../connect/php/specifying-a-cursor-type-and-selecting-rows.md)。  
  
*offset* [可选]：结合使用 SQLSRV_SCROLL_ABSOLUTE 和 SQLSRV_SCROLL_RELATIVE 以指定要检索的行。 结果集中的第一个记录为 0。  
  
## <a name="return-value"></a>返回值  
具有对应于结果集字段名称的属性的 PHP 对象。 使用相应的结果集字段值来填充属性值。 如果使用可选 *$className* 参数指定的类不存在，或如果指定的语句未关联任何活动的结果集，将返回 **False** 。 如果没有更多要检索的行，将返回 **NULL** 。  
  
返回对象中值的数据类型将是默认 PHP 数据类型。 有关默认 PHP 数据类型的信息，请参阅 [Default PHP Data Types](../../connect/php/default-php-data-types.md)。  
  
## <a name="remarks"></a>注解  
如果使用可选 *$className* 参数指定类名，将实例化此类类型的对象。 如果该类所具有的属性名称与结果集字段名称相匹配，则相应的结果集值将应用到该属性。 如果结果集字段名称不匹配类属性，则具有结果集字段名称的属性将添加到该对象，并该结果集值将应用到该属性。  
  
使用 *$className* 参数指定某个类时，应用以下规则：  
  
-   匹配区分大小写。 例如，属性名 CustomerId 不匹配字段名称 CustomerID。 在这种情况下，CustomerID 属性将添加到对象，并且 CustomerID 字段的值将赋值给 CustomerID 属性。  
  
-   无论访问修饰符如何，都会进行匹配。 例如，如果指定类所具有的私有属性的名称与结果集字段名称相匹配，该结果集字段的值将应用到该属性。  
  
-   忽略类属性数据类型。 如果结果集中的“CustomerID”字段为字符串，而类的“CustomerID”属性为整数，则将该结果集的字符串值写入“CustomerID”属性。  
  
-   如果指定的类不存在，该函数将返回 **false** ，并会向错误集合中添加一个错误。 有关检索错误信息的信息，请参阅 [sqlsrv_errors](../../connect/php/sqlsrv-errors.md)。  
  
如果返回不含名称的字段， **sqlsrv_fetch_object** 会丢弃该字段值并发出一条警告。 例如，考虑可将某个值插入数据库表并检索服务器生成的主键的 Transact-SQL 语句：  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
如果通过 **sqlsrv_fetch_object** 检索由此查询返回的结果，将丢弃 `SELECT SCOPE_IDENTITY()` 所返回的值并发出一条警告。 若要避免出现上述情形，可以在 Transact-SQL 语句中为返回的字段指定一个名称。 以下方法可用于在 Transact-SQL 中指定一个列名：  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="object-example"></a>对象示例  
以下示例将每一行结果集检索为 PHP 对象。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
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
  
/* Set up and execute the query. */  
$tsql = "SELECT FirstName, LastName  
         FROM Person.Contact  
         WHERE LastName='Alan'";  
$stmt = sqlsrv_query( $conn, $tsql);  
if( $stmt === false )  
{  
     echo "Error in query preparation/execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Retrieve each row as a PHP object and display the results.*/  
while( $obj = sqlsrv_fetch_object( $stmt))  
{  
      echo $obj->LastName.", ".$obj->FirstName."\n";  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="class-example"></a>类示例  
以下示例将每一行结果集检索为在脚本中定义的 *Product* 类的实例。 该示例将从产品（具有指定到期日期 DueDate 且库存量 StockQty 小于指定值）的 AdventureWorks 数据库的 Purchasing.PurchaseOrderDetail 和 Production.Product 表中检索产品信息   。 该示例重点介绍在对 **sqlsrv_fetch_object** 的调用中指定某个类时所应用的某些规则：  
  
-   *$product* 变量是 *Product* 类的实例，为存在使用 *$className* 参数和 *Product* 类指定的“Product”。  
  
-   *Name* 属性将添加到 *$product* 实例，因为现有 *name* 属性不匹配。  
  
-   *Color* 属性将添加到 *$product* 实例，因为没有任何匹配属性。  
  
-   将使用 *UnitPrice* 字段的值来填充私有属性 *UnitPrice* 。  
  
该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```  
<?php  
/* Define the Product class. */  
class Product  
{  
     /* Constructor */  
     public function Product($ID)  
     {  
          $this->objID = $ID;  
     }  
     public $objID;  
     public $name;  
     public $StockedQty;  
     public $SafetyStockLevel;  
     private $UnitPrice;  
     function getPrice()  
     {  
          return $this->UnitPrice;  
     }  
}  
  
/* Connect to the local server using Windows Authentication, and  
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Define the query. */  
$tsql = "SELECT Name,  
                SafetyStockLevel,  
                StockedQty,  
                UnitPrice,  
                Color  
         FROM Purchasing.PurchaseOrderDetail AS pdo  
         JOIN Production.Product AS p  
         ON pdo.ProductID = p.ProductID  
         WHERE pdo.StockedQty < ?  
         AND pdo.DueDate= ?";  
  
/* Set the parameter values. */  
$params = array(3, '2002-01-29');  
  
/* Execute the query. */  
$stmt = sqlsrv_query( $conn, $tsql, $params);  
if ( $stmt )  
{  
     echo "Statement executed.\n";  
}   
else   
{  
     echo "Error in statement execution.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
/* Iterate through the result set, printing a row of data upon each  
 iteration. Note the following:  
     1) $product is an instance of the Product class.  
     2) The $ctorParams parameter is required in the call to  
        sqlsrv_fetch_object, because the Product class constructor is  
        explicity defined and requires parameter values.  
     3) The "Name" property is added to the $product instance because  
        the existing "name" property does not match.  
     4) The "Color" property is added to the $product instance  
        because there is no matching property.  
     5) The private property "UnitPrice" is populated with the value  
        of the "UnitPrice" field.*/  
$i=0; //Used as the $objID in the Product class constructor.  
while( $product = sqlsrv_fetch_object( $stmt, "Product", array($i)))  
{  
     echo "Object ID: ".$product->objID."\n";  
     echo "Product Name: ".$product->Name."\n";  
     echo "Stocked Qty: ".$product->StockedQty."\n";  
     echo "Safety Stock Level: ".$product->SafetyStockLevel."\n";  
     echo "Product Color: ".$product->Color."\n";  
     echo "Unit Price: ".$product->getPrice()."\n";  
     echo "-----------------\n";  
     $i++;  
}  
  
/* Free statement and connection resources. */  
sqlsrv_free_stmt( $stmt);  
sqlsrv_close( $conn);  
?>  
```  
  
sqlsrv_fetch_object 函数将始终根据[默认 PHP 数据类型](../../connect/php/default-php-data-types.md)返回数据。 有关如何指定 PHP 数据类型的信息，请参阅[如何：指定 PHP 数据类型](../../connect/php/how-to-specify-php-data-types.md)。  
  
如果返回不含名称的字段， **sqlsrv_fetch_object** 会丢弃该字段值并发出一条警告。 例如，考虑可将某个值插入数据库表并检索服务器生成的主键的 Transact-SQL 语句：  
  
```sql
INSERT INTO Production.ProductPhoto (LargePhoto) VALUES (?);  
SELECT SCOPE_IDENTITY()
```
  
如果通过 **sqlsrv_fetch_object** 检索由此查询返回的结果，将丢弃 `SELECT SCOPE_IDENTITY()` 所返回的值并发出一条警告。 若要避免出现上述情形，可以在 Transact-SQL 语句中为返回的字段指定一个名称。 以下方法可用于在 Transact-SQL 中指定一个列名：  
  
```sql
SELECT SCOPE_IDENTITY() AS PictureID
```
  
## <a name="see-also"></a>另请参阅  
[检索数据](../../connect/php/retrieving-data.md)  

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  

[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  
  
