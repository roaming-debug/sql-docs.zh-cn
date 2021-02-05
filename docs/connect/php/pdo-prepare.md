---
title: PDO::prepare
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::prepare 函数的 API 参考。
ms.custom: ''
ms.date: 01/31/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: a8b16fdc-c748-49be-acf2-a6ac7432d16b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f05ecd9241b97fa4864451e79674b9a57bca2a6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206186"
---
# <a name="pdoprepare"></a>PDO::prepare
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

准备语句用于执行。

## <a name="syntax"></a>语法

```
PDOStatement PDO::prepare ( $statement [, array(key_pair)] )
```

#### <a name="parameters"></a>参数
$*语句*：包含 SQL 语句的字符串。

*key_pair*：包含属性名称和值的数组。 有关详细信息，请参阅备注部分。

## <a name="return-value"></a>返回值
如果成功，则返回 PDOStatement 对象。 如果失败，则返回 PDOException 对象或 False，具体取决于 `PDO::ATTR_ERRMODE` 的值。

## <a name="remarks"></a>注解
[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 在执行已准备的语句之前不会对其进行评估。

下表列出可能的 key_pair 值。

|键|说明|
|-------|---------------|
|PDO::ATTR_CURSOR|指定游标行为。 默认值为 `PDO::CURSOR_FWDONLY`，一个不可滚动的前向游标。 `PDO::CURSOR_SCROLL` 是可滚动的游标。<br /><br />例如，`array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY )`。<br /><br />如果设置为 `PDO::CURSOR_SCROLL`，则可以使用 `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` 设置可滚动游标的类型，如下所述。<br /><br />有关 PDO_SQLSRV 驱动程序中的结果集和游标的详细信息，请参阅[游标类型（PDO_SQLSRV 驱动程序）](../../connect/php/cursor-types-pdo-sqlsrv-driver.md)。|
|PDO::ATTR_EMULATE_PREPARES|默认情况下，此属性为 false，可通过此 `PDO::ATTR_EMULATE_PREPARES => true` 更改。 有关详细信息和示例，请参阅[模拟准备](#emulate-prepare)。|
|PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE|指定可滚动游标的类型。 仅在 `PDO::ATTR_CURSOR` 设置为 `PDO::CURSOR_SCROLL` 时有效。 有关此属性可采用的值，请参阅下文。|
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|指定设置提取的 Money 值格式时的小数位数。 仅当 `PDO::SQLSRV_ATTR_FORMAT_DECIMALS` 为 true 时此选项才起作用。 有关详细信息，请参阅[设置十进制字符串和 Money 值格式（PDO_SQLSRV 驱动程序）](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)。|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|如果为 True，则指定直接查询执行。 False 表示已准备的语句执行。 有关 `PDO::SQLSRV_ATTR_DIRECT_QUERY` 的详细信息，请参阅 [PDO_SQLSRV 驱动程序中的直接语句执行和预定语句执行](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md)。|
|PDO::SQLSRV_ATTR_ENCODING|PDO::SQLSRV_ENCODING_UTF8（默认值）<br /><br />PDO::SQLSRV_ENCODING_SYSTEM<br /><br />PDO::SQLSRV_ENCODING_BINARY|
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|指定是否以 [PHP DateTime](http://php.net/manual/en/class.datetime.php) 对象形式检索日期和时间类型。 有关详细信息，请参阅[操作说明：使用 PDO_SQLSRV 驱动程序检索日期和时间类型作为 PHP Datetime 对象](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|处理具有数值 SQL 类型的列中的数值提取。 有关详细信息，请参阅 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)。|
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|指定是否在合适时向十进制字符串添加前导零。 如已设置，此选项将启用用于设置 Money 类型格式的 `PDO::SQLSRV_ATTR_DECIMAL_PLACES` 选项。 有关详细信息，请参阅[设置十进制字符串和 Money 值格式（PDO_SQLSRV 驱动程序）](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)。| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|有关详细信息，请参阅 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)。|

使用 `PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` 时，可以使用 `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` 来指定游标的类型。 例如，将以下数组传递给 PDO::prepare 来设置动态游标：
```
array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL, PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_DYNAMIC));
```
下表列出了 `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE` 的可能值。 有关可滚动游标的详细信息，请参阅[游标类型（PDO_SQLSRV 驱动程序）](../../connect/php/cursor-types-pdo-sqlsrv-driver.md)。

|值|说明|
|---------|---------------|
|PDO::SQLSRV_CURSOR_BUFFERED|创建一个客户端（缓冲）静态游标，该游标将结果集缓冲到客户端计算机上的内存中。|
|PDO::SQLSRV_CURSOR_DYNAMIC|创建服务器端（无缓冲）动态游标，该游标支持你采用任意顺序访问行，并且可以反映数据库中的更改。|
|PDO::SQLSRV_CURSOR_KEYSET|创建服务器端键集游标。 如果从表中删除某一行，键集游标不会更新行计数（将返回不含任何值的已删除行）。|
|PDO::SQLSRV_CURSOR_STATIC|创建服务器端静态游标，该游标支持你采用任意顺序访问行，但不会反映数据库中的更改。<br /><br />`PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL` 意味着 `PDO::SQLSRV_ATTR_CURSOR_SCROLL_TYPE => PDO::SQLSRV_CURSOR_STATIC`。|

可以通过调用 `unset` 关闭 PDOStatement 对象：
```
unset($stmt);
```

## <a name="forward-only-example"></a>只进游标示例
本示例演示如何将 PDO::prepare 与参数标记以及只进游标结合使用。

```
<?php
$database = "Test";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$col1 = 'a';
$col2 = 'b';

$query = "insert into Table1(col1, col2) values(?, ?)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( $col1, $col2 ) );
print $stmt->rowCount();
echo "\n";

$query = "insert into Table1(col1, col2) values(:col1, :col2)";
$stmt = $conn->prepare( $query, array( PDO::ATTR_CURSOR => PDO::CURSOR_FWDONLY, PDO::SQLSRV_ATTR_QUERY_TIMEOUT => 1  ) );
$stmt->execute( array( ':col1' => $col1, ':col2' => $col2 ) );
print $stmt->rowCount();

unset($stmt);
?>
```

## <a name="static-cursor-example"></a>静态游标示例
此示例演示如何将 PDO::prepare 与服务器端静态游标结合使用。 有关展示客户端端游标的示例，请参阅[游标类型（PDO_SQLSRV 驱动程序）](../../connect/php/cursor-types-pdo-sqlsrv-driver.md)。

```
<?php
$database = "AdventureWorks";
$server = "(local)";
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");

$query = "select * from Person.ContactType";
$stmt = $conn->prepare( $query, array(PDO::ATTR_CURSOR => PDO::CURSOR_SCROLL));
$stmt->execute();

echo "\n";

while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){
   print "$row[Name]\n";
}
echo "\n..\n";

$row = $stmt->fetch( PDO::FETCH_BOTH, PDO::FETCH_ORI_FIRST );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_ASSOC, PDO::FETCH_ORI_REL, 1 );
print "$row[Name]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_NEXT );
print "$row[1]\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_PRIOR );
print "$row[1]..\n";

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_ABS, 0 );
print_r($row);

$row = $stmt->fetch( PDO::FETCH_NUM, PDO::FETCH_ORI_LAST );
print_r($row);
?>
```

## <a name="targeted-example"></a>目标示例
以下两个代码段演示如何将 PDO::prepare 与面向 CHAR/VARCHAR 列的数据一起使用。 因为 PDO::prepare 的默认编码为 UTF-8，所以用户可以使用选项 `PDO::SQLSRV_ENCODING_SYSTEM` 来避免隐式转换。

**选项 1**
```
$options = array(PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_SYSTEM);
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue',
  $options
);

$statement->bindValue(':myVarcharValue', 'my data', PDO::PARAM_STR);
```

**方法 2**
```
$statement = $pdo->prepare(
  'SELECT *
   FROM myTable
   WHERE myVarcharColumn = :myVarcharValue'
);
$p = 'my data';
$statement->bindParam(':myVarcharValue', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_SYSTEM);
```

<a name="emulate-prepare" />

## <a name="prepare-example"></a>Prepare 示例

此示例演示如何将 PDO::prepare 设置为 true 的 `PDO::ATTR_EMULATE_PREPARES` 结合使用。

```
<?php
$serverName = "yourservername";
$username = "yourusername";
$password = "yourpassword";
$database = "tempdb";
$conn = new PDO("sqlsrv:server = $serverName; Database = $database", $username, $password);

$pdo_options = array();
$pdo_options[PDO::ATTR_EMULATE_PREPARES] = true;
$pdo_options[PDO::SQLSRV_ATTR_ENCODING] = PDO::SQLSRV_ENCODING_UTF8;

$stmt = $conn->prepare("CREATE TABLE TEST([id] [int] IDENTITY(1,1) NOT NULL,
                                          [name] nvarchar(max))",
                                          $pdo_options);
$stmt->execute();

$prefix = '가각';
$name = '가각ácasa';
$name2 = '가각sample2';

$stmt = $conn->prepare("INSERT INTO TEST(name) VALUES(:p0)", $pdo_options);
$stmt->execute(['p0' => $name]);
unset($stmt);

$stmt = $conn->prepare("SELECT * FROM TEST WHERE NAME LIKE :p0", $pdo_options);
$stmt->execute(['p0' => "$prefix%"]);
foreach ($stmt as $row) {
    echo "\n" . 'FOUND: ' . $row['name'];
}

unset($stmt);
unset($conn);
?>
```

PDO_SQLSRV 驱动程序在内部用 [PDOStatement::bindParam()](../../connect/php/pdostatement-bindparam.md) 绑定的参数替换所有占位符。 因此，将向服务器发送一个没有占位符的 SQL 查询字符串。 请看下面的示例，

```
$statement = $PDO->prepare("INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)");
$statement->bindParam(:cus_name, "Cardinal");
$statement->bindParam(:con_name, "Tom B. Erichsen");
$statement->execute();
```

将 `PDO::ATTR_EMULATE_PREPARES` 设置为 false（默认情况），发送到数据库的数据为：

```
"INSERT into Customers (CustomerName, ContactName) VALUES (:cus_name, :con_name)"
Information on :cus_name parameter
Information on :con_name parameter
```

服务器将使用绑定参数的参数化查询功能执行查询。 另一方面，当 `PDO::ATTR_EMULATE_PREPARES` 设置为 true 时，发送到服务器的查询本质上是：

```
"INSERT into Customers (CustomerName, ContactName) VALUES ('Cardinal', 'Tom B. Erichsen')"
```

将 `PDO::ATTR_EMULATE_PREPARES` 设置为 true 可以绕过 SQL Server 中的一些限制。 例如，SQL Server 在某些 Transact-SQL 子句中不支持命名参数或位置参数。 此外，SQL Server 限制最多绑定 2100 个参数。

> [!NOTE]
> 将模拟准备设置为 true 时，参数化查询的安全性不会生效。 因此，应用程序应确保绑定到参数的数据不包含恶意 Transact-SQL 代码。

### <a name="encoding"></a>编码

如果用户希望使用不同的编码绑定参数（例如，UTF-8 或二进制），则应在 PHP 脚本中明确指定编码。

PDO_SQLSRV 驱动程序首先检查 `PDO::bindParam()` 中指定的编码（例如 `$statement->bindParam(:cus_name, "Cardinal", PDO::PARAM_STR, 10, PDO::SQLSRV_ENCODING_UTF8)`）。

如果找不到，则驱动程序会检查是否在 `PDO::prepare()` 或 `PDOStatement::setAttribute()` 中设置了任何编码。 否则，驱动程序将使用 `PDO::__construct()` 或 `PDO::setAttribute()` 中指定的编码。

此外，从版本 5.8.0 开始，在使用 PDO::prepare 时，如果 `PDO::ATTR_EMULATE_PREPARES` 设置为 true，则用户可以使用 [PHP 7.2 中引入的扩展字符串类型](https://wiki.php.net/rfc/extended-string-types-for-pdo)，以确保使用 `N` 前缀。 下面的代码段显示了各种备选方法。

> [!NOTE]
> 默认情况下，模拟准备设置为 false，在这种情况下，将忽略扩展 PDO 字符串常量。

**绑定时使用驱动程序选项 PDO::SQLSRV_ENCODING_UTF8**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR, 0, PDO::SQLSRV_ENCODING_UTF8);
$stmt->execute();
```

**使用 PDO::SQLSRV_ATTR_ENCODING 属性**

```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true, PDO::SQLSRV_ATTR_ENCODING => PDO::SQLSRV_ENCODING_UTF8);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

**使用 PDO 常量 PDO::PARAM_STR_NATL**
```
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->bindParam(':value', $p, PDO::PARAM_STR | PDO::PARAM_STR_NATL);
$stmt->execute();
```

**设置默认字符串参数类型 PDO::PARAM_STR_NATL**
```
$conn->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$p = '가각';
$sql = 'SELECT :value';
$options = array(PDO::ATTR_EMULATE_PREPARES => true);
$stmt = $conn->prepare($sql, $options);
$stmt->execute([':value' => $p]);
```

### <a name="limitations"></a>限制

如你所见，绑定是由驱动程序在内部完成的。 一个有效的查询会被发送到服务器，以便在没有任何参数的情况下完成执行。 与常规情况相比，当不使用参数化查询功能时，会产生一些限制。

- 它不适用于绑定为 `PDO::PARAM_INPUT_OUTPUT` 的参数。
    - 当用户在 `PDO::bindParam()` 中指定 `PDO::PARAM_INPUT_OUTPUT` 时，将引发 PDO 异常。
- 它不适用于绑定为输出参数的参数。
    - 当用户使用用于输出参数的占位符创建一个准备好的语句时（也就是说，在占位符后面有一个等号，比如 `SELECT ? = COUNT(*) FROM Table1`），将引发 PDO 异常。
    - 当准备好的语句调用将占位符作为输出参数自变量的存储过程时，不会引发异常，因为驱动程序无法检测输出参数。 但是，用户为输出参数提供的变量将保持不变。
- 二进制编码参数的重复占位符将不起作用

## <a name="see-also"></a>另请参阅
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)

