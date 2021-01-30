---
description: " (Azure Synapse Analytics sp_special_columns_100) "
title: " (Azure Synapse Analytics sp_special_columns_100) "
ms.custom: ''
ms.date: 03/14/2017
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.subservice: design
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5774fadc-77cc-46f8-8f9f-a0f9efe95e21
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 26aef826cfe6b35d6428240296b31dbc8a99474c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189264"
---
# <a name="sp_special_columns_100-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_special_columns_100) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  返回一组唯一标识表中某个行的最优列。 如果事务更新了行中的某个值，则还将返回自动更新的列。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_special_columns_100 [ @table_name = ] 'table_name'     
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @qualifier = ] 'qualifier' ]   
     [ , [ @col_type = ] 'col_type' ]   
     [ , [ @scope = ] 'scope' ]  
     [ , [ @nullable = ] 'nullable' ]   
     [ , [ @ODBCVer = ] 'ODBCVer' ]   
[ ; ]  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>参数  
 [ @table_name =] "*table_name*"  
 用于返回目录信息的表的名称。 *名称* 为 **sysname**，无默认值。 不支持通配符模式匹配。  
  
 [ @table_owner =] "*table_owner*"  
 用于返回目录信息的表的表所有者。 *所有者* 为 **sysname**，默认值为 NULL。 不支持通配符模式匹配。 如果未指定 *owner* ，则应用基础 DBMS 的默认表可见性规则。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，如果当前用户拥有一个具有指定名称的表，则返回该表的列。 如果未指定 *owner* ，并且当前用户没有具有指定 *名称* 的表，则此过程将查找由数据库所有者拥有的指定 *名称* 的表。 如果存在该表，则返回该表的列。  
  
 [ @qualifier =] '*限定符*'  
 表限定符的名称。 *限定符* 的值为 **sysname**，默认值为 NULL。 各种 DBMS 产品支持表的三部分命名 (*qualifier.owner.name*) 。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列表示数据库名称。 在某些产品中，它表示表所在数据库环境的服务器名称。  
  
 [ @col_type =] "*col_type*"  
 列类型。 *col_type* 的数据类型为 **char (** 1 **)**，默认值为 r。 type r 返回的最佳列或列集通过检索列中的值，可以唯一标识指定表中的任何行。 列可以是为此目的专门设计的伪列，也可以是表的某个唯一索引的一个或多个列。 如果列类型为 V，则返回指定表中的列（如果有）。如果有事务更新了行中的某个值，则数据源将自动更新返回的列。  
  
 [ @scope =] "*scope*"  
 ROWID 要求的最小作用域。 *作用域* 为 **char (** 1 **)**，默认值为 T。作用域 C 指定 ROWID 仅在定位在该行上时才有效。 如果作用域为 T，则指定 ROWID 对该事务有效。  
  
 [ @nullable =] "*nullable*"  
 指示特殊列能否接受 Null 值。 *可以为 null* **(** 1 **)**，默认值为 U。 O 指定不允许空值的特殊列。 如果值为 U，则指定列可以部分为 Null 值。  
  
 [ @ODBCVer =] "*ODBCVer*"  
 所使用的 ODBC 版本。 *ODBCVer* 的 **整数 (** 4 **)**，默认值为2。 这指示 ODBC 版本 2.0。 有关 ODBC 2.0 版和 ODBC 3.0 版之间的差别的详细信息，请参阅 ODBC 3.0 版的 ODBC SQLSpecialColumns 规范。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|SCOPE|**smallint**|行 ID 的实际作用域。 可以为 0、1 或 2。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 始终返回0。 此字段始终返回值。<br /><br /> 0 = SQL_SCOPE_CURROW。 行 ID 只有位于该行上时才能保证有效。 如果另一个事务更新或删除了该行，则以后使用该行 ID 重新选择时，可能无法返回一个行。<br /><br /> 1 = SQL_SCOPE_TRANSACTION。 行 ID 在当前事务期间保证有效。<br /><br /> 2 = SQL_SCOPE_SESSION。 行 ID 在会话（跨事务边界）期间保证有效。|  
|COLUMN_NAME|**sysname**|返回的 *表* 中每列的列名。 此字段始终返回值。|  
|DATA_TYPE|**smallint**|ODBC SQL 数据类型。|  
|TYPE_NAME|**sysname**|依赖于数据源的数据类型名称;例如， **char**、 **varchar**、 **money** 或 **text**。|  
|PRECISION|**Int**|数据源中的列的精度。 此字段始终返回值。|  
|LENGTH|**Int**|数据源中数据类型的数据类型所需的长度（以字节为单位），例如， **char (** 10 **)** 为10，对于 **整数**，则为 2; 对于 **smallint**，则为2。|  
|SCALE|**smallint**|数据源中列的小数位数。 对于不适用小数位数的数据类型，返回 NULL。|  
|PSEUDO_COLUMN|**smallint**|指示列是否为伪列。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 始终返回 1：<br /><br /> 0 = SQL_PC_UNKNOWN<br /><br /> 1 = SQL_PC_NOT_PSEUDO<br /><br /> 2 = SQL_PC_PSEUDO|  
  
## <a name="remarks"></a>备注  
 sp_special_columns 与 ODBC 中的 SQLSpecialColumns 等效。 返回的结果按 SCOPE 排序。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例将返回有关特定列的信息，该列唯一标识了 `FactFinance` 表中的行。  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_special_columns_100 @table_name = 'FactFinance';  
```  
  
## <a name="see-also"></a>另请参阅  
 [Azure Synapse 分析存储过程](../../relational-databases/system-stored-procedures/sql-data-warehouse-stored-procedures.md)  
  
  

