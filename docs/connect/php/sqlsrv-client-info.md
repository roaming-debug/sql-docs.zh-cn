---
title: sqlsrv_client_info
description: Microsoft SQLSRV Driver for PHP for SQL Server 中 sqlsrv_client_info 函数的 API 参考。
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- sqlsrv_client_info
apitype: NA
helpviewer_keywords:
- API Reference, sqlsrv_client_info
- sqlsrv_client_info
ms.assetid: 3e2d3679-436a-45d8-8bdc-7c633b65a720
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 041df13402ce55106474d3b8595df12dcc576c52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190725"
---
# <a name="sqlsrv_client_info"></a>sqlsrv_client_info
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

返回有关连接和客户端堆栈的信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
sqlsrv_client_info( resource $conn)  
```  
  
#### <a name="parameters"></a>参数  
*$conn*：连接客户端时所依据的连接资源。  
  
## <a name="return-value"></a>返回值  
下表中所述的带有密钥的关联阵列，或者如果连接资源为 NULL，则为 **false** 。  
  
**对于适用于 SQL Server 版本 3.2 和 3.1 的 PHP**：  
  
|键|说明|  
|-------|---------------|  
|DriverDllName|MSODBCSQL11.DLL (ODBC Driver 11 for SQL Server)|  
|DriverODBCVer|ODBC 版本 (xx.yy)|  
|DriverVer|ODBC Driver 11 for SQL Server DLL 版本：<br /><br />xx.yy.zzzz（[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 3.2 或 3.1）|  
|ExtensionVer|php_sqlsrv.dll 版本：<br /><br />3.2.xxxx.x（适用于 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 3.2）<br /><br />3.1.xxxx.x（适用于 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 3.1）|  
  
**对于适用于 SQL Server 版本 3.0 和 2.0 的 PHP**：  
  
|键|说明|  
|-------|---------------|  
|DriverDllName|SQLNCLI10.DLL（[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 2.0）|  
|DriverODBCVer|ODBC 版本 (xx.yy)|  
|DriverVer|SQL Server Native Client DLL 版本：<br /><br />10.50.xxx（[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 2.0）|  
|ExtensionVer|php_sqlsrv.dll 版本：<br /><br />2.0.xxxx.x（[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 2.0）|  
  
## <a name="example"></a>示例  
当从命令行运行以下示例时，该示例会将客户端信息写入控制台。 该示例假定已在本地计算机上安装了 SQL Server。 从命令行运行该示例时，所有输出都将写入控制台。  
  
```  
<?php  
/*Connect to the local server using Windows Authentication and   
specify the AdventureWorks database as the database in use. */  
$serverName = "(local)";  
$conn = sqlsrv_connect( $serverName);  
  
if( $conn === false )  
{  
     echo "Could not connect.\n";  
     die( print_r( sqlsrv_errors(), true));  
}  
  
if( $client_info = sqlsrv_client_info( $conn))  
{  
       foreach( $client_info as $key => $value)  
      {  
              echo $key.": ".$value."\n";  
      }  
}  
else  
{  
       echo "Client info error.\n";  
}  
  
/* Close connection resources. */  
sqlsrv_close( $conn);  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[SQLSRV 驱动程序 API 参考](../../connect/php/sqlsrv-driver-api-reference.md)

[文档中相关的代码示例](../../connect/php/about-code-examples-in-the-documentation.md)  
  
