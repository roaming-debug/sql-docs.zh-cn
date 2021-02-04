---
title: PDO::__construct
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::__construct 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 3ee53aff-6fe4-44cd-a15b-51770c98c712
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 27155dbbae9967895412628b4cf82da381c97f8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202056"
---
# <a name="pdo__construct"></a>PDO::__construct
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

创建与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的连接。  
  
## <a name="syntax"></a>语法  
  
```  
  
PDO::__construct($dsn [,$username [,$password [,$driver_options ]]] )  
```  
  
#### <a name="parameters"></a>参数  
*$dsn*：包含前缀名（始终是 `sqlsrv`）、冒号和 Server 关键字的字符串。 例如，`"sqlsrv:server=(local)"`。 你可以选择指定其他连接关键字。 有关 Server 关键字和其他连接关键字的介绍，请参阅 [Connection Options](../../connect/php/connection-options.md) 。 因为整个 *$dsn* 包含在引号中，所以每个连接关键字不应单独引用。  
  
*$username*：可选。 包含用户名的字符串。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接，请指定登录 ID。 若要使用 Windows 身份验证进行连接，请指定 `""`。  
  
*$password*：可选。 包含用户密码的字符串。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证进行连接，请指定密码。 若要使用 Windows 身份验证进行连接，请指定 `""`。  
  
*$driver_options*：可选。 你可以指定 PDO 驱动程序管理器属性和 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 特定驱动程序属性 -- PDO::SQLSRV_ATTR_ENCODING、PDO::SQLSRV_ATTR_DIRECT_QUERY。 无效属性不会生成异常。 当使用 [PDO::setAttribute](../../connect/php/pdo-setattribute.md)指定时，无效属性会生成异常。  
  
## <a name="return-value"></a>返回值  
返回 PDO 对象。 如果失败，则返回 PDOException 对象。  
  
## <a name="exceptions"></a>例外  
PDOException  
  
## <a name="remarks"></a>注解  
你可以通过将实例设置为 NULL 来关闭连接对象。  
  
连接后，PDO::errorCode 将显示 01000 而非 00000。  
  
如果 PDO::__construct 出于任何原因而失败，将引发异常，即使将 PDO::ATTR_ERRMODE 设置为 PDO::ERRMODE_SILENT 也是如此。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example-with-database"></a>使用数据库的示例  
此示例显示如何使用 Windows 身份验证连接到服务器，以及如何指定数据库。  
  
```  
<?php  
   $c = new PDO( "sqlsrv:Server=(local) ; Database = AdventureWorks ", "", "", array(PDO::SQLSRV_ATTR_DIRECT_QUERY => true));   
  
   $query = 'SELECT * FROM Person.ContactType';   
   $stmt = $c->query( $query );   
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ) {   
      print_r( $row );   
   }  
   $c = null;   
?>  
```  
  
## <a name="example-without-database"></a>不使用数据库的示例  
此示例显示如何连接到服务器，从而指定更高版本的数据库。  
  
```  
<?php  
   $c = new PDO( "sqlsrv:server=(local)");  
  
   $c->exec( "USE AdventureWorks");  
   $query = 'SELECT * FROM Person.ContactType';  
   $stmt = $c->query( $query );  
   while ( $row = $stmt->fetch( PDO::FETCH_ASSOC ) ){  
      print_r( $row );  
   }  
   $c = null;  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
