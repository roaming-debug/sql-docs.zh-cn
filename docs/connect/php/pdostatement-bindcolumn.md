---
title: PDOStatement::bindColumn
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDOStatement::bindColumn 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: bbdcea53-d23d-4769-89a0-95c7cf4d5390
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d9ed841aaf609178ee28b6340206421fa6d0eb81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195231"
---
# <a name="pdostatementbindcolumn"></a>PDOStatement::bindColumn
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

将变量绑定到结果集中的列。  
  
## <a name="syntax"></a>语法  
  
```  
  
bool PDOStatement::bindColumn($column, &$param[, $type[, $maxLen[, $driverdata ]]] );  
```  
  
#### <a name="parameters"></a>参数  
$column：列（从 1 开始的索引）的（混合）数量或结果集中的列的名称。  
  
&$param：列将绑定到的 PHP 变量的（混合）名称。  
  
$type：参数的可选数据类型，由 PDO::PARAM_* 常量表示。  
  
$maxLen：可选整数，不由 Microsoft Drivers for PHP for SQL Server 使用。  
  
$driverdata：适用于驱动程序的可选混合参数。 例如，你可以指定 PDO::SQLSRV_ENCODING_UTF8 来将列作为使用 UTF-8 编码的字符串绑定到变量。  
  
## <a name="return-value"></a>返回值  
如果成功，则为 TRUE；否则为 FALSE。  
  
## <a name="remarks"></a>注解  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
此示例显示变量可如何绑定到结果集中的列。  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "SELECT Title, FirstName, EmailAddress FROM Person.Contact where LastName = 'Estes'";  
$stmt = $conn->prepare($query);  
$stmt->execute();  
  
$stmt->bindColumn('EmailAddress', $email);  
while ( $row = $stmt->fetch( PDO::FETCH_BOUND ) ){  
   echo "$email\n";  
}  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDOStatement 类](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
