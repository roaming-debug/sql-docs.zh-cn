---
description: 查询 SQL Server 系统目录常见问题
title: 查询 SQL Server 系统目录常见问题解答 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], examples
- metadata [SQL Server], frequently asked questions
- metadata [SQL Server], example queries
- system catalogs [SQL Server], example queries
- catalog views [SQL Server], frequently asked questions
ms.assetid: ca202580-c37e-4ccd-9275-77ce79481f64
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 523db89e98e2c95bac3231efa6843f21b8ae1216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200981"
---
# <a name="querying-the-sql-server-system-catalog-faq"></a>查询 SQL Server 系统目录常见问题
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主题包含一个常见问题列表。 这些问题的答案是基于目录视图的查询。  
  
##  <a name="frequently-asked-questions"></a><a name="_TOP"></a> 常见问题  
 下列部分按类别列出常见问题。  
  
### <a name="data-types"></a>数据类型  
  
-   [如何找到指定表中列的数据类型？](#_FAQ7)  
  
-   [如何找到指定表中的 LOB 数据类型？](#_FAQ14)  
  
-   [如何找到依赖于指定数据类型的列？](#_FAQ22)  
  
-   [如何找到依赖于指定 CLR 用户定义类型或别名数据类型的计算列？](#_FAQ23)  
  
-   [如何找到依赖于指定 CLR 用户定义类型或别名类型的参数？](#_FAQ24)  
  
-   [如何找到依赖于指定 CLR 用户定义类型的 CHECK 约束？](#_FAQ25)  
  
-   [如何找到依赖于指定 CLR 用户定义类型或别名类型的视图、Transact-SQL 函数和 Transact-SQL 存储过程？](#_FAQ26)  
  
### <a name="tables-indexes-views-and-constraints"></a>表、索引、视图和约束  
  
-   [如何找到指定数据库中的所有用户定义表？](#_FAQ31)  
  
-   [如何找到指定数据库中没有聚集索引的所有表？](#_FAQ1)  
  
-   [如何找到没有索引的所有表？](#_FAQ4)  
  
-   [如何找到没有主键的所有表？](#_FAQ3)  
  
-   [如何找到具有标识列的所有表？](#_FAQ5)  
  
-   [如何找到所有进行了分区的表和索引？](#_FAQ32)  
  
-   [如何找到数据库中的所有视图？](#_FAQ13)  
  
-   [如何找到视图的定义？](#_FAQ35)  
  
-   [如何找到最近 n 天内修改过的所有实体？](#_FAQ6)  
  
-   [如何找到指定表的主键列？](#_FAQ16)  
  
-   [如何找到指定表的外键列？](#_FAQ17)  
  
-   [如何确定某列是否在计算列表达式中使用？](#_FAQ20)  
  
-   [如何找到在计算列表达式中使用的所有列？](#_FAQ21)  
  
-   [如何找到指定表的所有约束？](#_FAQ27)  
  
-   [如何找到指定表的所有索引？](#_FAQ28)  
  
-   [如何找到具有指定列名称的所有表？](#_FAQ30)  
  
-   [如何找到指定对象的所有统计信息？](#_FAQ33)  
  
-   [如何找到指定对象的所有统计信息和统计信息列？](#_FAQ34)  
  
### <a name="modules-stored-procedures-user-defined-functions-and-triggers"></a>模块（存储过程、用户定义函数和触发器）  
  
-   [如何找到数据库中的所有存储过程？](#_FAQ9)  
  
-   [如何找到数据库中的所有用户定义函数？](#_FAQ12)  
  
-   [如何找到指定存储过程或函数的参数？](#_FAQ10)  
  
-   [如何找到指定函数的依赖项？](#_FAQ8)  
  
-   [如何查看模块定义？](#_FAQ15)  
  
-   [如何查看服务器级别触发器的定义？](#_FAQ19)  
  
### <a name="schemas-users-roles-and-permissions"></a>架构、用户、角色和权限  
  
-   [如何找到指定架构中包含的实体的全部所有者？](#_FAQ2)  
  
-   [如何找到对指定主体授予或拒绝的权限？](#_FAQ18)  
  
## <a name="answers"></a>答案  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-a-clustered-index-in-a-specified-database"></a><a name="_FAQ1"></a> 如何实现查找在指定数据库中没有聚集索引的所有表？  
 运行下列查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name, t.name AS table_name  
FROM sys.tables AS t  
WHERE NOT EXISTS   
   (  
     SELECT * FROM sys.indexes AS i  
     WHERE i.object_id = t.object_id  
     AND i.type = 1  -- or type_desc = 'CLUSTERED'  
   )  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 或者，可以使用以下示例所显示的 `OBJECTPROPERTY` 函数。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name, name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasClustIndex') = 0  
ORDER BY schema_id, name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-owners-of-entities-contained-in-a-specified-schema"></a><a name="_FAQ2"></a> 如何实现查找指定架构中包含的实体的所有所有者？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name>`。  
  
```  
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
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-a-primary-key"></a><a name="_FAQ3"></a> 如何实现查找没有主键的所有表？  
 运行下列查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(t.schema_id) AS schema_name  
    ,t.name AS table_name  
FROM sys.tables t   
WHERE object_id NOT IN   
   (  
    SELECT parent_object_id   
    FROM sys.key_constraints   
    WHERE type_desc = 'PRIMARY_KEY_CONSTRAINT' -- or type = 'PK'  
    );  
GO  
  
```  
  
 或者，可以运行以下查询。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasPrimaryKey') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-do-not-have-an-index"></a><a name="_FAQ4"></a> 如何实现查找没有索引的所有表？  
 运行以下查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name  
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'IsIndexed') = 0  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-that-have-an-identity-column"></a><a name="_FAQ5"></a> 如何实现查找包含标识列的所有表？  
 运行以下查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    , t.name AS table_name  
    , c.name AS column_name  
FROM sys.tables AS t  
JOIN sys.identity_columns c ON t.object_id = c.object_id  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 或者，可以运行以下查询。  
  
> [!NOTE]  
>  此查询不返回列的名称。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,name AS table_name   
FROM sys.tables   
WHERE OBJECTPROPERTY(object_id,'TableHasIdentity') = 1  
ORDER BY schema_name, table_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-data-types-of-the-columns-of-a-specified-table"></a><a name="_FAQ7"></a> 如何实现查找指定表的列的数据类型？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT c.name AS column_name  
    ,c.column_id  
    ,SCHEMA_NAME(t.schema_id) AS type_schema  
    ,t.name AS type_name  
    ,t.is_user_defined  
    ,t.is_assembly_type  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
FROM sys.columns AS c   
JOIN sys.types AS t ON c.user_type_id=t.user_type_id  
WHERE c.object_id = OBJECT_ID('<schema_name.table_name>')  
ORDER BY c.column_id;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-dependencies-on-a-specified-function"></a><a name="_FAQ8"></a> 如何实现查找指定函数上的依赖项？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.function_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS referencing_object_name  
    ,COALESCE(COL_NAME(object_id, column_id), '(n/a)') AS referencing_column_name  
    ,*  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.function_name>')  
ORDER BY OBJECT_NAME(object_id), COL_NAME(object_id, column_id);  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-stored-procedures-in-a-database"></a><a name="_FAQ9"></a> 如何实现查找数据库中的所有存储过程？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`。  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS procedure_name   
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
FROM sys.procedures;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-parameters-for-a-specified-stored-procedure-or-function"></a><a name="_FAQ10"></a> 如何实现查找指定存储过程或函数的参数吗？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.object_name>`。  
  
```  
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
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-user-defined-functions-in-a-database"></a><a name="_FAQ12"></a> 如何实现查找数据库中的所有用户定义函数？  
 运行以下查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
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
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-views-in-a-database"></a><a name="_FAQ13"></a> 如何实现查找数据库中的所有视图？  
 运行以下查询之前，请使用有效数据库名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT name AS view_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,OBJECTPROPERTYEX(object_id,'IsIndexed') AS IsIndexed  
  ,OBJECTPROPERTYEX(object_id,'IsIndexable') AS IsIndexable  
  ,create_date  
  ,modify_date  
FROM sys.views;  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-entities-that-have-been-modified-in-the-last-n-days"></a><a name="_FAQ6"></a> 如何实现查找在最近 N 天内修改过的所有实体？  
 运行以下查询之前，请使用有效值替换 `<database_name>` 和 `<n_days>`。  
  
```  
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
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-lob-data-types-of-a-specified-table"></a><a name="_FAQ14"></a> 如何实现查找指定表的 LOB 数据类型？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
  
USE <database_name>;  
GO  
SELECT name AS column_name   
    ,column_id   
    ,TYPE_NAME(user_type_id) AS type_name  
    ,max_length  
    ,CASE   
       WHEN max_length = -1 AND TYPE_NAME(user_type_id) <> 'xml'  
            THEN 1  
            ELSE 0  
     END AS [(max)]  
FROM sys.columns  
WHERE object_id=OBJECT_ID('<schema_name.table_name>')   
    AND ( TYPE_NAME(user_type_id) IN ('xml','text', 'ntext','image')  
         OR (TYPE_NAME(user_type_id) IN ('varchar','nvarchar','varbinary')  
         AND max_length = -1)  
        );  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-view-the-definition-of-a-module"></a><a name="_FAQ15"></a> 如何实现查看模块的定义？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.object_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 或者，可以使用以下示例所显示的 `OBJECT_DEFINITION` 函数。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-view-the-definition-of-a-server-level-trigger"></a><a name="_FAQ19"></a> 如何实现查看服务器级别触发器的定义吗？  
  
```  
SELECT definition  
FROM sys.server_sql_modules;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-of-a-primary-key-for-a-specified-table"></a><a name="_FAQ16"></a> 如何实现查找指定表的主键的列？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,ic.index_column_id  
    ,key_ordinal  
    ,c.name AS column_name  
    ,TYPE_NAME(c.user_type_id)AS column_type   
    ,is_identity  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
INNER JOIN sys.columns AS c   
    ON ic.object_id = c.object_id AND c.column_id = ic.column_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 或者，可以使用以下示例所显示的 `COL_NAME` 函数。  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,COL_NAME(ic.object_id,ic.column_id) AS column_name  
    ,ic.index_column_id  
    ,key_ordinal  
FROM sys.indexes AS i  
INNER JOIN sys.index_columns AS ic   
    ON i.object_id = ic.object_id AND i.index_id = ic.index_id  
WHERE i.is_primary_key = 1   
    AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-of-a-foreign-key-for-a-specified-table"></a><a name="_FAQ17"></a> 如何实现查找指定表的外键的列？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT   
    f.name AS foreign_key_name  
   ,OBJECT_NAME(f.parent_object_id) AS table_name  
   ,COL_NAME(fc.parent_object_id, fc.parent_column_id) AS constraint_column_name  
   ,OBJECT_NAME (f.referenced_object_id) AS referenced_object  
   ,COL_NAME(fc.referenced_object_id, fc.referenced_column_id) AS referenced_column_name  
   ,is_disabled  
   ,delete_referential_action_desc  
   ,update_referential_action_desc  
FROM sys.foreign_keys AS f  
INNER JOIN sys.foreign_key_columns AS fc   
   ON f.object_id = fc.constraint_object_id   
WHERE f.parent_object_id = OBJECT_ID('<schema_name.table_name>');  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-permissions-granted-or-denied-to-a-specified-principal"></a><a name="_FAQ18"></a> 如何实现查找授予或拒绝指定主体的权限？  
 以下示例创建函数以返回检查对其权限的实体的名称。 在下列查询中调用函数。 必须在每个数据库（要在其中检查权限）中创建函数。  
  
```  
-- Create a function to return the name of the entity on which the permissions are checked.  
IF OBJECT_ID (N'dbo.entity_instance_name', N'FN') IS NOT NULL  
    DROP FUNCTION dbo.entity_instance_name;  
GO  
CREATE FUNCTION dbo.entity_instance_name(@class_desc nvarchar(60), @major_id int)   
RETURNS sysname AS  
BEGIN  
    DECLARE @the_entity_name sysname  
    SELECT @the_entity_name = CASE  
        WHEN @class_desc = 'DATABASE' THEN DB_NAME()  
        WHEN @class_desc = 'SCHEMA' THEN SCHEMA_NAME(@major_id)  
        WHEN @class_desc = 'OBJECT_OR_COLUMN' THEN OBJECT_NAME(@major_id)  
        WHEN @class_desc = 'DATABASE_PRINCIPAL' THEN USER_NAME(@major_id)  
        WHEN @class_desc = 'ASSEMBLY' THEN   
            (SELECT name FROM sys.assemblies WHERE assembly_id=@major_id)  
        WHEN @class_desc = 'TYPE' THEN TYPE_NAME(@major_id)  
        WHEN @class_desc = 'XML_SCHEMA_COLLECTION' THEN   
            (SELECT name FROM sys.xml_schema_collections  
              WHERE xml_collection_id=@major_id)  
        WHEN @class_desc = 'MESSAGE_TYPE' THEN   
            (SELECT name FROM sys.service_message_types WHERE message_type_id=@major_id)  
        WHEN @class_desc = 'SERVICE_CONTRACT' THEN   
           (SELECT name FROM sys.service_contracts  
              WHERE service_contract_id=@major_id)  
        WHEN @class_desc = 'SERVICE' THEN  
          (SELECT name FROM sys.services WHERE service_id=@major_id)  
        WHEN @class_desc = 'REMOTE_SERVICE_BINDING' THEN  
          (SELECT name FROM sys.remote_service_bindings  
             WHERE remote_service_binding_id=@major_id)  
        WHEN @class_desc = 'ROUTE' THEN  
          (SELECT name FROM sys.routes WHERE route_id=@major_id)  
        WHEN @class_desc = 'FULLTEXT_CATALOG' THEN  
          (SELECT name FROM sys.fulltext_catalogs WHERE fulltext_catalog_id=@major_id)  
        WHEN @class_desc = 'SYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.symmetric_keys WHERE symmetric_key_id=@major_id)  
        WHEN @class_desc = 'CERTIFICATE' THEN  
          (SELECT name FROM sys.certificates WHERE certificate_id=@major_id)  
        WHEN @class_desc = 'ASYMMETRIC_KEY' THEN  
          (SELECT name FROM sys.asymmetric_keys WHERE asymmetric_key_id=@major_id)  
        WHEN @class_desc = 'SERVER' THEN   
             (SELECT name FROM sys.servers WHERE server_id=@major_id)  
        WHEN @class_desc = 'SERVER_PRINCIPAL' THEN SUSER_NAME(@major_id)  
        WHEN @class_desc = 'ENDPOINT' THEN   
             (SELECT name FROM sys.endpoints WHERE endpoint_id=@major_id)        
        ELSE '?'  
    END  
    RETURN @the_entity_name  
END;  
GO  
-- Return server-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc, major_id) AS entity_name   
    ,minor_id  
    ,SUSER_NAME(grantee_principal_id) AS grantee  
    ,SUSER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc   
FROM sys.server_permissions   
WHERE grantee_principal_id = SUSER_ID('public');  
GO  
-- Return database-level permissions for the user.  
SELECT class  
    ,class_desc  
    ,dbo.entity_instance_name(class_desc , major_id) AS entity_name   
    ,minor_id  
    ,USER_NAME(grantee_principal_id) AS grantee  
    ,USER_NAME(grantor_principal_id) AS grantor  
    ,type  
    ,permission_name  
    ,state_desc     
FROM  sys.database_permissions   
WHERE grantee_principal_id = DATABASE_PRINCIPAL_ID('public');  
GO  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-determine-if-a-column-is-used-in-a-computed-column-expression"></a><a name="_FAQ20"></a> 如何实现确定列是否在计算列表达式中使用？  
 运行以下查询之前，请将 `<database_name>` 、 `<schema_name.table_name>` 和> 替换 `<column_name` 为有效的名称。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS computed_column   
    ,class_desc  
    ,is_selected  
    ,is_updated  
    ,is_select_all  
FROM sys.sql_dependencies  
WHERE referenced_major_id = OBJECT_ID('<schema_name.table_name>')  
    AND referenced_minor_id = COLUMNPROPERTY(referenced_major_id, '<column_name>', 'ColumnId')  
    AND class = 1;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-columns-that-are-used-in-a-computed-column-expression"></a><a name="_FAQ21"></a> 如何实现查找在计算列表达式中使用的所有列？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(d.referenced_major_id) AS object_name  
    ,COL_NAME(d.referenced_major_id, d.referenced_minor_id) AS column_name  
    ,OBJECT_NAME(referenced_major_id) AS dependent_object_name   
    ,COL_NAME(d.object_id, d.column_id) AS dependent_computed_column  
    ,cc.definition AS computed_column_definition  
FROM sys.sql_dependencies AS d  
JOIN sys.computed_columns AS cc   
    ON cc.object_id = d.object_id AND cc.column_id = d.column_id AND d.object_id=d.referenced_major_id       
WHERE d.class = 1  
ORDER BY object_name, column_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-columns-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ22"></a> 如何实现查找依赖于指定 CLR 用户定义类型或别名类型的列？  
 运行以下查询之前，请将替换为有效的名称，将替换为 `<database_name>` 有效的 `<schema_name.data_type_name>` 架构限定的 CLR 用户定义类型或架构限定的别名类型名称。 下面的查询要求 **db_owner** 角色的成员身份或权限才能查看数据库中的所有依赖列和计算列元数据。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,c.name AS column_name   
    ,SCHEMA_NAME(t.schema_id) AS schema_name  
    ,TYPE_NAME(c.user_type_id) AS user_type_name  
    ,c.max_length  
    ,c.precision  
    ,c.scale  
    ,c.is_nullable  
    ,c.is_computed  
FROM sys.columns AS c  
INNER JOIN sys.types AS t ON c.user_type_id = t.user_type_id  
WHERE c.user_type_id = TYPE_ID('<schema_name.data_type_name>');   
GO  
  
```  
  
 下面的查询将返回依赖于 CLR 用户定义类型或别名的列的受限、窄视图，但结果集对 **public** 角色可见。 如果您已经将您的用户定义类型的 REFERENCE 权限授予了其他人，并且您没有权限查看其他人使用该类型创建的对象的元数据，则可以使用此查询。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name   
    ,COL_NAME(object_id, column_id) AS column_name  
    ,TYPE_NAME(user_type_id) AS user_type  
FROM sys.column_type_usages  
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-computed-columns-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ23"></a> 如何实现查找依赖于指定 CLR 用户定义类型或别名类型的计算列？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`，并使用有效的架构限定的 CLR 用户定义类型和别名类型名称替换 `<schema_name.data_type_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,COL_NAME(object_id, column_id) AS column_name  
FROM sys.sql_dependencies  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(object_id, 'IsTable') = 1;   -- exclude non-table dependencies  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-parameters-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ24"></a> 如何实现查找依赖于指定 CLR 用户定义类型或别名类型的参数吗？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`，并使用有效的架构限定的 CLR 用户定义类型和别名类型名称替换 `<schema_name.data_type_name>`。 下面的查询要求 **db_owner** 角色的成员身份或权限才能查看数据库中的所有依赖列和计算列元数据。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,NULL AS procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
UNION   
SELECT OBJECT_NAME(object_id) AS object_name  
    ,procedure_number  
    ,name AS param_name  
    ,parameter_id AS param_num  
    ,TYPE_NAME(p.user_TYPE_ID) AS type_name  
FROM sys.numbered_procedure_parameters AS p  
WHERE p.user_TYPE_ID = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY object_name, procedure_number, param_num;  
GO  
  
```  
  
 下面的查询将返回依赖于 CLR 用户定义类型或别名的参数的受限、窄视图，但结果集对 **public** 角色可见。 如果您已经将您的用户定义类型的 REFERENCE 权限授予了其他人，并且您没有权限查看其他人使用该类型创建的对象的元数据，则可以使用此查询。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) AS object_name  
    ,parameter_id  
    ,TYPE_NAME(user_type_id) AS type_name  
FROM sys.parameter_type_usages   
WHERE user_type_id = TYPE_ID('<schema_name.data_type_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-check-constraints-that-depend-on-a-specified-clr-user-defined-type"></a><a name="_FAQ25"></a> 如何实现查找依赖于指定 CLR 用户定义类型的 CHECK 约束？  
 运行以下查询之前，请使用 `<database_name>` 有效名称替换，并 `<schema_name.data_type_name>` 使用有效的架构限定 CLR 用户定义类型名称替换。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(o.parent_object_id) AS table_name  
    ,OBJECT_NAME(o.object_id) AS constraint_name  
FROM sys.sql_dependencies AS d  
JOIN sys.objects AS o ON o.object_id = d.object_id  
WHERE referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
    AND class = 2 -- schema-bound references to type  
    AND OBJECTPROPERTY(o.object_id, 'IsCheckCnst') = 1; -- exclude non-CHECK dependencies  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-views-transact-sql-functions-and-transact-sql-stored-procedures-that-depend-on-a-specified-clr-user-defined-type-or-alias-type"></a><a name="_FAQ26"></a> 如何实现查找依赖于指定 CLR 用户定义类型或别名类型的视图、Transact-sql 函数和 Transact-sql 存储过程？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`，并使用有效的架构限定的 CLR 用户定义类型和别名类型名称替换 `<schema_name.data_type_name>`。  
  
 在函数或过程中定义的参数为隐式架构绑定。 因此，可以使用 [sys.sql_dependencies](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md) 目录视图来查看依赖于 CLR 用户定义类型或别名类型的参数。 过程和触发器均未绑定到架构。 这意味着不会维护任何在过程或触发器的主体中定义的表达式与 CLR 用户定义类型或别名类型之间的依赖关系。 具有依赖于 CLR 用户定义类型或别名类型的表达式的架构绑定视图和架构绑定用户定义函数在 **sys.sql_dependencies** 目录视图中维护。 不维护类型和 CLR 函数及类型和 CLR 过程之间的依赖关系。  
  
 以下查询返回指定 CLR 用户定义类型或别名类型在视图、[!INCLUDE[tsql](../../includes/tsql-md.md)] 函数和 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程中的所有架构绑定依赖关系。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS dependent_object_schema  
  ,OBJECT_NAME(o.object_id) AS dependent_object_name  
  ,o.type_desc AS dependent_object_type  
  ,d.class_desc AS kind_of_dependency  
  ,TYPE_NAME (d.referenced_major_id) AS type_name  
FROM sys.sql_dependencies AS d   
JOIN sys.objects AS o  
  ON d.object_id = o.object_id  
  AND o.type IN ('FN','IF','TF', 'V', 'P')  
WHERE d.class = 2 -- dependencies on types  
  AND d.referenced_major_id = TYPE_ID('<schema_name.data_type_name>')  
ORDER BY dependent_object_schema, dependent_object_name;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-constraints-for-a-specified-table"></a><a name="_FAQ27"></a> 如何实现查找指定表的所有约束？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id) as constraint_name  
    ,SCHEMA_NAME(schema_id) AS schema_name  
    ,OBJECT_NAME(parent_object_id) AS table_name  
    ,type_desc  
    ,create_date  
    ,modify_date  
    ,is_ms_shipped  
    ,is_published  
    ,is_schema_published  
FROM sys.objects  
WHERE type_desc LIKE '%CONSTRAINT'   
    AND parent_object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-indexes-for-a-specified-table"></a><a name="_FAQ28"></a> 如何实现查找指定表的所有索引？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.table_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT i.name AS index_name  
    ,i.type_desc  
    ,is_unique  
    ,ds.type_desc AS filegroup_or_partition_scheme  
    ,ds.name AS filegroup_or_partition_scheme_name  
    ,ignore_dup_key  
    ,is_primary_key  
    ,is_unique_constraint  
    ,fill_factor  
    ,is_padded  
    ,is_disabled  
    ,allow_row_locks  
    ,allow_page_locks  
FROM sys.indexes AS i  
INNER JOIN sys.data_spaces AS ds ON i.data_space_id = ds.data_space_id  
WHERE is_hypothetical = 0 AND i.index_id <> 0   
AND i.object_id = OBJECT_ID('<schema_name.table_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-objects-that-have-a-specified-column-name"></a><a name="_FAQ30"></a> 如何实现查找具有指定列名称的所有对象？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<column_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_NAME(object_id)  
FROM sys.columns  
WHERE name = '<column_name>';  
GO  
  
```  
  
 或  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name   
    ,o.name AS object_name  
    ,type_desc  
FROM sys.objects AS o  
INNER JOIN sys.columns AS c ON o.object_id = c.object_id  
WHERE c.name = '<column_name>';  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-user-defined-tables-in-a-specified-database"></a><a name="_FAQ31"></a> 如何实现查找指定数据库中的所有用户定义表？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT *   
FROM sys.tables;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-tables-and-indexes-that-are-partitioned"></a><a name="_FAQ32"></a> 如何实现查找所有已分区的表和索引？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(o.schema_id) AS schema_name  
    ,OBJECT_NAME(p.object_id) AS table_name  
    ,i.name AS index_name  
    ,p.partition_number  
    ,rows   
FROM sys.partitions AS p  
INNER JOIN sys.indexes AS i ON p.object_id = i.object_id AND p.index_id = i.index_id  
INNER JOIN sys.partition_schemes ps ON i.data_space_id=ps.data_space_id  
INNER JOIN sys.objects AS o ON o.object_id = i.object_id  
ORDER BY index_name, partition_number;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-statistics-on-a-specified-object"></a><a name="_FAQ33"></a> 如何实现查找指定对象上的所有统计信息？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`，并使用有效的表、索引视图或表值函数名称替换 `<schema_name.object_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT name AS statistics_name  
    ,stats_id  
    ,auto_created  
    ,user_created  
    ,no_recompute  
FROM sys.stats  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-all-the-statistics-and-statistics-columns-on-a-specified-object"></a><a name="_FAQ34"></a> 如何实现查找指定对象上的所有统计信息和统计信息列吗？  
 运行以下查询之前，请使用有效名称替换 `<database_name>`，并使用有效的表、索引视图或表值函数名称替换 `<schema_name.object_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT s.name AS statistics_name  
    ,c.name AS column_name  
    ,sc.stats_column_id  
FROM sys.stats AS s  
INNER JOIN sys.stats_columns AS sc   
    ON s.object_id = sc.object_id AND s.stats_id = sc.stats_id  
INNER JOIN sys.columns AS c   
    ON sc.object_id = c.object_id AND c.column_id = sc.column_id  
WHERE s.object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 [TOP](#_TOP)  
  
###  <a name="how-do-i-find-the-definition-of-a-view"></a><a name="_FAQ35"></a> 如何实现查找视图的定义？  
 运行以下查询之前，请使用有效名称替换 `<database_name>` 和 `<schema_name.object_name>`。  
  
```  
USE <database_name>;  
GO  
SELECT definition  
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID('<schema_name.object_name>');  
GO  
  
```  
  
 或者，可以使用以下示例所显示的 `OBJECT_DEFINITION` 函数。  
  
```  
USE <database_name>;  
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID('<schema_name.object_name>')) AS ObjectDefinition;  
GO  
  
```  
  
 [TOP](#_TOP)  
  
## <a name="see-also"></a>另请参阅  
 [将系统表映射到系统视图 &#40;Transact-sql&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)  
  
  
