---
title: 元数据可见性配置 | Microsoft Docs
description: 了解如何在 SQL Server 中为用户拥有或已具有访问权限的安全对象配置元数据可见性。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- subcomponents visibility [SQL Server]
- metadata [SQL Server], visibility
- permissions [SQL Server], metadata access
- viewing metadata
- objects [SQL Server], metadata
- displaying metadata
- database metadata [SQL Server]
- metadata [SQL Server], permissions
ms.assetid: 50d2e015-05ae-4014-a1cd-4de7866ad651
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65eeb429fbef85f1e23a79a8f06299a64a5179d2
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755857"
---
# <a name="metadata-visibility-configuration"></a>元数据可见性配置
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  元数据的可见性仅限用户所拥有的安全对象，或已授予用户某些权限的安全对象。 例如，如果用户获得了对表 `myTable`的 SELECT 或 INSERT 权限，则下面的查询将返回一行。  
  
```  
SELECT name, object_id  
FROM sys.tables  
WHERE name = N'myTable';  
GO  
```  
  
 但如果用户对 `myTable`没有任何权限，则查询返回空的结果集。  
  
## <a name="scope-and-impact-of-metadata-visibility-configuration"></a>元数据可见性配置的作用域和影响  
 元数据可见性配置仅可应用于下列安全对象。  

:::row:::
    :::column:::
        目录视图

        公开元数据的内置函数

        兼容性视图
    :::column-end:::
    :::column:::
         [!INCLUDE[ssDE](../../includes/ssde-md.md)] sp_help 存储过程

        信息架构视图

        扩展属性
    :::column-end:::
:::row-end:::
  
 元数据可见性配置不能应用于下列安全对象。  

:::row:::
    :::column:::
        日志传送系统表

        数据库维护计划系统表

        复制系统表
    :::column-end:::
    :::column:::
         [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理系统表

        备份系统表

        复制及 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 **sp_help** 存储过程
    :::column-end:::
:::row-end:::

 有限的元数据可访问性意味着：  
  
-   假定 **public** 可访问元数据的应用程序将中断。  
  
-   对系统视图的查询可能只返回行子集，有时返回空的结果集。  
  
-   元数据生成的内置函数（如 OBJECTPROPERTYEX）可能返回 NULL。  
  
-   [!INCLUDE[ssDE](../../includes/ssde-md.md)] sp_help 存储过程可能只返回行子集或 NULL。  
  
 SQL 模块（如存储过程和触发器）在调用方的安全上下文中运行，因此，它们只有有限的元数据访问性。 例如，在以下代码中，当存储过程尝试访问表 `myTable` （调用方对该表没有权限）的元数据时，返回空的结果集。 在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，返回一行。  
  
```  
CREATE PROCEDURE assumes_caller_can_access_metadata  
BEGIN  
SELECT name, object_id   
FROM sys.objects   
WHERE name = N'myTable';  
END;  
GO  
```  
  
 若要允许调用方查看元数据，可在适当的作用域（对象级、数据库级或服务器级）中授予调用方 VIEW DEFINITION 权限。 因此，在以上示例中，如果调用方对 `myTable`具有 VIEW DEFINITION 权限，则存储过程返回一行。 有关详细信息，请参阅 [GRANT (Transact SQL)](../../t-sql/statements/grant-transact-sql.md) 和[授予数据库权限 (Transact SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)。  
  
 也可以将存储过程修改为使用所有者凭据执行。 当过程所有者和表所有者相同时，便会应用所有权链，并且过程所有者的安全上下文便会启用对 `myTable`元数据的访问。 在这种情况下，以下代码会向调用方返回一行元数据。  
  
> [!NOTE]  
>  以下示例使用的是 [sys.objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) 目录视图，而非 [sys.sysobjects](../../relational-databases/system-compatibility-views/sys-sysobjects-transact-sql.md) 兼容视图。  
  
```  
CREATE PROCEDURE does_not_assume_caller_can_access_metadata  
WITH EXECUTE AS OWNER  
AS  
BEGIN  
SELECT name, object_id  
FROM sys.objects   
WHERE name = N'myTable'   
END;  
GO  
```  
  
> [!NOTE]  
>  您可以使用 EXECUTE AS 临时切换到调用方的安全上下文。 有关详细信息，请参阅 [EXECUTE AS (Transact SQL)](../../t-sql/statements/execute-as-transact-sql.md)。  
  
## <a name="benefits-and-limits-of-metadata-visibility-configuration"></a>元数据可见性配置的优点和限制  
 元数据可见性配置在整个安全计划中发挥着重要作用。 但在有些情况下，有经验的用户和已确定的用户可以强制公开某些元数据。 建议将元数据权限部署为多种深层防御中的一种。  
  
 理论上可以通过控制查询中的谓词评估顺序，强制在错误消息中显示元数据。 此类“试错攻击”的威胁并非是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 专有的。 它由关系代数中允许的关联转换和交换转换表示。 可以通过限制错误消息中返回的信息来降低此风险。 为了以此方式进一步限制元数据的可见性，可以使用跟踪标志 3625 启动服务器。 此跟踪标志限制错误消息中显示的信息量。 进一步有助于防止强制泄漏。 需要在错误消息的简洁性和用于调试的难易程度之间进行权衡。 有关详细信息，请参阅[数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)和[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。  
  
 下列元数据无法强制公开：  
  
-   存储在 **sys.servers** 的 **provider_string** 列中的值。 没有 ALTER ANY LINKED SERVER 权限的用户将看到此列中的值为 NULL。  
  
-   用户定义的对象（如存储过程或触发器）的源定义。 只有在满足下列某一种条件时，才能看到源代码：  
  
    -   用户对该对象具有 VIEW DEFINITION 权限。  
  
    -   用户没有被拒绝对该对象具有 VIEW DEFINITION 权限，并且还对其具有 CONTROL、ALTER 或 TAKE OWNERSHIP 权限。 所有其他用户看到的是 NULL。  
  
-   在下列目录视图中找到的定义列：  

    - **sys.all_sql_modules**  
    - **sys.server_sql_modules**  
    - **sys.default_constraints**
    - **sys.numbered_procedures**
    - **sys.sql_modules**
    - **sys.check_constraints**
    - **sys.computed_columns**

-   **syscomments** 兼容视图中的 **ctext** 列。  
  
-   **sp_helptext** 过程的输出。  
  
-   信息架构视图中的以下列：

    - INFORMATION_SCHEMA.CHECK_CONSTRAINTS.CHECK_CLAUSE
    - INFORMATION_SCHEMA.DOMAINS.DOMAIN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINES.ROUTINE_DEFINITION
    - INFORMATION_SCHEMA.COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.ROUTINE_COLUMNS.COLUMN_DEFAULT
    - INFORMATION_SCHEMA.VIEWS.VIEW_DEFINITION

-   OBJECT_DEFINITION() 函数  
  
-   存储在 **sys.sql_logins** 的 password_hash 列中的值。  不具有 CONTROL SERVER 权限的用户可以看到该列中的 NULL 值。  
  
> [!NOTE]  
>  内置系统过程和函数的 SQL 定义通过 **sys.system_sql_modules** 目录视图、 **sp_helptext** 存储过程以及 OBJECT_DEFINITION() 函数公开可见。  
  
## <a name="general-principles-of-metadata-visibility"></a>元数据可见性的一般原则  
 下列是需要注意的有关元数据可见性的一般原则：  
  
-   固定角色隐式权限  
  
-   权限的作用域  
  
-   DENY 的优先顺序  
  
-   子组件元数据的可见性  
  
### <a name="fixed-roles-and-implicit-permissions"></a>固定角色和隐式权限  
 固定角色可以访问的元数据取决于这些角色相应的隐式权限。  
  
### <a name="scope-of-permissions"></a>权限的作用域  
 某个作用域上的权限意味着可以查看该作用域和所有包含的作用域上的元数据。 例如，对架构的 SELECT 权限意味着被授权者对该架构所包含的所有安全对象都具有 SELECT 权限。 因此，授予对架构的 SELECT 权限可以使用户查看架构的元数据以及其中的所有表、视图、函数、过程、队列、同义词、类型和 XML 架构集合。 有关范围的详细信息，请参阅[权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)。  
  
### <a name="precedence-of-deny"></a>DENY 的优先顺序  
 DENY 通常优先于其他权限。 例如，如果授予数据库用户对架构的 EXECUTE 权限，但是拒绝其对架构中存储过程的 EXECUTE 权限，则该用户不能查看此存储过程的元数据。  
  
 另外，如果拒绝用户对某个架构具有 EXECUTE 权限，但授予其对该架构中某个存储过程具有 EXECUTE 权限，则该用户无法查看该存储过程的元数据。  
  
 再比如，如果通过各种角色成员身份同时授予和拒绝用户对存储过程的 EXECUTE 权限，则优先执行 DENY 权限，因此，用户不能查看存储过程的元数据。  
  
### <a name="visibility-of-subcomponent-metadata"></a>子组件元数据的可见性  
 索引、检查约束和触发器这类子组件的可见性由对父级组件的权限决定。 这些子组件没有可授予的权限。 例如，如果针对表授予用户某些权限，则用户可以查看表、列、索引、检查约束、触发器以及其他此类子组件的元数据。  
  
#### <a name="metadata-that-is-accessible-to-all-database-users"></a>所有数据库用户可访问的元数据  
 某些元数据对于特定数据库中的所有用户都必须是可访问的。 例如，文件组没有可授予的权限，因此不能授予用户查看文件组的元数据的权限。 但是，可以创建表的任何用户都必须能够访问文件组元数据，以使用 CREATE TABLE 语句的 ON *filegroup* 或 TEXTIMAGE_ON *filegroup* 子句。  
  
 所有用户均可查看由 DB_ID() 函数和 DB_NAME() 函数返回的元数据。  
  
 下表列出了对 public 角色可见的目录视图。  

:::row:::
    :::column:::
        **sys.partition_functions**

        **sys.partition_schemes**

        **sys.filegroups**

        **sys.database_files**

        **sys.partitions**

        **sys.schemas**

        **sys.sql_dependencies**

        **sys.parameter_type_usages**
    :::column-end:::
    :::column:::
        **sys.partition_range_values**

        **sys.data_spaces**

        **sys.destination_data_spaces**

        **sys.allocation_units**

        **sys.messages**

        **sys.configurations**

        **sys.type_assembly_usages**

        **sys.column_type_usages**
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [EXECUTE AS 子句 (Transact-SQL)](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [兼容性视图 (Transact SQL)](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
