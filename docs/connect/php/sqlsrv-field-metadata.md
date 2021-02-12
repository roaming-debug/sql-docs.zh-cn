---
description: sqlsrv_field_metadata
title: sqlsrv_field_metadata | Microsoft Docs
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_field_metadata
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_field_metadata
- sqlsrv_field_metadata
ms.assetid: c02f6942-0484-4567-a78e-fe8aa2053536
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 45b617e91f2f296aa9749f7f8f2f25b640ddf546
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623767"
---
# <a name="sqlsrv_field_metadata"></a>sqlsrv_field_metadata
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

检索已准备语句的字段的元数据。 有关准备语句的信息，请参阅 [sqlsrv_query](../../connect/php/sqlsrv-query.md) 或 [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md)。 请注意，无论在执行前还是执行后，都可以在任何已准备的语句上调用 sqlsrv_field_metadata。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_field_metadata( resource $stmt)  
```  
  
#### <a name="parameters"></a>参数  
*$stmt*：寻找字段元数据的语句资源。  
  
## <a name="return-value"></a>返回值  
数组的 **array** 或 **false**。 数组由结果集中的每个字段的一个数组组成。 每个子数组具有下表中所述的键。 如果在检索字段元数据时出现错误，则返回 **false** 。  
  
|密钥|说明|  
|-------|---------------|  
|名称|字段对应的列的名称。|  
|类型|对应于 SQL 类型的数值。|  
|大小|字符类型（char(n)、varchar(n)、nchar(n)、nvarchar(n)、XML）的字段的字符数。 二进制类型（binary(n)、varbinary(n)、UDT）的字段的字节数。 **NULL** 用于其他 SQL Server 数据类型。|  
|精准率|变量精度类型（real、numeric、decimal、datetime2、datetimeoffset 和 time）的精度。 **NULL** 用于其他 SQL Server 数据类型。|  
|缩放|变量小数位数类型（numeric、decimal、datetime2、datetimeoffset 和 time）的小数位数。 **NULL** 用于其他 SQL Server 数据类型。|  
|Nullable|指示列可以为 null (SQLSRV_NULLABLE_YES)、不可为 null (SQLSRV_NULLABLE_NO) 还是未知列是否可以为 null (SQLSRV_NULLABLE_UNKNOWN) 的枚举值。|  
  
下表提供有关每个子数组的键的详细信息（有关这些类型的详细信息，请参阅 SQL Server 文档）：  
  
|SQL Server 2008 数据类型|类型|最小/最大精度|最小/最大小数位数|大小|  
|-----------------------------|--------|----------------------|------------------|--------|  
|bigint|SQL_BIGINT (-5)|||8|  
|binary|SQL_BINARY (-2)|||0 < n < 8000 <sup>1</sup>|  
|bit|SQL_BIT (-7)||||  
|char|SQL_CHAR (1)|||0 < n < 8000 <sup>1</sup>|  
|date|SQL_TYPE_DATE (91)|10/10|0/0||  
|datetime|SQL_TYPE_TIMESTAMP (93)|23/23|3/3||  
|datetime2|SQL_TYPE_TIMESTAMP (93)|19/27|0/7||  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET (-155)|26/34|0/7||  
|Decimal|SQL_DECIMAL (3)|1/38|0/精度值||  
|float|SQL_FLOAT (6)|4/8|||  
|image|SQL_LONGVARBINARY (-4)|||2 GB|  
|int|SQL_INTEGER (4)||||  
|money|SQL_DECIMAL (3)|19/19|4/4||  
|nchar|SQL_WCHAR (-8)|||0 < n < 4000 <sup>1</sup>|  
|ntext|SQL_WLONGVARCHAR (-10)|||1 GB|  
|numeric|SQL_NUMERIC (2)|1/38|0/精度值||  
|nvarchar|SQL_WVARCHAR (-9)|||0 < n < 4000 <sup>1</sup>|  
|real|SQL_REAL (7)|4/4|||  
|smalldatetime|SQL_TYPE_TIMESTAMP (93)|16/16|0/0||  
|smallint|SQL_SMALLINT (5)|||2 个字节|  
|Smallmoney|SQL_DECIMAL (3)|10/10|4/4||  
|sql_variant|SQL_SS_VARIANT (-150)|||可变|  
|文本|SQL_LONGVARCHAR (-1)|||2 GB|  
|time|SQL_SS_TIME2 (-154)|8/16|0/7||  
|timestamp|SQL_BINARY (-2)|||8 个字节|  
|tinyint|SQL_TINYINT (-6)|||1 个字节|  
|udt|SQL_SS_UDT (-151)|||可变|  
|uniqueidentifier|SQL_GUID (-11)|||16|  
|varbinary|SQL_VARBINARY (-3)|||0 < n < 8000 <sup>1</sup>|  
|varchar|SQL_VARCHAR (12)|||0 < n < 8000 <sup>1</sup>|  
|xml|SQL_SS_XML (-152)|||0|  
  
(1) 零 (0) 指示允许最大大小。  
  
可以为 Null 的键可能为是或否。  
  
## <a name="example"></a>示例  
以下示例创建语句资源，然后检索并显示字段元数据。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/adventure-works) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```
<?php
/* Connect to the local server using Windows Authentication and
specify the AdventureWorks database as the database in use. */
$serverName = "(local)";
$connectionInfo = array("Database"=>"AdventureWorks");
$conn = sqlsrv_connect($serverName, $connectionInfo);
if ($conn === false) {
    echo "Could not connect.\n";
    die( print_r( sqlsrv_errors(), true));
}

/* Prepare the statement. */
$tsql = "SELECT ReviewerName, Comments FROM Production.ProductReview";
$stmt = sqlsrv_prepare( $conn, $tsql);
  
/* Get and display field metadata. */
foreach( sqlsrv_field_metadata( $stmt) as $fieldMetadata) {
    foreach( $fieldMetadata as $name => $value) {
        echo "$name: $value\n";
    }  
    echo "\n";
}  
  
/* Note: sqlsrv_field_metadata can be called on any statement
resource, pre- or post-execution. */
  
/* Free statement and connection resources. */
sqlsrv_free_stmt($stmt);
sqlsrv_close($conn);
?>
```

## <a name="sensitivity-data-classification-metadata"></a>敏感度数据分类元数据

版本 5.8.0 中引入了新选项 `DataClassification`，用户可以使用该选项通过 `sqlsrv_field_metadata`（需要 Microsoft ODBC Driver 17.4.2 或更高版本）来访问 Microsoft SQL Server 2019 中的[敏感度数据分类元数据](../../relational-databases/security/sql-data-discovery-and-classification.md)。 

默认情况下，选项 `DataClassification` 为 `false`，但设置为 `true` 后，将使用敏感度数据分类元数据（如果存在）填充 `sqlsrv_field_metadata` 返回的数组。 

以患者表为例：

```
CREATE TABLE Patients 
      [PatientId] int identity,
      [SSN] char(11),
      [FirstName] nvarchar(50),
      [LastName] nvarchar(50),
      [BirthDate] date)
```

我们可以将 SSN 和 BirthDate 列分类，如下所示：

```
ADD SENSITIVITY CLASSIFICATION TO [Patients].SSN WITH (LABEL = 'Highly Confidential - secure privacy', INFORMATION_TYPE = 'Credentials')
ADD SENSITIVITY CLASSIFICATION TO [Patients].BirthDate WITH (LABEL = 'Confidential Personal Data', INFORMATION_TYPE = 'Birthdays')
```

若要访问元数据，请调用 `sqlsrv_field_metadata`，如以下代码片段所示：

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_prepare($conn, $tsql, array(), array('DataClassification' => true));
if (sqlsrv_execute($stmt)) {
    $fieldmeta = sqlsrv_field_metadata($stmt);

    foreach ($fieldmeta as $f) {
        if (count($f['Data Classification']) > 0) {
            echo $f['Name'] . ": \n";
            print_r($f['Data Classification']); 
        }
    }
}
```

输出将为：

```
SSN: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Highly Confidential - secure privacy
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Credentials
                    [id] => 
                )

        )

)
BirthDate: 
Array
(
    [0] => Array
        (
            [Label] => Array
                (
                    [name] => Confidential Personal Data
                    [id] => 
                )

            [Information Type] => Array
                (
                    [name] => Birthdays
                    [id] => 
                )

        )

)
```

如果使用 `sqlsrv_query` 而不是 `sqlsrv_prepare`，则可以修改上述代码片段，如下所示：

```
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = sqlsrv_query($conn, $tsql, array(), array('DataClassification' => true));
$fieldmeta = sqlsrv_field_metadata($stmt);

foreach ($fieldmeta as $f) {
    $jstr = json_encode($f);
    echo $jstr . PHP_EOL;
}
```

正如下面的 JSON 表示形式所示，如果数据分类元数据与列相关联，则会显示它：

```
{"Name":"PatientId","Type":4,"Size":null,"Precision":10,"Scale":null,"Nullable":0,"Data Classification":[]}
{"Name":"SSN","Type":1,"Size":11,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]}
{"Name":"FirstName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"LastName","Type":-9,"Size":50,"Precision":null,"Scale":null,"Nullable":1,"Data Classification":[]}
{"Name":"BirthDate","Type":91,"Size":null,"Precision":10,"Scale":0,"Nullable":1,"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]}
```

## <a name="sensitivity-rank-using-a-predefined-set-of-values"></a>使用一组预定义值的敏感度级别

从 5.9.0 开始，PHP 驱动程序在使用 ODBC 驱动程序 17.4.2 或更高版本时添加了分类级别检索。 使用 [ADD SENSITIVITY CLASSIFICATION](/sql/t-sql/statements/add-sensitivity-classification-transact-sql) 对任何数据列进行分类时，用户可以定义级别。 

例如，如果用户分别将 `NONE` 和 `LOW` 分配给 BirthDate 和 SSN，则 JSON 表示形式如下所示：

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":0},"rank":0}
{"0":{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""},"rank":10},"rank":10}
```

如[敏感度分类](/sql/relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql)中所示，级别的数值为：

```
0 for NONE
10 for LOW
20 for MEDIUM
30 for HIGH
40 for CRITICAL
```

因此，如果用户在对 BirthDate 列进行分类时定义了 `RANK=CRITICAL` 而不是 `RANK=NONE`，则分类元数据将为：

```
  array(7) {
    ["Name"]=>
    string(9) "BirthDate"
    ["Type"]=>
    int(91)
    ["Size"]=>
    NULL
    ["Precision"]=>
    int(10)
    ["Scale"]=>
    int(0)
    ["Nullable"]=>
    int(1)
    ["Data Classification"]=>
    array(2) {
      [0]=>
      array(3) {
        ["Label"]=>
        array(2) {
          ["name"]=>
          string(26) "Confidential Personal Data"
          ["id"]=>
          string(0) ""
        }
        ["Information Type"]=>
        array(2) {
          ["name"]=>
          string(9) "Birthdays"
          ["id"]=>
          string(0) ""
        }
        ["rank"]=>
        int(40)
      }
      ["rank"]=>
      int(40)
    }
  }
```

更新后的 JSON 表示形式如下所示：

```
{"0":{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""},"rank":40},"rank":40}
```

## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)  

[常量（Microsoft Drivers for PHP for SQL Server）](../../connect/php/constants-microsoft-drivers-for-php-for-sql-server.md)  

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  
