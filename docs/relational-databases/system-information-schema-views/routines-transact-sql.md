---
description: ROUTINES (Transact-SQL)
title: Transact-sql)  (例程 |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINES_TSQL
- ROUTINES
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINES view
- INFORMATION_SCHEMA.ROUTINES view
ms.assetid: c75561b2-c9a1-48a1-9afa-a5896b6454cf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25ea15fd105733e04dd571dc925df394b1520d47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192084"
---
# <a name="routines-transact-sql"></a>ROUTINES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为当前数据库中可由当前用户访问的每个存储过程及函数返回一行。 描述返回值的列只适用于函数。 对于存储过程，这些列将为 NULL。  
  
 若要从这些视图中检索信息，请指定 INFORMATION_SCHEMA 的完全限定名称。*view_name*。  
  
> [!NOTE]  
>  ROUTINE_DEFINITION 列包含创建函数或存储过程的源语句。 这些源语句有可能包含嵌入式回车符。 如果将该列返回给某个以文本格式显示结果的应用程序，那么 ROUTINE_DEFINITION 结果中的嵌入式回车符可能会影响整个结果集的格式。 如果选择 ROUTINE_DEFINITION 列，那么必须对嵌入式回车符进行调整，例如，可将结果集返回到一个网格或者将 ROUTINE_DEFINITION 返回到其自己的文本框。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|SPECIFIC_CATALOG|**nvarchar (** 128 **)**|特定的目录名称。 该名称与 ROUTINE_CATALOG 相同。|  
|SPECIFIC_SCHEMA|**nvarchar (** 128 **)**|特定的架构名称。<br /><br /> **\* \* 重要 \* 说明 \*** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|SPECIFIC_NAME|**nvarchar (** 128 **)**|特定的目录名称。 该名称与 ROUTINE_NAME 相同。|  
|ROUTINE_CATALOG|**nvarchar (** 128 **)**|函数的目录名。|  
|ROUTINE_SCHEMA|**nvarchar (** 128 **)**|包含此函数的架构的名称。<br /><br /> **\* \* 重要 \* 说明 \*** 不要使用 INFORMATION_SCHEMA 视图来确定对象的架构。 INFORMATION_SCHEMA 视图仅表示对象的元数据的子集。 查找对象架构的唯一可靠方法是查询 sys.databases 目录视图。|  
|ROUTINE_NAME|**nvarchar (** 128 **)**|函数的名称。|  
|ROUTINE_TYPE|**nvarchar (** 20 **)**|为存储过程返回 PROCEDURE；为函数返回 FUNCTION。|  
|MODULE_CATALOG|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|MODULE_SCHEMA|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|MODULE_NAME|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|UDT_CATALOG|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|UDT_SCHEMA|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|UDT_NAME|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|DATA_TYPE|**nvarchar (** 128 **)**|函数返回值的数据类型。 如果表值函数，则返回 **table** 。|  
|CHARACTER_MAXIMUM_LENGTH|**int**|以字符为单位的最大长度（如果返回类型是字符类型）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。|  
|CHARACTER_OCTET_LENGTH|**int**|以字节为单位的最大长度（如果返回类型是字符类型）。<br /><br /> 对于 **xml** 和大值类型的数据，为-1。|  
|COLLATION_CATALOG|**nvarchar (** 128 **)**|始终返回 NULL。|  
|COLLATION_SCHEMA|**nvarchar (** 128 **)**|始终返回 NULL。|  
|COLLATION_NAME|**nvarchar (** 128 **)**|返回值的排序规则名。 对于非字符类型，返回 NULL。|  
|CHARACTER_SET_CATALOG|**nvarchar (** 128 **)**|始终返回 NULL。|  
|CHARACTER_SET_SCHEMA|**nvarchar (** 128 **)**|始终返回 NULL。|  
|CHARACTER_SET_NAME|**nvarchar (** 128 **)**|返回值字符集的名称。 对于非字符类型，返回 NULL。|  
|NUMERIC_PRECISION|**smallint**|返回值的数字精度。 对于非数字类型，返回 NULL。|  
|NUMERIC_PRECISION_RADIX|**smallint**|返回值的数字精度基数。 对于非数字类型，返回 NULL。|  
|NUMERIC_SCALE|**smallint**|返回值的小数位数。 对于非数字类型，返回 NULL。|  
|DATETIME_PRECISION|**smallint**|如果返回值的类型为 **datetime**，则为秒的小数精度。 否则，返回 NULL。|  
|INTERVAL_TYPE|**nvarchar (** 30 **)**|NULL。 留待将来使用。|  
|INTERVAL_PRECISION|**smallint**|NULL。 留待将来使用。|  
|TYPE_UDT_CATALOG|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|TYPE_UDT_SCHEMA|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|TYPE_UDT_NAME|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|SCOPE_CATALOG|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|SCOPE_SCHEMA|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|SCOPE_NAME|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|MAXIMUM_CARDINALITY|**bigint**|NULL。 留待将来使用。|  
|DTD_IDENTIFIER|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|ROUTINE_BODY|**nvarchar (** 30 **)**|对于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 函数，返回 SQL；对于外部编写的函数，返回 EXTERNAL。<br /><br /> 函数将始终是 SQL 函数。|  
|ROUTINE_DEFINITION|**nvarchar (** 4000 **)**|如果函数或存储过程未加密，返回函数或存储过程的定义文本最前面的 4000 字符。 否则，返回 NULL。<br /><br /> 若要确保获取完整的定义，请在[sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)目录视图中查询[OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md)函数或定义列。|  
|EXTERNAL_NAME|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|EXTERNAL_LANGUAGE|**nvarchar (** 30 **)**|NULL。 留待将来使用。|  
|PARAMETER_STYLE|**nvarchar (** 30 **)**|NULL。 留待将来使用。|  
|IS_DETERMINISTIC|**nvarchar (** 10 **)**|如果例程是确定性的，则返回 YES。<br /><br /> 如果例程是不确定性的，则返回 NO。<br /><br /> 对于存储过程，始终返回 NO。|  
|SQL_DATA_ACCESS|**nvarchar (** 30 **)**|返回以下值之一：<br /><br /> NONE = 函数不包含 SQL。<br /><br /> CONTAINS = 函数可能包含 SQL。<br /><br /> READS = 函数可能读取 SQL 数据。<br /><br /> MODIFIES = 函数可能修改 SQL 数据。<br /><br /> 为所有函数返回 READS，为所有存储过程返回 MODIFIES。|  
|IS_NULL_CALL|**nvarchar (** 10 **)**|指示如果例程的任一参数为 NULL，是否将调用该例程。|  
|SQL_PATH|**nvarchar (** 128 **)**|NULL。 留待将来使用。|  
|SCHEMA_LEVEL_ROUTINE|**nvarchar (** 10 **)**|如果是架构级函数，则返回 YES；如果不是架构级函数，则返回 NO。<br /><br /> 始终返回 YES。|  
|MAX_DYNAMIC_RESULT_SETS|**smallint**|例程返回的最大动态结果集数。<br /><br /> 如果是函数，则返回 0。|  
|IS_USER_DEFINED_CAST|**nvarchar (** 10 **)**|如果是用户定义的转换函数，则返回 YES；如果不是用户定义的转换函数，则返回 NO。<br /><br /> 始终返回 NO。|  
|IS_IMPLICITLY_INVOCABLE|**nvarchar (** 10 **)**|如果可以隐式调用例程，则返回 YES；如果不能隐式调用函数，则返回 NO。<br /><br /> 始终返回 NO。|  
|CREATED|**datetime**|创建例程的时间。|  
|LAST_ALTERED|**datetime**|最后一次修改函数的时间。|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [&#40;Transact-sql&#41;的信息架构视图 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.procedures (Transact-SQL)](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)  
  
