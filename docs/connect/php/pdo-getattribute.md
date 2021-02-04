---
title: PDO::getAttribute
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::getAttribute 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: c81833ea-8b8a-459d-8f24-920098da994d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ba4e6b81216933ea4367bb4b37feeb61dd646c88
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187617"
---
# <a name="pdogetattribute"></a>PDO::getAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

检索预定义 PDO 或驱动程序属性的值。  
  
## <a name="syntax"></a>语法  
  
```  
  
mixed PDO::getAttribute ( $attribute )  
```  
  
#### <a name="parameters"></a>参数  
*$attribute*：受支持的属性之一。 请参阅受支持的属性列表的“备注”部分。  
  
## <a name="return-value"></a>返回值  
如果成功，返回连接选项的值、预定义 PDO 属性或自定义驱动程序属性。 如果失败，返回 null。  
  
## <a name="remarks"></a>注解  
下表包含受支持的属性列表。  
  
|属性|由以下值处理|支持的值|说明|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|指定列名称是否应使用特定格式。 PDO::CASE_LOWER 强制使用小写列名称，PDO::CASE_NATURAL 保留数据库返回的列名称，而PDO::CASE_UPPER 强制使用大写列名称。<br /><br />默认值为 PDO::CASE_NATURAL。<br /><br />还可以使用 PDO::setAttribute 设置此属性。|  
|PDO::ATTR_CLIENT_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|字符串数组|介绍驱动程序和相关库的版本。 返回带有以下元素的数组：ODBC 版本 (*MajorVer*.*MinorVer*)、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client DLL 名称和版本、[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 (*MajorVer*.*MinorVer*.*BuildNumber*.*Revision*)|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|如果未设置为 PDO::PARAM_STR_CHAR，则返回 PDO::PARAM_STR_NATL。|
|PDO::ATTR_DRIVER_NAME|PDO|String|始终返回“sqlsrv”。|  
|PDO::ATTR_DRIVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|指示 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] 版本 (MajorVer.MinorVer.BuildNumber.Revision)   |  
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|指定驱动程序应如何处理失败。<br /><br />PDO::ERRMODE_SILENT（默认值）设置错误代码和信息。<br /><br />PDO::ERRMODE_WARNING 引发 E_WARNING。<br /><br />PDO::ERRMODE_EXCEPTION 引发异常。<br /><br />还可以使用 PDO::setAttribute 设置此属性。|  
|PDO::ATTR_ORACLE_NULLS|PDO|请参阅 PDO 文档。|请参阅 PDO 文档。|  
|PDO::ATTR_SERVER_INFO|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|3 个元素的数组|返回当前数据库、SQL Server 版本和 SQL Server 实例。|  
|PDO::ATTR_SERVER_VERSION|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|String|指示 SQL Server 版本 (Major.Minor.BuildNumber)  |  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|请参阅 PDO 文档|请参阅 PDO 文档。|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 到 PHP 内存限制。|配置保留客户端游标的结果集的缓冲区大小。<br /><br />默认值为 10240 KB (10 MB)。<br /><br />有关客户端游标的详细信息，请参阅[游标类型（SQLSRV 驱动程序）](../../connect/php/cursor-types-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|true<br /><br />false|指定直接或已准备的查询执行。 有关详细信息，请参阅 [PDO_SQLSRV 驱动程序中的直接语句执行和预定语句执行](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM|指定驱动程序用于与服务器通信的字符集编码。<br /><br />默认值为 PDO::SQLSRV_ENCODING_UTF8。|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True 或 False|处理带有数值 SQL 类型（bit、integer、smallint、tinyint、float 或 real）的列的数值提取。<br /><br />如果连接选项标志 ATTR_STRINGIFY_FETCHES 已启用，即使 SQLSRV_ATTR_FETCHES_NUMERIC_TYPE 已启用，返回值也是字符串。<br /><br />如果绑定列中返回的 PDO 类型是 PDO_PARAM_INT，即使 SQLSRV_ATTR_FETCHES_NUMERIC_TYPE 处于关闭状态，整数列的返回值也是 int。|  
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|整型|设置查询超时（以秒为单位）。<br /><br />默认值为 0，这意味着该驱动程序将无限期地等待结果。<br /><br />不允许使用负数。|  

  
PDO 将处理某些预定义的属性，同时它需要驱动程序处理其他属性。 所有自定义属性和连接选项都由驱动程序处理，不受支持的属性或连接选项将返回 null。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
此示例显示在更改其值之前和之后的 PDO::ATTR_ERRMODE 属性的值。  
  
```  
<?php  
$database = "AdventureWorks";  
$conn = new PDO( "sqlsrv:server=(local) ; Database = $database", "", "");  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
$conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );  
  
$attributes1 = array( "ERRMODE" );  
foreach ( $attributes1 as $val ) {  
     echo "PDO::ATTR_$val: ";  
     var_dump ($conn->getAttribute( constant( "PDO::ATTR_$val" ) ));  
}  
  
// An example using PDO::ATTR_CLIENT_VERSION  
print_r($conn->getAttribute( PDO::ATTR_CLIENT_VERSION ));  
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
