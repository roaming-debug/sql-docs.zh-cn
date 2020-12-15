---
description: sys.objects (Transact-SQL)
title: sys.databases (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.objects_TSQL
- objects
- sys.objects
- objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.objects catalog view
- table-valued parameters, sys.objects catalog view
- user-defined table types [SQL Server]
- table types [SQL Server]
ms.assetid: f8d6163a-2474-410c-a794-997639f31b3b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fc2a9d5ef268d806a9e7a0f6b78df58539d69a4b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461678"
---
# <a name="sysobjects-transact-sql"></a>sys.objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在数据库内创建的每个用户定义的架构范围内的对象（包括本机编译的标量用户定义函数）都包含一行。  
  
 有关详细信息，请参阅[内存中 OLTP 的标量用户定义函数](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md)。  
  
> [!NOTE]  
>  sys.objects 不显示 DDL 触发器，因为它们不是架构范围内的对象。 所有触发器（包括 DML 和 DDL）都位于 [sys.databases](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)中。 sys.triggers 支持对各种触发器应用混合名称范围规则。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|name|**sysname**|对象名称。|  
|object_id|**int**|对象标识号。 在数据库中是唯一的。|  
|principal_id|**int**|如果不是架构所有者，则为单个所有者的 ID。 默认情况下，架构包含的对象由架构所有者拥有。 不过，通过使用 ALTER AUTHORIZATION 语句更改所有权可以指定备用所有者。<br /><br /> 如果没有备用的单个所有者，则为 NULL。<br /><br /> 如果对象类型为下列类型之一，则为 NULL：<br /><br /> C = CHECK 约束<br /><br /> D = DEFAULT（约束或独立）<br /><br /> F = FOREIGN KEY 约束<br /><br /> PK = PRIMARY KEY 约束<br /><br /> R = 规则（旧式，独立）<br /><br /> TA = 程序集（CLR 集成）触发器<br /><br /> TR = SQL 触发器<br /><br /> UQ = UNIQUE 约束<br /><br /> EC = Edge 约束 |  
|schema_id|**int**|包含该对象的架构的 ID。<br /><br /> 始终包含在 sys 或 INFORMATION_SCHEMA 架构中的架构范围内的系统对象。|  
|parent_object_id|**int**|此对象所属对象的 ID。<br /><br /> 0 = 不是子对象。|  
|type|**char(2)**|对象类型：<br /><br /> AF = 聚合函数 (CLR)<br /><br /> C = CHECK 约束<br /><br /> D = DEFAULT（约束或独立）<br /><br /> F = FOREIGN KEY 约束<br /><br /> FN = SQL 标量函数<br /><br /> FS = 程序集 (CLR) 标量函数<br /><br /> FT = 程序集 (CLR) 表值函数<br /><br /> IF = SQL 内联表值函数<br /><br /> IT = 内部表<br /><br /> P = SQL 存储过程<br /><br /> PC = Assembly (CLR) 存储过程<br /><br /> PG = 计划指南<br /><br /> PK = PRIMARY KEY 约束<br /><br /> R = 规则（旧式，独立）<br /><br /> RF = 复制筛选过程<br /><br /> S = 系统基表<br /><br /> SN = 同义词<br /><br /> SO = 序列对象<br /><br /> U = 表（用户定义类型）<br /><br /> V = 视图<br /><br /> EC = Edge 约束 <br /><br /> <br /><br /> **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> <br /><br /> SQ = 服务队列<br /><br /> TA = 程序集 (CLR) DML 触发器<br /><br /> TF = SQL 表值函数<br /><br /> TR = SQL DML 触发器<br /><br /> TT = 表类型<br /><br /> UQ = UNIQUE 约束<br /><br /> X = 扩展存储过程<br /><br /> <br /><br /> **适用** 于： [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 和更高版本、、和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 。<br /><br /> <br /><br /> ET = 外部表|  
|type_desc|**nvarchar(60)**|对对象类型的说明：<br /><br /> AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_STORED_PROCEDURE<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> CLR_TRIGGER<br /><br /> DEFAULT_CONSTRAINT<br /><br /> EXTENDED_STORED_PROCEDURE<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> INTERNAL_TABLE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> RULE<br /><br /> SEQUENCE_OBJECT<br /><br /> <br /><br /> **适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> <br /><br /> SERVICE_QUEUE<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> SQL_STORED_PROCEDURE<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> SYNONYM<br /><br /> SYSTEM_TABLE<br /><br /> TABLE_TYPE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> USER_TABLE<br /><br /> VIEW|  
|create_date|**datetime**|对象的创建日期。|  
|modify_date|**datetime**|上次使用 ALTER 语句修改对象的日期。 如果对象是表或视图，则在创建或更改表或视图的索引时，modify_date 也会发生更改。|  
|is_ms_shipped|**bit**|对象由内部 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件创建。|  
|is_published|**bit**|对象为发布对象。|  
|is_schema_published|**bit**|仅发布对象的架构。|  
  
## <a name="remarks"></a>备注  
 可以将 [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md)、 [OBJECT_NAME](../../t-sql/functions/object-name-transact-sql.md)和 [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md) ( # A1 内置函数应用于 sys.databases 中显示的对象。  
  
 此视图有一个版本，它具有与系统对象相同的架构（称为 [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)）。 还有另一个名为 [sys.all_objects](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md) 的视图，它显示系统对象和用户对象。 所有这三个目录视图的结构都相同。  
  
 在此版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，扩展索引（例如 XML 索引或空间索引）将视为 sys.objects 中的内部表（type = IT，type_desc = INTERNAL_TABLE）。 对于扩展索引：  
  
-   name 是索引表的内部名称。  
  
-   parent_object_id 是基表的 object_id。  
  
-   is_ms_shipped、is_published 和 is_schema_published 列设置为 0。  

**相关的有用系统视图**  
可以通过对特定类型的对象使用系统视图来查看对象的子集，例如：  
- [sys.tables](sys-tables-transact-sql.md)  
- [sys.views](sys-views-transact-sql.md)  
- [sys.procedures](sys-procedures-transact-sql.md)  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-all-the-objects-that-have-been-modified-in-the-last-n-days"></a>A. 返回在最近 N 天内修改过的所有对象  
 运行以下查询之前，请使用有效值替换 `<database_name>` 和 `<n_days>`。  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
```  
  
### <a name="b-returning-the-parameters-for-a-specified-stored-procedure-or-function"></a>B. 返回指定存储过程或函数的参数  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.object_name>`。  
  
```sql  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
```  
  
### <a name="c-returning-all-the-user-defined-functions-in-a-database"></a>C. 返回数据库中的所有用户定义函数  
 运行以下查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
```  
  
### <a name="d-returning-the-owner-of-each-object-in-a-schema"></a>D. 返回架构中每个对象的所有者。  
 运行以下查询之前，请使用有效名称替换所有的 `<database_name>` 和 `<schema_name>`。  
  
```sql  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.all_objects &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md)   
 [sys.system_objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [查询 SQL Server 系统目录常见问题](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.internal_tables &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md)  
  
  
