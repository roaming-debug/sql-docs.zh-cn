---
description: sp_statistics (Transact-SQL)
title: sp_statistics (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_statistics_TSQL
- sp_statistics
dev_langs:
- TSQL
helpviewer_keywords:
- sp_statistics
ms.assetid: 0bb6495f-258a-47ec-9f74-fd16671d23b8
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab64c4fa1ea96390631508130ba9a7b57d8a8493
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97410468"
---
# <a name="sp_statistics-transact-sql"></a>sp_statistics (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回针对指定的表或索引视图的所有索引和统计信息的列表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_statistics [ @table_name = ] 'table_name'    
     [ , [ @table_owner = ] 'owner' ]   
     [ , [ @table_qualifier = ] 'qualifier' ]   
     [ , [ @index_name = ] 'index_name' ]   
     [ , [ @is_unique = ] 'is_unique' ]  
     [ , [ @accuracy = ] 'accuracy' ]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>自变量  
`[ @table_name = ] 'table_name'` 指定用于返回目录信息的表。 *table_name* **sysname**，无默认值。 不支持通配符模式匹配。  
  
`[ @table_owner = ] 'owner'` 用于返回目录信息的表的表所有者的名称。 *table_owner* 的默认值为 **sysname**，默认值为 NULL。 不支持通配符模式匹配。 如果未指定 *owner* ，则应用基础 DBMS 的默认表可见性规则。  
  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，如果当前用户拥有一个具有指定名称的表，则返回该表的索引。 如果未指定 *owner* ，并且当前用户没有具有指定 *名称* 的表，则此过程将查找由数据库所有者拥有的具有指定 *名称* 的表。 如果存在这样的表，则返回该表的索引。  
  
`[ @table_qualifier = ] 'qualifier'` 表限定符的名称。 *限定符* 的值为 **sysname**，默认值为 NULL。 各种 DBMS 产品支持表的三部分命名 (_限定符_**。**_所有者_**。**_名称_) 。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，此参数表示数据库名称。 在某些产品中，该列表示表所在的数据库环境的服务器名。  
  
`[ @index_name = ] 'index_name'` 索引名称。 *index_name* 的值为 **sysname**，默认值为%。 支持通配符模式匹配。  
  
`[ @is_unique = ] 'is_unique'` 指示是否只有唯一索引 (要返回的 **Y**) 。 *is_unique* 为 **char (1)**，默认值为 **N**。  
  
`[ @accuracy = ] 'accuracy'` 统计信息的基数和页准确性的级别。 *准确性* 为 **char (1)**，默认值为 **Q**。指定 **E** 以确保统计信息更新，以便基数和页面准确无误。  
  
 值 **E** (SQL_ENSURE) 要求驱动程序无条件检索统计信息。  
  
 值 **Q** (SQL_QUICK) 会要求驱动程序仅在服务器上随时可用时才检索基数和页面。 在这种情况下，驱动程序不能保证是最新值。 按照 Open Group 标准编写的应用程序将总是从 ODBC 3.x 兼容驱动程序获得 SQL_QUICK 行为。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_QUALIFIER**|**sysname**|表限定符名称。 该列可以为 NULL。|  
|**TABLE_OWNER**|**sysname**|表所有者名称。 该列始终返回值。|  
|**TABLE_NAME**|**sysname**|表名。 该列始终返回值。|  
|**NON_UNIQUE**|**smallint**|NOT NULL。<br /><br /> 0 = 唯一<br /><br /> 1 = 不唯一|  
|**INDEX_QUALIFIER**|**sysname**|索引所有者名称。 某些 DBMS 产品允许表所有者之外的用户创建索引。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，此列始终与 **TABLE_NAME** 相同。|  
|**INDEX_NAME**|**sysname**|索引的名称。 该列始终返回值。|  
|**TYPE**|**smallint**|此列始终返回值：<br /><br /> 0 = 表的统计信息<br /><br /> 1 = 聚集<br /><br /> 2 = 哈希<br /><br /> 3 = 非聚集|  
|**SEQ_IN_INDEX**|**smallint**|列在索引中的位置。|  
|**COLUMN_NAME**|**sysname**|返回的每个 **TABLE_NAME** 列的列名。 该列始终返回值。|  
|**COLLATION**|**char(1)**|在排序规则中使用的顺序。 可以是：<br /><br /> A = 升序<br /><br /> D = 降序<br /><br /> NULL = 不适用|  
|**基数**|**int**|表中的行数或索引中的唯一值。|  
|**页**|**int**|用于存储索引或表的页数。|  
|**FILTER_CONDITION**|**varchar(128)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不返回值。|  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="remarks"></a>备注  
 结果集中的索引按列 **NON_UNIQUE**、 **类型**、 **INDEX_NAME** 和 **SEQ_IN_INDEX** 按升序显示。  
  
 聚集索引类型引用一个索引，该索引中的表数据按索引的顺序存储。 这对应于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 聚集索引。  
  
 哈希索引类型接受完全匹配或范围搜索，但模式匹配搜索不使用该索引。  
  
 **sp_statistics** 等效于 ODBC 中的 **SQLStatistics** 。 返回的结果按 **NON_UNIQUE**、 **类型**、 **INDEX_QUALIFIER**、 **INDEX_NAME** 和 **SEQ_IN_INDEX** 进行排序。 有关详细信息，请参阅 [ODBC API 参考](../../odbc/reference/syntax/odbc-reference.md)。  
  
## <a name="permissions"></a>权限  
 需要对架构的 SELECT 权限。  
  
## <a name="example-sssdwfull-and-sspdw"></a>示例： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 下面的示例返回有关表的信息 `DimEmployee` 。  
  
```sql  
-- Uses AdventureWorks  
  
EXEC sp_statistics DimEmployee;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的目录存储过程 ](../../relational-databases/system-stored-procedures/catalog-stored-procedures-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
