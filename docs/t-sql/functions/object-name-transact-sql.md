---
description: OBJECT_NAME (Transact-SQL)
title: OBJECT_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OBJECT_NAME
- OBJECT_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- OBJECT_NAME function
- viewing database object names
- objects [SQL Server], names
- database objects [SQL Server], names
- displaying database object names
- database objects [SQL Server]
- names [SQL Server], database objects
ms.assetid: 7d5b923f-0c3e-4af9-b39b-132807a6d5b3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 57282b366dd13bcece297359787b6cc5c62d263e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468168"
---
# <a name="object_name-transact-sql"></a>OBJECT_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回架构范围内对象的数据库对象名称。 有关架构范围内对象的列表，请参阅 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
OBJECT_NAME ( object_id [, database_id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 object_id  
 要使用的对象的 ID。 object_id 的数据类型为 int，并假定为指定数据库或当前数据库上下文中的架构范围内的对象。  
  
 database_id  
 要在其中查找对象的数据库的 ID。 database_id 的数据类型为 int。  
  
## <a name="return-types"></a>返回类型  
 **sysname**  
  
## <a name="exceptions"></a>例外  
 出现错误时或调用方没有查看对象的权限时，将返回 NULL。 如果目标数据库的 AUTO_CLOSE 选项设置为 ON，则此函数将打开此数据库。  
  
 用户只能查看符合如下条件的安全对象的元数据：该安全对象为该用户所有，或已授予该用户对该安全对象的权限。 也就是说，如果用户对该对象没有任何权限，则那些会生成元数据的内置函数（如 OBJECT_NAME）可能返回 NULL。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="permissions"></a>权限  
 需要对对象拥有 ANY 权限。 若要指定数据库 ID，还需要对数据库拥有 CONNECT 权限，或者必须启用 guest 帐户。  
  
## <a name="remarks"></a>备注  
 系统函数可以在选择列表、WHERE 子句和任何允许使用表达式的地方使用。 有关详细信息，请参阅[表达式](../../t-sql/language-elements/expressions-transact-sql.md)和 [WHERE](../../t-sql/queries/where-transact-sql.md)。  
  
 由此系统函数返回的值使用当前数据库的排序规则。  
  
 默认情况下，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]假定 object_id 在当前数据库的上下文中。 在其他数据库中引用 object_id 的查询将返回 NULL 或错误的结果。 例如，以下查询中当前数据库上下文是 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]将尝试返回在该数据库（而非查询的 FROM 子句中指定的数据库）中指定的对象 ID 的对象名称。 因此，会返回不正确的信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_NAME(object_id)  
FROM master.sys.objects;  
GO  
```  
  
 您可以通过指定数据库 ID 在其他数据库的上下文中解析对象名。 以下示例在 `master` 函数中指定 `OBJECT_SCHEMA_NAME` 数据库的数据库 ID，并返回正确的结果。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT DISTINCT OBJECT_SCHEMA_NAME(object_id, 1) AS schema_name  
FROM master.sys.objects;  
GO  
```  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-object_name-in-a-where-clause"></a>A. 在 WHERE 子句中使用 OBJECT_NAME  
 以下示例将返回来自对象的 `sys.objects` 目录视图的列，该对象是通过 `OBJECT_NAME` 在 `WHERE` 语句的 `SELECT` 子句中指定的。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyID INT;  
SET @MyID = (SELECT OBJECT_ID('AdventureWorks2012.Production.Product',  
    'U'));  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(@MyID);  
GO  
```  
  
### <a name="b-returning-the-object-schema-name-and-object-name"></a>B. 返回对象架构名称和对象名称  
 以下示例返回所有缓存查询计划（为非临时语句或预定义语句）的对象架构名称、对象名称和 SQL 文本。  
  
```sql  
SELECT DB_NAME(st.dbid) AS database_name,   
    OBJECT_SCHEMA_NAME(st.objectid, st.dbid) AS schema_name,  
    OBJECT_NAME(st.objectid, st.dbid) AS object_name,   
    st.text AS query_text  
FROM sys.dm_exec_query_stats AS qs  
CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) AS st  
WHERE st.objectid IS NOT NULL;  
GO  
```  
  
### <a name="c-returning-three-part-object-names"></a>C. 返回由三部分组成的对象名称  
 以下示例返回所有数据库中所有对象的数据库名称、架构名称和对象名称，同时返回 `sys.dm_db_index_operational_stats` 动态管理视图中的其他所有列。  
  
```sql  
SELECT QUOTENAME(DB_NAME(database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_SCHEMA_NAME(object_id, database_id))   
    + N'.'   
    + QUOTENAME(OBJECT_NAME(object_id, database_id))  
    , *   
FROM sys.dm_db_index_operational_stats(NULL, NULL, NULL, NULL);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-object_name-in-a-where-clause"></a>D. 在 WHERE 子句中使用 OBJECT_NAME  
 以下示例将返回来自对象的 `sys.objects` 目录视图的列，该对象是通过 `OBJECT_NAME` 在 `WHERE` 语句的 `SELECT` 子句中指定的。 （对象编号（在下面的示例中为 274100017）各不相同。  若要测试此示例，请通过在数据库中执行 `SELECT name, object_id FROM sys.objects;` 来查看有效的对象编号。）  
  
```sql  
SELECT name, object_id, type_desc  
FROM sys.objects  
WHERE name = OBJECT_NAME(274100017);  
```  
  
## <a name="see-also"></a>另请参阅  
 [元数据函数 (Transact-SQL)](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)   
 [OBJECT_ID (Transact-SQL)](../../t-sql/functions/object-id-transact-sql.md)  
  
  

