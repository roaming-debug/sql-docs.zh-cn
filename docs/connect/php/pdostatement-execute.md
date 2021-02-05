---
title: PDOStatement::execute
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDOStatement::execute 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c2e80566-fa41-4918-8521-cf2e05374cbd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7910425a9abc5dfff0560a9c886e0b439af85d1b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206345"
---
# <a name="pdostatementexecute"></a>PDOStatement::execute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

执行语句。  
  
## <a name="syntax"></a>语法  
  
```  
  
bool PDOStatement::execute ([ $input ] );  
```  
  
#### <a name="parameters"></a>参数  
$input：（可选）包含参数标记的值的关联阵列。  
  
## <a name="return-value"></a>返回值  
如果成功，则为 True；否则为 False。  
  
## <a name="remarks"></a>注解  
使用 PDOStatement::execute 执行的语句必须先通过 [PDO::prepare](../../connect/php/pdo-prepare.md)准备就绪。 请参阅 [PDO_SQLSRV 驱动程序中的直接语句执行和已准备的语句执行](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md) 获取有关如何指定直接或已准备的语句执行的信息。  
  
输入参数数组的所有值将会视为 PDO::PARAM_STR 值。  
  
如果已准备的语句包括参数标记，你必须调用 PDOStatement::bindParam 来将 PHP 变量绑定到参数标记，或传递仅输入参数值的数组。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
  
```  
<?php  
$database = "AdventureWorks";  
$server = "(local)";  
$conn = new PDO( "sqlsrv:server=$server ; Database = $database", "", "");  
  
$query = "select * from Person.ContactType";  
$stmt = $conn->prepare( $query );  
$stmt->execute();  
  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print "$row[Name]\n";  
}  
  
echo "\n";  
$param = "Owner";  
$query = "select * from Person.ContactType where name = ?";  
$stmt = $conn->prepare( $query );  
$stmt->execute(array($param));  
  
while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
   print "$row[Name]\n";  
}  
?>  
```  
  
> [!NOTE]
> 当由于 PHP 的[浮点数](https://php.net/manual/en/language.types.float.php)具有有限精确度而将值绑定到[十进制或数值列](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)以确保精确度和准确度时，建议将字符串用作输入。 这同样适用于 bigint 列，尤其是在值超出[整数](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)范围的情况下。

## <a name="see-also"></a>另请参阅  
[PDOStatement 类](../../connect/php/pdostatement-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
