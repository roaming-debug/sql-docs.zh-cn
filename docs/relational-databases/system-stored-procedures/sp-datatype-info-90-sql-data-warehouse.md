---
description: " (Azure Synapse Analytics sp_datatype_info_90) "
title: " (Azure Synapse Analytics sp_datatype_info_90) "
ms.custom: ''
ms.date: 03/13/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 1d043964-dc6e-4c3e-ab61-bc444d5e25ae
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 8a859571ef9f4682c4c8556038247dc21d47eb50
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474608"
---
# <a name="sp_datatype_info_90-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_datatype_info_90) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  返回有关当前环境所支持的数据类型的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_datatype_info_90 [ [ @data_type = ] data_type ]   
     [ , [ @ODBCVer = ] odbc_version ]   
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>自变量  
`[ @data_type = ] data_type` 指定的数据类型的代码号。 若要获得所有数据类型的列表，请省略此参数。 *data_type* 的值为 **int**，默认值为0。  
  
`[ @ODBCVer = ] odbc_version` 使用的 ODBC 版本。 *odbc_version* 为 **tinyint**，默认值为2。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|TYPE_NAME|**sysname**|与 DBMS 相关的数据类型。|  
|DATA_TYPE|**smallint**|此类型的所有列所映射到的 ODBC 类型代码。|  
|PRECISION|**int**|数据源中数据类型的最大精度。 数据类型的精度不适用时返回 NULL。 PRECISION 列的返回值以 10 为基数。|  
|LITERAL_PREFIX|**varchar (** 32 **)**|常量前使用的一个或多个字符。 例如，单引号 (**'**) 用于字符类型，0x 用于二进制。|  
|LITERAL_SUFFIX|**varchar (** 32 **)**|用于终止常数的一个或多个字符。 例如，单引号 (**"**) 用于字符类型，而不使用引号作为二进制。|  
|CREATE_PARAMS|**varchar (** 32 **)**|此数据类型的创建参数的说明。 例如， **decimal** 为 "精度，小数位数"， **float** 为 NULL， **varchar** 为 "max_length"。|  
|NULLABLE|**smallint**|指定为 Null 性。<br /><br /> 1 = 允许 Null 值。<br /><br /> 0 = 不允许 Null 值。|  
|CASE_SENSITIVE|**smallint**|指定是否区分大小写。<br /><br /> 1 = 此类型的所有列都区分大小写（用于排序规则）。<br /><br /> 0 = 此类型的所有列都不区分大小写。|  
|可搜索|**smallint**|指定列类型的搜索能力：<br /><br /> 1 = 不能搜索。<br /><br /> 2 = 可以使用 LIKE 进行搜索。<br /><br /> 3 = 可以使用 WHERE 进行搜索。<br /><br /> 4 = 可以使用 WHERE 或 LIKE 进行搜索。|  
|UNSIGNED_ATTRIBUTE|**smallint**|指定数据类型的符号。<br /><br /> 1 = 数据类型没有符号。<br /><br /> 0 = 数据类型有符号。|  
|MONEY|**smallint**|指定 **money** 数据类型。<br /><br /> 1 = **money** 数据类型。<br /><br /> 0 = 不是 **money** 数据类型。|  
|AUTO_INCREMENT|**smallint**|指定自动递增。<br /><br /> 1 = 自动递增。<br /><br /> 0 = 不自动递增。<br /><br /> NULL = 属性不适用。<br /><br /> 应用程序可以将值插入具有此属性的列中，但应用程序不能更新列中的值。 除了 **bit** 数据类型之外，AUTO_INCREMENT 仅对属于精确数字和近似数字数据类型类别的数据类型有效。|  
|LOCAL_TYPE_NAME|**sysname**|与数据源相关的数据类型名称的本地化版本。 例如，DECIMAL 的法语版本是 DECIMALE。 如果是数据源不支持的本地化名称，则返回 NULL。|  
|MINIMUM_SCALE|**smallint**|数据源中数据类型的最小小数位数。 如果数据类型的小数位数是固定的，则 MINIMUM_SCALE 和 MAXIMUM_SCALE 列将同时包含此值。 当小数位数不适用时，将返回 NULL。|  
|MAXIMUM_SCALE|**smallint**|数据源中数据类型的最大小数位数。 如果在数据源中没有单独定义最大小数位数，而是将其定义为与最大精度相同，则此列的值与 PRECISION 列的值相同。|  
|SQL_DATA_TYPE|**smallint**|SQL 数据类型在描述符的 TYPE 字段中显示的值。 此列与 DATA_TYPE 列相同， **datetime** 和 ANSI **interval** 数据类型除外。 此字段始终返回值。|  
|SQL_DATETIME_SUB|**smallint**|**datetime** 或 ANSI **interval** 子代码（如果 SQL_DATA_TYPE 的值 SQL_DATETIME 或 SQL_INTERVAL。 对于 **日期时间** 和 ANSI **间隔** 以外的数据类型，此字段为 NULL。|  
|NUM_PREC_RADIX|**int**|计算列可容纳的最大数目的位数或位数。 如果数据类型是近似数字数据类型，则此列包含的值为 2，以指示几个位。 对于精确数字类型，此列包含的值为 10，以指示几个十进制数字。 否则，此列为 NULL。 通过将精度和基数相结合，应用程序可以计算出列所能容纳的最大数。|  
|INTERVAL_PRECISION|**smallint**|如果 *data_type* 为 **interval**，则为间隔前导精度值;否则为 NULL。|  
|USERTYPE|**smallint**|systypes 表中的 **usertype** 值。|  
  
## <a name="remarks"></a>备注  
 sp_datatype_info 等效于 ODBC 中的 SQLGetTypeInfo。 返回的结果按 DATA_TYPE 排序，再按数据类型映射到相应 ODBC SQL 数据类型的紧密程度进行排序。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 下面的示例通过指定的 *data_type* 值来检索 **sysname** 和 **nvarchar** 数据类型的信息 `-9` 。  
  
```sql  
USE master;  
GO  
EXEC sp_datatype_info_90 -9;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse 分析存储过程](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)  
  
  

