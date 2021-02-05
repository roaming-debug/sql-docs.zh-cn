---
title: PDO::setAttribute
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中 PDO::setAttribute 函数的 API 参考。
ms.custom: ''
ms.date: 08/10/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 56f9ee96-e1d2-46cc-b137-38f06a251863
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ac1b72ea564df077ae7edb0f771becd9b76cda7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195242"
---
# <a name="pdosetattribute"></a>PDO::setAttribute
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

设置预定义的 PDO 属性或自定义驱动程序属性。  
  
## <a name="syntax"></a>语法  
  
```
bool PDO::setAttribute ( $attribute, $value );  
```  
  
#### <a name="parameters"></a>参数  
*$attribute*：要设置的属性。 请参阅受支持的属性列表的“备注”部分。  
  
$value：值（混合类型）。  
  
## <a name="return-value"></a>返回值  
如果成功，将返回 True；否则返回 False。  
  
## <a name="remarks"></a>注解  
  
|属性|由以下值处理|支持的值|说明|  
|-------------|----------------|--------------------|---------------|  
|PDO::ATTR_CASE|PDO|PDO::CASE_LOWER<br /><br />PDO::CASE_NATURAL<br /><br />PDO::CASE_UPPER|指定列名称的大小写。<br /><br />PDO::CASE_LOWER 会使列名称小写。<br /><br />PDO::CASE_NATURA（默认值）显示数据库返回的列名称。<br /><br />PDO::CASE_UPPER 会使列名称大写。<br /><br />可以使用 PDO::setAttribute 设置此属性。|  
|PDO::ATTR_DEFAULT_FETCH_MODE|PDO|请参阅 PDO 文档。|请参阅 PDO 文档。|  
|PDO::ATTR_DEFAULT_STR_PARAM|PDO|PDO::PARAM_STR_CHAR<br /><br />PDO::PARAM_STR_NATL|有关更多信息，请参阅 [PDO::quote](../../connect/php/pdo-quote.md) 中的示例。|
|PDO::ATTR_ERRMODE|PDO|PDO::ERRMODE_SILENT<br /><br />PDO::ERRMODE_WARNING<br /><br />PDO::ERRMODE_EXCEPTION|指定驱动程序报告失败的方式。<br /><br />PDO::ERRMODE_SILENT（默认值）设置错误代码和信息。<br /><br />PDO::ERRMODE_WARNING 引发 E_WARNING。<br /><br />PDO::ERRMODE_EXCEPTION 会引发异常。<br /><br />可以使用 PDO::setAttribute 设置此属性。|  
|PDO::ATTR_ORACLE_NULLS|PDO|请参阅 PDO 文档。|指定应如何返回 NULL。<br /><br />PDO::NULL_NATURAL 不执行任何转换。<br /><br />PDO::NULL_EMPTY_STRING 将空字符串转换为 NULL。<br /><br />PDO::NULL_TO_STRING 将 NULL 转换为空字符串。|  
|PDO::ATTR_STATEMENT_CLASS|PDO|请参阅 PDO 文档。|设置派生自 PDOStatement 的用户提供的语句类。<br /><br />需要 `array(string classname, array(mixed constructor_args))`。<br /><br />有关详细详细，请参阅 PDO 文档。|  
|PDO::ATTR_STRINGIFY_FETCHES|PDO|True 或 False|检索数据时，将数值转换为字符串。|  
|PDO::SQLSRV_ATTR_CLIENT_BUFFER_MAX_KB_SIZE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|1 到 PHP 内存限制。|设置使用客户端游标时保留的结果集的缓冲区大小。<br /><br />如果未在 php.ini 文件中指定，则默认值为 10240 KB。<br /><br />不允许使用零和负数。<br /><br />有关创建客户端游标的查询的详细信息，请参阅[游标类型（PDO_SQLSRV 驱动程序）](../../connect/php/cursor-types-pdo-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_DECIMAL_PLACES|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|介于 0 和 4 之间（含 0 和 4）的整数|指定设置提取的 Money 值格式时的小数位数。<br /><br />将忽略任何负整数或大于 4 的值。<br /><br />此选项仅在 PDO::SQLSRV_ATTR_FORMAT_DECIMALS 为 true 时适用。<br /><br />还可以在语句级别设置此选项。 如果是这样，语句级别选项将覆盖此选项。<br /><br />有关详细信息，请参阅[设置十进制字符串和 Money 值格式（PDO_SQLSRV 驱动程序）](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)。|
|PDO::SQLSRV_ATTR_DIRECT_QUERY|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True 或 False|指定直接或已准备的查询执行。 有关详细信息，请参阅 [PDO_SQLSRV 驱动程序中的直接语句执行和预定语句执行](../../connect/php/direct-statement-execution-prepared-statement-execution-pdo-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_ENCODING|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|PDO::SQLSRV_ENCODING_UTF8<br /><br />PDO::SQLSRV_ENCODING_SYSTEM。|设置驱动程序用于与服务器通信的字符集编码。<br /><br />不支持 PDO::SQLSRV_ENCODING_BINARY。<br /><br />默认值为 PDO::SQLSRV_ENCODING_UTF8。|  
|PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True 或 False|指定是否以 [PHP DateTime](http://php.net/manual/en/class.datetime.php) 对象形式检索日期和时间类型。 如果保留 false，则默认行为是将它们作为字符串返回。<br /><br />还可以在语句级别设置此选项。 如果是这样，语句级别选项将覆盖此选项。<br /><br />有关详细信息，请参阅[操作说明：使用 PDO_SQLSRV 驱动程序检索日期和时间类型作为 PHP Datetime 对象](../../connect/php/how-to-retrieve-datetime-objects-using-pdo-sqlsrv-driver.md)。|  
|PDO::SQLSRV_ATTR_FETCHES_NUMERIC_TYPE|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True 或 False|处理带有数值 SQL 类型（bit、integer、smallint、tinyint、float 或 real）的列的数值提取。<br /><br />当打开了连接选项标志 ATTR_STRINGIFY_FETCHES，返回值将是一个字符串，即使 SQLSRV_ATTR_FETCHES_NUMERIC_TYPE 处于打开状态。<br /><br />如果绑定列中返回的 PDO 类型是 PDO_PARAM_INT，即使 SQLSRV_ATTR_FETCHES_NUMERIC_TYPE 处于关闭状态，整数列的返回值也是 int。|  
|PDO::SQLSRV_ATTR_FORMAT_DECIMALS|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|True 或 False|指定是否在合适时向十进制字符串添加前导零。 如已设置，此选项将启用用于设置 Money 类型格式的 PDO::SQLSRV_ATTR_DECIMAL_PLACES 选项。 如果保留 false，使用的默认行为是返回精确的精度，并为小于 1 的值省略前导零。<br /><br />还可以在语句级别设置此选项。 如果是这样，语句级别选项将覆盖此选项。<br /><br />有关详细信息，请参阅[设置十进制字符串和 Money 值格式（PDO_SQLSRV 驱动程序）](../../connect/php/formatting-decimals-pdo-sqlsrv-driver.md)。| 
|PDO::SQLSRV_ATTR_QUERY_TIMEOUT|[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]|整型|设置查询超时（以秒为单位）。<br /><br />默认值为 0，这意味着该驱动程序将无限期地等待结果。<br /><br />不允许使用负数。|  
  
PDO 将处理某些预定义的属性，并且需要驱动程序处理其他属性。 由驱动程序处理所有自定义属性和连接选项。 将根据 PDO::ATTR_ERRMODE 的设置报告不受支持的属性、连接选项或不受支持的值。  
  
已在 [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]的版本 2.0 中添加了对 PDO 的支持。  
  
## <a name="example"></a>示例  
此示例演示如何设置 PDO::ATTR_ERRMODE 属性。  
  
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
?>  
```  
  
## <a name="see-also"></a>另请参阅  
[PDO 类](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
