---
title: PDO::errorCode
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::errorCode 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 5864b1d8-6814-41cd-a88d-415124484c13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed2a580a972375a14c5ed6532e40eb7db43373a5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202039"
---
# <a name="pdoerrorcode"></a>PDO::errorCode
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

PDO::errorCode 检索数据库句柄上最近操作的 SQLSTATE。  
  
## <a name="syntax"></a>语法  
  
```  
  
mixed PDO::errorCode();  
```  
  
## <a name="return-value"></a>返回值  
如果数据库句柄上没有任何操作，PDO::errorCode 将以字符串或 NULL 形式返回五个字符的 SQLSTATE。  
  
## <a name="remarks"></a>注解  
PDO_SQLSRV 驱动程序中的 PDO::errorCode 会针对某些成功操作返回警告。 例如，成功连接时，PDO::errorCode 会返回“01000”，该值指示 SQL_SUCCESS_WITH_INFO。  
  
PDO::errorCode 仅检索直接在数据库连接上执行的操作的错误代码。 如果通过 PDO::prepare 或 PDO::query 创建 PDOStatement 实例，并生成与语句对象有关的错误，PDO::errorCode 不会检索该错误。 必须调用 PDOStatement::errorCode 才可以返回在特定语句对象上执行的操作的错误代码。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
在此示例中，列名拼写不正确（是 `Cityx` 而不是 `City`）导致了错误，随后报告了该错误。  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
