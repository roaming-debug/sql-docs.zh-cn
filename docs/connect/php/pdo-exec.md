---
title: PDO::exec
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::exec 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 359a87c6-c13a-4518-8f23-a922e7f3b171
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ad79792d21e9b097f6aee681b1097c8aacfa44fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202015"
---
# <a name="pdoexec"></a>PDO::exec
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

在单个函数调用中准备和执行 SQL 语句，从而返回受该语句影响的行数。  
  
## <a name="syntax"></a>语法  
  
```  
  
int PDO::exec ($statement)  
```  
  
#### <a name="parameters"></a>参数  
*$statement*：包含要执行的 SQL 语句的字符串。  
  
## <a name="return-value"></a>返回值  
报告受影响行数的整数。  
  
## <a name="remarks"></a>注解  
如果 *$statement* 包含多个 SQL 语句，则仅为最后一个语句报告受影响行的计数。  
  
PDO::exec 不为 SELECT 语句返回结果。  
  
以下属性会影响 PDO::exec 的行为：  
  
-   PDO::ATTR_DEFAULT_FETCH_MODE  
  
-   PDO::SQLSRV_ATTR_ENCODING  
  
-   PDO::SQLSRV_ATTR_QUERY_TIMEOUT  
  
有关详细信息，请参阅 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)。 
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
此示例删除表 1 中的行，该行在第 1 列中具有“xxxyy”。 然后，该示例报告已删除的行数。  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec("use Test");  
   $ret = $c->exec("delete from Table1 where col1 = 'xxxyy'");  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
