---
title: PDO::beginTransaction
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::beginTransaction 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 4d5db438-9df7-4d22-9907-3ddc63bd2220
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2aff13f864bd22ab9cc2407e7a990c6114a6e108
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202087"
---
# <a name="pdobegintransaction"></a>PDO::beginTransaction
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

关闭自动提交模式，然后开始某个事务。  
  
## <a name="syntax"></a>语法  
  
```  
  
bool PDO::beginTransaction();  
```  
  
## <a name="return-value"></a>返回值  
如果方法调用成功，则为 True；否则为 False。  
  
## <a name="remarks"></a>注解  
调用 [PDO::commit](../../connect/php/pdo-commit.md) 或 [PDO::rollback](../../connect/php/pdo-rollback.md) 时，将结束使用 PDO::beginTransaction 开始的事务。  
  
PDO::beginTransaction 不受 PDO::ATTR_AUTOCOMMIT 的值影响（也不影响该值）。  
  
除非先使用 PDO::rollback 或 PDO::commit 结束之前的 PDO::beginTransaction，否则不允许调用 PDO::beginTransaction。  
  
如果此方法失败，该连接会返回到自动提交模式。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
以下示例使用名为 Test 的数据库和名为 Table1 的表。 它将启动某个事务，然后发出命令，以便添加两行并随后删除一行。 将这些命令发送到该数据库，并且使用 `PDO::commit`显式结束该事务。  
  
```  
<?php  
   $conn = new PDO( "sqlsrv:server=(local); Database = Test", "", "");  
   $conn->beginTransaction();  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'b') ");  
   $ret = $conn->exec("insert into Table1(col1, col2) values('a', 'c') ");  
   $ret = $conn->exec("delete from Table1 where col1 = 'a' and col2 = 'b'");  
   $conn->commit();  
   // $conn->rollback();  
   echo $ret;  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
