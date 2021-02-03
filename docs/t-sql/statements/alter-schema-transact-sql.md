---
description: ALTER SCHEMA (Transact-SQL)
title: ALTER SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER SCHEMA
- ALTER_SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- objects [SQL Server], transferring
- transferring objects between schemas
- ALTER SCHEMA statement
- schemas [SQL Server], modifying
- modifying schemas
ms.assetid: 0a760138-460e-410a-a3c1-d60af03bf2ed
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8bee4d9040dc3c5cdc71f4b8f7d1ddf32de7a788
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204053"
---
# <a name="alter-schema-transact-sql"></a>ALTER SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在架构之间传输安全对象。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER SCHEMA schema_name   
   TRANSFER [ <entity_type> :: ] securable_name   
[;]  
  
<entity_type> ::=  
    {  
    Object | Type | XML Schema Collection  
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
ALTER SCHEMA schema_name   
   TRANSFER [ OBJECT :: ] securable_name   
[;]  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 当前数据库中的架构名称，安全对象将移入其中。 其数据类型不能为 SYS 或 INFORMATION_SCHEMA。  
  
 \<entity_type>  
 更改其所有者的实体的类。 Object 是默认值。  
  
 securable_name  
 要移入架构中的架构范围内的安全对象的一部分或两部分名称。  
  
## <a name="remarks"></a>备注  
 用户与架构完全分离。  
  
 ALTER SCHEMA 仅可用于在同一数据库中的架构之间移动安全对象。 若要更改或删除架构中的安全对象，请使用特定于该安全对象的 ALTER 或 DROP 语句。  
  
 如果对 securable_name 使用了由一部分组成的名称，则将使用当前生效的名称解析规则查找该安全对象。  
  
 将安全对象移入新架构时，将删除与该安全对象关联的全部权限。 如果已显式设置安全对象的所有者，则该所有者保持不变。 如果安全对象的所有者已设置为 SCHEMA OWNER，则该所有者将保持为 SCHEMA OWNER；但移动之后，SCHEMA OWNER 将解析为新架构的所有者。 新所有者的 principal_id 将为 NULL。  
  
 无论是 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) 目录视图的 definition 列中的相应对象，还是使用 [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) 内置函数获取的相应对象，移动存储过程、函数、视图或触发器都不会更改其架构名称（如有）。 因此，我们建议不要使用 ALTER SCHEMA 移动这些对象类型。 而是删除对象，然后在新架构中重新创建该对象。  
  
 移动表或同义词不会自动更新对该对象的引用。 必须手动修改引用已移动对象的任何对象。 例如，如果移动了某个表，并且触发器中引用了该表，则必须修改触发器以反映新的架构名称。 请使用 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) 列出该对象上的依赖关系，然后再进行移动。  

 若要通过使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 更改表的架构，请在对象资源管理器中右键单击该表，然后单击“设计”。 按 F4 以打开“属性”窗口。 在“架构”框中，选择新架构。  
 
 ALTER SCHEMA 使用架构级别锁。
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="permissions"></a>权限  
 若要从另一个架构中传输安全对象，当前用户必须拥有对该安全对象（非架构）的 CONTROL 权限，并拥有对目标架构的 ALTER 权限。  
  
 如果已为安全对象指定 EXECUTE AS OWNER，且所有者已设置为 SCHEMA OWNER，则用户还必须拥有对目标架构所有者的 IMPERSONATE 权限。  
  
 在移动安全对象后，将删除与所传输的安全对象相关联的所有权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-transferring-ownership-of-a-table"></a>A. 传递表的所有权  
 以下示例通过将表 `HumanResources` 从架构 `Address` 传输到 `Person`“HumanResources”架构来修改该架构。  
  
```sql  
USE AdventureWorks2012;  
GO  
ALTER SCHEMA HumanResources TRANSFER Person.Address;  
GO  
```  
  
### <a name="b-transferring-ownership-of-a-type"></a>B. 传递类型的所有权  
 以下示例在 `Production` 架构中创建一个类型，然后将该类型传递到 `Person` 架构。  
  
```sql  
USE AdventureWorks2012;  
GO  
  
CREATE TYPE Production.TestType FROM [VARCHAR](10) NOT NULL ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
  
-- Change the type to the Person schema.  
ALTER SCHEMA Person TRANSFER type::Production.TestType ;  
GO  
  
-- Check the type owner.  
SELECT sys.types.name, sys.types.schema_id, sys.schemas.name  
    FROM sys.types JOIN sys.schemas   
        ON sys.types.schema_id = sys.schemas.schema_id   
    WHERE sys.types.name = 'TestType' ;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-transferring-ownership-of-a-table"></a>C. 传递表的所有权  
 下面的示例在 `dbo` 架构中创建一个表 `Region`，创建一个 `Sales` 架构，然后将 `Region` 表从 `dbo` 架构移动到 `Sales` 架构。  
  
```sql  
CREATE TABLE dbo.Region   
    (Region_id INT NOT NULL,  
    Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
  
CREATE SCHEMA Sales;  
GO  
  
ALTER SCHEMA Sales TRANSFER OBJECT::dbo.Region;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

