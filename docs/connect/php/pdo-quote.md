---
title: PDO::quote
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::quote 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: ab9ddc48-42f8-4edf-aa8b-b0fc66706161
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9b9e74b05b48c4e3c225ec37a07408d2ca5c889b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206373"
---
# <a name="pdoquote"></a>PDO::quote
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

根据基础 SQL Server 数据库的需要，通过用引号将输入字符串括起来处理字符串，以供在查询中使用。 PDO::quote 将使用适用于 SQL Server 的引号样式对输入字符串内的特殊字符进行转义。  
  
## <a name="syntax"></a>语法  
  
```  
  
string PDO::quote( $string[, $parameter_type ] )  
```  
  
#### <a name="parameters"></a>参数  
$*string*：要用引号括起来的字符串。  
  
$parameter_type：指示数据类型的可选（整数）符号。  默认值为 PDO::PARAM_STR。  

PHP 7.2 中引入了新的 PDO 常量，以添加对[绑定 Unicode 和非 Unicode 字符串](https://wiki.php.net/rfc/extended-string-types-for-pdo)的支持。 Unicode 字符串可以括在引号中，并使用 N 作为前缀（即 N'string' 而不是 'string'）。

1. PDO::PARAM_STR_NATL - Unicode 字符串的新类型，作为按位 OR 应用到 PDO::PARAM_STR
1. PDO::PARAM_STR_CHAR - 非 Unicode 字符串的新类型，作为按位 OR 应用到 PDO::PARAM_STR
1. PDO::ATTR_DEFAULT_STR_PARAM - 设置为 PDO::PARAM_STR_NATL 或 PDO::PARAM_STR_CHAR，以指示默认情况下 PDO::PARAM_STR 的按位 OR 的值

从版本 5.8.0 开始，可以将这些常量和 PDO::quote 一起使用。
  
## <a name="return-value"></a>返回值  
可传递到 SQL 语句的用引号括起来的字符串，或 False（如果失败）。  
  
## <a name="remarks"></a>注解  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="string-escape-example"></a>字符串转义示例  
  
```  
<?php  
$database = "test";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$param = 'a \' g';  
$param2 = $conn->quote( $param );  
  
$query = "INSERT INTO Table1 VALUES( ?, '1' )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param));  
  
$query = "INSERT INTO Table1 VALUES( ?, ? )";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param, $param2));  
?>  
```  
  
## <a name="pdo-quote-example"></a>PDO 引用示例  

下面的脚本演示了一些示例，说明扩展字符串类型如何影响带有 PHP 7.2+ 的 PDO::quote()。

```
<?php
$database = "test";
$server = "(local)";
$db = new PDO("sqlsrv:server=$server; Database=$database", "", "");

$db->quote('über', PDO::PARAM_STR | PDO::PARAM_STR_NATL); // N'über'
$db->quote('foo'); // 'foo'

$db->setAttribute(PDO::ATTR_DEFAULT_STR_PARAM, PDO::PARAM_STR_NATL);
$db->quote('über'); // N'über'
$db->quote('foo', PDO::PARAM_STR | PDO::PARAM_STR_CHAR); // 'foo'
?>
```
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
