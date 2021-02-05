---
title: sqlsrv_connect
description: 创建连接资源，并使用适用于 PHP 的 sql_srv 驱动程序打开连接。 默认情况下，使用 Windows 身份验证尝试连接。
ms.custom: ''
ms.date: 03/26/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_connect
apitype: NA
helpviewer_keywords:
- connecting to the server
- API Reference, sqlsrv_connect
- connection pooling support
- sqlsrv_connect
ms.assetid: 37836b49-258e-45ce-9549-b8bd85d6952d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ea1fdf0513e8ba793425154020cb4dfc837a1045
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195076"
---
# <a name="sqlsrv_connect"></a>sqlsrv_connect
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

创建一个连接资源，并打开该连接。 默认情况下，使用 Windows 身份验证尝试连接。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_connect( string $serverName [, array $connectionInfo])  
```  
  
#### <a name="parameters"></a>参数  
*$serverName*：指定建立连接的服务器名称的字符串。 实例名称（例如“myServer\instanceName”）或端口号（例如“myServer, 1521”）可以包括为此字符串的一部分。 有关可用于此参数的选项的完整介绍，请参阅[结合使用连接字符串关键字与 SQL Native Client](../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md) 的 ODBC 驱动程序连接字符串关键字部分中的 Server 关键字。  
  
从 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 3.0 开始，你还可以指定带有 `"(localdb)\instancename"`的 LocalDB 实例。 有关详细信息，请参阅[支持 LocalDB](php-driver-for-sql-server-support-for-localdb.md)。  
  
同样从 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 3.0 开始，你还可以指定要连接到 AlwaysOn 可用性组的虚拟网络名称。 有关对 [!INCLUDE[ssHADR](../../includes/sshadr_md.md)] 的 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 支持的详细信息，请参阅[对高可用性和灾难恢复的支持](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)。  
  
*$connectionInfo* [可选]：包含连接属性的关联数组（例如，array("Database" => "AdventureWorks")）   。 有关数组的受支持密钥的列表，请参阅 [Connection Options](connection-options.md) 。  
  
## <a name="return-value"></a>返回值  
PHP 连接资源。 如果无法成功创建和打开连接，则返回 **false** 。  
  
## <a name="remarks"></a>备注  
如果在可选的 *$connectionInfo* 参数中没有指定 *UID* 和 *PWD* 的值，将尝试使用 Windows 身份验证进行此连接。 有关连接到服务器的详细信息，请参阅[如何：使用 Windows 身份验证进行连接](how-to-connect-using-windows-authentication.md)和[如何：使用 SQL Server 身份验证进行连接](how-to-connect-using-sql-server-authentication.md)。  
  
## <a name="example"></a>示例  
以下示例使用 Windows 身份验证创建和打开连接。 该示例假定已在本地计算机上安装了 SQL Server 和 [AdventureWorks](https://www.codeplex.com/SqlServerSamples) 数据库。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```  
<?php  
/*  
Connect to the local server using Windows Authentication and specify  
the AdventureWorks database as the database in use. To connect using  
SQL Server Authentication, set values for the "UID" and "PWD"  
 attributes in the $connectionInfo parameter. For example:  
$connectionInfo = array("UID" => $uid, "PWD" => $pwd, "Database"=>"AdventureWorks");  
*/  
$serverName = "(local)";  
$connectionInfo = array( "Database"=>"AdventureWorks");  
$conn = sqlsrv_connect( $serverName, $connectionInfo);  
  
if( $conn )  
{  
     echo "Connection established.\n";  
}  
else  
{  
     echo "Connection could not be established.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
//-----------------------------------------------  
// Perform operations with connection.  
//-----------------------------------------------  
  
/* Close the connection. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](sqlsrv-driver-api-reference.md)

[连接到服务器](connecting-to-the-server.md)

[文档中相关的代码示例](about-code-examples-in-the-documentation.md)  
  
