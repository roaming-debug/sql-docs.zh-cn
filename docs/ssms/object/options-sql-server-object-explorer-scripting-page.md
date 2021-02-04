---
description: 选项（SQL Server 对象资源管理器 -“脚本”页）
title: 选项（SQL Server 对象资源管理器 -“脚本”页）
ms.custom: seo-lt-2019
ms.date: 08/01/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.ObjectExplorerScripting
- VS.ToolsOptionsPages.Sql_Server_Object_Explorer.ObjectExplorerScripting
ms.assetid: 6105aec9-1b72-4cb2-bd24-fc35f6d95240
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cbaf09a88e5856bfd5c8f36826d8894b947e3c0d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250496"
---
# <a name="options-sql-server-object-explorer---scripting-page"></a>选项（SQL Server 对象资源管理器 -“脚本”页）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
 使用此页可设置脚本选项，以应用于对象资源管理器中对象上下文菜单上的以下命令：  
  
-   用于用户表和视图的“编辑”命令。  
  
-   用于用户创建对象的“生成 <object> 脚本”命令。  
  
-   用于用户创建对象的“修改”命令。  
  
-   本页也可用于设置“生成 SQL Server 脚本向导”的脚本选项默认值。  
  
## <a name="remarks"></a>注解  
“编辑”和“修改”命令产生的结果可能与相同选项设置的“生成 <object> 脚本”命令产生的结果不同。 “编辑”和“修改”命令用于在查询编辑器会话期间修改当前数据库中的对象。 “生成 <object> 脚本”命令用于生成一个脚本，这样以后便可使用此脚本创建对象。  
  
## <a name="options"></a>选项  
通过从每个选项右侧列表中的可用设置中进行选择，可以指定脚本选项。

> [!NOTE]
> 列出的默认设置仅适用于“编写整个数据库及所有数据库对象的脚本”选项，并且在使用“选择特定数据库对象”选项时，此设置可能会有变化。
  
### <a name="general-scripting-options"></a>常规脚本选项  
**分隔各条语句**  
使用批处理分隔符分隔各条 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 若要更改 **查询编辑器**，选择“工具”/“选项”/“查询执行”/“SQL Server”/“常规”/“批处理分隔符”。 默认值为 False。 有关详细信息，请参阅 [GO (Transact-SQL)](../../t-sql/language-elements/sql-server-utilities-statements-go.md)。  
  
**包含说明性标头**  
通过将每个对象的脚本分隔为多个部分以向脚本添加说明性注释。 默认值为 True。 有关详细信息，请参阅 [/ *...* / (Comment) (Transact-SQL)](../../t-sql/language-elements/slash-star-comment-transact-sql.md)。  
  
**包括启用 vardecimal 压缩**  
包含 vardecimal 存储选项。 默认值为 False。 有关详细信息，请参阅 [sp_db_vardecimal_storage_format (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md)。  
  
**编写更改跟踪的脚本**  
将更改跟踪信息包含到脚本中。  
  
**编写全文目录脚本**  
包含用于全文目录的脚本。 默认值为 False。 有关详细信息，请参阅 [CREATE FULLTEXT CATALOG (Transact-SQL)](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)。  
  
**脚本 USE <database>**  
将 USE DATABASE 语句添加到脚本，可在当前 **对象资源管理器** 数据库的上下文中创建数据库对象。 如果希望该脚本可在其他数据库中使用，请选择 False 以忽略该选项。 默认值为 True。 有关详细信息，请参阅 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)。  
  
### <a name="object-scripting-options"></a>对象脚本选项  

**检查对象是否存在** 在删除或更改前检查是否存在具有给定名称的对象，或者在创建前检查是否不存在具有给定名称的对象。 有关详细信息，请参阅 [IF...ELSE (Transact-SQL)](../../t-sql/language-elements/if-else-transact-sql.md) 和 [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md)。

**生成依赖对象的脚本**  
为在执行所选对象的脚本时需要的附加对象生成脚本。 默认值为 False。  
  
**架构限定对象名称**  
使用对象架构限定对象名称。 默认值为 False。 有关详细信息，请参阅 [创建数据库架构](../../relational-databases/security/authentication-access/create-a-database-schema.md)。  

**脚本数据压缩选项** 在脚本中包含数据压缩选项。 默认值为 False。

**编写扩展属性脚本**  
如果对象具有扩展属性，则在脚本中包含扩展属性。 默认值为 False。 有关详细信息，请参阅 [sp_addextendedproperty (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql.md)。  
  
**编写所有者脚本**  
在生成的脚本中包含所有者。 默认值为 False。  
  
**编写权限脚本**  
在脚本中包括数据库对象的权限。 默认值为 True。 有关详细信息，请参阅 [权限](../../relational-databases/security/permissions-database-engine.md)。  
  
### <a name="tableview-options"></a>表/视图选项  
以下选项仅应用于表或视图的脚本。  
  
**将用户定义数据类型转换为基类型**  
将用户定义数据类型转换为用于创建此用户定义数据类型的基类型。 将运行脚本的数据库中不存在源数据库用户定义数据类型时，请使用 True。 使用 False 可以保留用户定义数据类型。 默认值为 False。 有关详细信息，请参阅 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)。  
  
**生成 SET ANSI PADDING 命令**  
将 SET ANSI_PADDING 语句添加在每条 CREATE TABLE 语句的前后。 默认值为 True。 有关详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
**包含排序规则**  
在列定义中包含排序规则。 默认值为 True。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
**包含 IDENTITY 属性**  
包含 IDENTITY 种子和 IDENTITY 增量的定义。 默认值为 True。 有关详细信息，请参阅 [IDENTITY (Property) (Transact-SQL)](../../t-sql/statements/create-table-transact-sql-identity-property.md)。  
  
**架构限定外键引用**  
将架构名称添加到 FOREIGN KEY 约束的表引用。 默认值为 True。  
  
**绑定到脚本的默认值和规则**  
包括 **sp_bindefault** 和 **sp_bindrule** 绑定存储过程调用。 默认值为 True。 有关详细信息，请参阅 [sp_bindefault (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md) 和 [sp_bindrule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-bindrule-transact-sql.md)。  
  
**编写检查约束脚本**  
将 [CHECK 约束](../../relational-databases/tables/unique-constraints-and-check-constraints.md) 添加到脚本中。 默认值为 True。  
  
**编写默认值脚本**  
在脚本中包含列默认值。 默认值为 False。 有关详细信息，请参阅 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)。  
  
**编写文件组脚本**  
在 ON 子句中为表定义指定文件组。 默认值为 False。 有关详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
**编写外键脚本**  
在脚本中包含 [FOREIGN KEY 约束](../../relational-databases/tables/primary-and-foreign-key-constraints.md) 。 默认值为 False。  
  
**编写全文检索脚本**  
在脚本中包含全文检索。 默认值为 False。 有关详细信息，请参阅 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)。  
  
**编写索引脚本**  
在脚本中包含聚集索引、非聚集索引和 XML 索引。 默认值为 True。 有关详细信息，请参阅 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)。  
  
**编写分区方案脚本**  
在脚本中包含表分区方案。 默认值为 False。 有关详细信息，请参阅 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)。  
  
**编写主键脚本**  
在脚本中包含 [和 FOREIGN KEY 约束](../../relational-databases/tables/primary-and-foreign-key-constraints.md) 。 默认值为 True。  
  
**编写统计信息脚本**  
在脚本中包含用户定义统计信息。 默认值为 False。 有关详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
**编写触发器脚本**  
在脚本中包含触发器。 默认值为 False。 有关详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
**编写唯一键脚本**  
在脚本中包含 [UNIQUE 约束和 CHECK 约束](../../relational-databases/tables/unique-constraints-and-check-constraints.md) 。 默认值为 False。  
  
**编写视图列脚本**  
在视图页眉中声明视图列。 默认值为 False。 有关详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。  
  
**包括 dri 系统名称**  
包含系统生成的约束名称，以强制声明性引用完整性。 默认值为 False。 有关详细信息，请参阅 [REFERENTIAL_CONSTRAINTS (Transact-SQL)](../../relational-databases/system-information-schema-views/referential-constraints-transact-sql.md)。  
  
### <a name="version-options"></a>版本选项

**将脚本设置与源进行匹配** 如果启用，则将生成的脚本的目标版本、引擎版本和引擎类型设置为要脚本化的对象的服务器的值。 这将禁用（并忽略）其他版本选项。 

**数据库引擎版本的脚本** 生成的脚本将面向指定的 [引擎版本](/dotnet/api/microsoft.sqlserver.management.smo.edition)。

**数据库引擎类型的脚本** 生成的脚本将面向指定的 [数据库引擎类型](/previous-versions/sql/sql-server-2014/ee642509(v=sql.120))。

**为服务器版本编写脚本**  
生成的脚本将面向指定版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 无法为早期版本编写 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 新增功能的脚本。 某些为 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 创建的脚本无法在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]早期版本的服务器或具有早期 [数据库兼容级别设置](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)的数据库中运行。  

## <a name="see-also"></a>另请参阅  
[生成脚本 (SQL Server Management Studio)](../scripting/generate-scripts-sql-server-management-studio.md)  
