---
title: PDOStatement::getColumnMeta
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDOStatement::getColumnMeta 函数的 API 参考。
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c92a21cc-8e53-43d0-a4bf-542c77c100c9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8767da09f84be9c557238643e16c925756e0bede
ms.sourcegitcommit: c52a6aeb6fa6d7c3a86b3e84449361f4a0949ad0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2021
ms.locfileid: "99623777"
---
# <a name="pdostatementgetcolumnmeta"></a>PDOStatement::getColumnMeta
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

检索列的元数据。  
  
## <a name="syntax"></a>语法  
  
```  
  
array PDOStatement::getColumnMeta ( $column );  
```  
  
#### <a name="parameters"></a>参数  
$conn：（整数）要检索其元数据的列的从零开始的数。  
  
## <a name="return-value"></a>返回值  
包含列的元数据的关联阵列（键和值）。 有关数组中的字段的说明，请参阅“备注”部分。  
  
## <a name="remarks"></a>注解  
下表介绍 getColumnMeta 返回的数组中的字段。  
  
|名称|VALUES|  
|--------|----------|  
|native_type|指定列的 PHP 类型。 始终为字符串。|  
|driver:decl_type|指定用于表示数据库中的列值的 SQL 类型。 如果结果集中的列是函数的结果，则 PDOStatement::getColumnMeta 不会返回此值。|  
|flags|指定为此列设置的标志。 始终为 0。|  
|name|指定数据库中的列的名称。|  
|表|指定包含数据库中的列的表格名称。 始终为空白。|  
|len|指定列长度。|  
|精准率|指定此列的数值精度。|  
|pdo_type|指定此列的类型，由 PDO::PARAM_* 常量表示。 Always PDO::PARAM_STR (2)。|  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$stmt = $conn->query("select * from Person.ContactType");  
$metadata = $stmt->getColumnMeta(2);  
var_dump($metadata);  
  
print $metadata['sqlsrv:decl_type'] . "\n";  
print $metadata['native_type'] . "\n";  
print $metadata['name'];  
?>  
```  
  
## <a name="sensitivity-data-classification-metadata"></a>敏感度数据分类元数据

从版本 5.8.0 开始，用户可以使用新的语句属性 `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` 通过 `PDOStatement::getColumnMeta`（需要 Microsoft ODBC Driver 17.4.2 或更高版本）来访问 Microsoft SQL Server 2019 中的[敏感度数据分类元数据](../../relational-databases/security/sql-data-discovery-and-classification.md)。

请注意，默认情况下 `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` 属性为 `false`，但当设置为 `true` 时，将使用敏感度数据分类元数据（如果存在）填充上述数组字段 `flags`。 

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

若要访问元数据，请在将 `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` 设置为 true 后使用 `PDOStatement::getColumnMeta`，如以下代码片段所示：

```
$options = array(PDO::SQLSRV_ATTR_DATA_CLASSIFICATION => true);
$tableName = 'Patients';
$tsql = "SELECT * FROM $tableName";
$stmt = $conn->prepare($tsql, $options);
$stmt->execute();
$numCol = $stmt->columnCount();

for ($i = 0; $i < $numCol; $i++) {
    $metadata = $stmt->getColumnMeta($i);
    $jstr = json_encode($metadata);
    echo $jstr . PHP_EOL;
}
```

所有列的元数据输出如下：

```
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Highly Confidential - secure privacy","id":""},"Information Type":{"name":"Credentials","id":""}}]},"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":{"Data Classification":[]},"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":{"Data Classification":[{"Label":{"name":"Confidential Personal Data","id":""},"Information Type":{"name":"Birthdays","id":""}}]},"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
```

如果我们通过将 `PDO::SQLSRV_ATTR_DATA_CLASSIFICATION` 设置为 `false`（默认情况）来修改上述代码片段，`flags` 字段将始终为 `0`，如下所示：

```
{"flags":0,"sqlsrv:decl_type":"int identity","native_type":"string","table":"","pdo_type":2,"name":"PatientId","len":10,"precision":0}
{"flags":0,"sqlsrv:decl_type":"char","native_type":"string","table":"","pdo_type":2,"name":"SSN","len":11,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"FirstName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"nvarchar","native_type":"string","table":"","pdo_type":2,"name":"LastName","len":50,"precision":0}
{"flags":0,"sqlsrv:decl_type":"date","native_type":"string","table":"","pdo_type":2,"name":"BirthDate","len":10,"precision":0}
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
array(1) {
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
[PDOStatement 类](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
