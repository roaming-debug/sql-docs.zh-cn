---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4472f545fd22be8b957e6ad5d5722230ab75ff76
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465158"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为在中为 SQL 语言定义的模块的每个对象（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包括本机编译的标量用户定义函数）返回一行。 类型为 P、RF、V、TR、FN、IF、TF 和 R 的对象均有关联的 SQL 模块。 在此视图中，独立的默认值，即 D 类型的对象也具有 SQL 模块定义。 有关这些类型的说明，请参阅 [sys.databases](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)目录视图中的 **类型** 列。  
  
 有关详细信息，请参阅[内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|包含对象的对象 ID。 在数据库中是唯一的。|  
|**definition**|**nvarchar(max)**|定义此模块的 SQL 文本。 还可以使用 [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) 内置函数获取此值。<br /><br /> NULL = 已加密。|  
|uses_ansi_nulls|**bit**|模块是使用 SET ANSI_NULLS ON 创建的。<br /><br /> 对于规则和默认值，始终 = 0。|  
|**uses_quoted_identifier**|**bit**|模块是使用 SET QUOTED_IDENTIFIER ON 创建的。|  
|**is_schema_bound**|**bit**|模块是通过 SCHEMABINDING 选项创建的。<br /><br /> 对于本机编译存储过程，始终包含值 1。|  
|**uses_database_collation**|**bit**|1 = 架构绑定模块定义取决于正确处理所需的数据库的默认排序规则；否则为 0。 这种依赖关系可防止更改数据库的默认排序规则。|  
|**is_recompiled**|**bit**|已通过重新编译选项创建了过程。|  
|**null_on_null_input**|**bit**|模块被声明为针对任何 NULL 输入生成 NULL 输出。|  
|**execute_as_principal_id**|**Int**|EXECUTE AS 数据库主体的 ID。<br /><br /> 默认情况下，或者 EXECUTE AS CALLER 时，为 NULL。<br /><br /> 指定主体的 ID （如果 EXECUTE AS SELF 或 EXECUTE AS） \<principal> 。<br /><br /> -2 = EXECUTE AS OWNER。|  
|**uses_native_compilation**|**bit**|**适用范围**： [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]。<br /><br /> 0 = 非本机编译<br /><br /> 1 = 本机编译<br /><br /> 默认值为 0。|  
|**is_inlineable**|**bit**|**适用于**：[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] 及更高版本。<br/><br />指示模块是否可内联。 Inlineability 基于 [此处](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements)指定的条件。<br /><br /> 0 = 非可内联<br /><br /> 1 = 可内联。 <br /><br /> 对于标量 Udf，如果 UDF 为可内联，则值为 1; 否则为0。 对于内联 Tvf，它始终包含值1，对于所有其他模块类型，该值始终为0。<br />|  
|**inline_type**|**bit**|**适用于**：[!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] 及更高版本。<br /><br />指示当前是否为模块启用内联。 <br /><br />0 = 关闭内联<br /><br /> 1 = 开启内联。<br /><br /> 对于标量 Udf，如果显式或隐式)  (启用内联，则该值为1。 对于内联 Tvf，值始终为1，对于其他模块类型，该值始终为0。<br />|  

  
## <a name="remarks"></a>备注  
 默认约束（类型为 D 的对象）的 SQL 表达式可在 [sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) 目录视图中找到。 在 [sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) 目录视图中找到检查约束（类型为 C 的对象）的 SQL 表达式。  
  
 [Sys.dm_db_uncontained_entities &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)中也介绍了此信息。  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
 下面的示例返回当前数据库中每个模块的名称、类型和定义。  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
