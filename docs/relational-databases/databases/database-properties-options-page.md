---
title: 数据库属性（“选项”页）| Microsoft Docs
description: 了解如何使用“数据库属性”对话框中的“选项”选项卡来查看或修改数据库的排序规则、恢复模式和其他设置。
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.options.f1
ms.assetid: a3447987-5507-4630-ac35-58821b72354d
author: stevestein
ms.author: sstein
ms.openlocfilehash: 87c3b10b95a37c74ea1595dca15341aeffe5c261
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031096"
---
# <a name="database-properties-options-page"></a>数据库属性（“选项”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用此页可以查看或修改所选数据库的选项。 有关此页上可用选项的详细信息，请参阅 [ALTER DATABASE SET 选项 (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md) 和 [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)  
  
## <a name="page-header"></a>页眉  
 **排序规则**  
 通过从列表中进行选择来指定数据库的排序规则。 有关详细信息，请参阅 [Set or Change the Database Collation](../../relational-databases/collations/set-or-change-the-database-collation.md)。  
  
 **恢复模式**  
 指定下列模式之一来恢复数据库：**完整**、**大容量日志** 或 **简单**。 有关恢复模式的详细信息，请参阅[恢复模式 (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)。  
  
 **兼容性级别**  
 指定数据库支持的最新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本。 有关可能的值，请参阅 [ALTER DATABASE (Transact-SQL) 兼容性级别](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)。 升级 SQL Server 数据库时，如果可能，保留该数据库的兼容性级别，或更改为新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的最低级别。 
  
 **包含类型**  
 指定无或部分以便指定这是否为包含数据库。 有关包含的数据库的详细信息，请参阅 [Contained Databases](../../relational-databases/databases/contained-databases.md)。 服务器属性 **“启用包含的数据库”** 必须首先设置为 **TRUE** ，然后才能将某个数据库配置为包含数据库。  
  
> [!IMPORTANT]  
>  启用部分包含数据库会将对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问控制委托给该数据库的所有者。 有关详细信息，请参阅 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
## <a name="automatic"></a>自动  
 **自动关闭**  
 指定在上一个用户退出后，数据库是否完全关闭并释放资源。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则在上一个用户注销之后，数据库会完全关闭并释放其资源。  

 自动创建增量统计信息  
 指定在创建每个分区的统计信息时是否使用增量选项。 有关增量统计信息的信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
 **自动创建统计信息**  
 指定数据库是否自动创建缺少的优化统计信息。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则将在优化过程中自动生成优化查询需要但缺少的所有统计信息。 有关详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
 **自动收缩**  
 指定数据库文件是否可定期收缩。 可能的值包括 **True** 和 **False**。 有关详细信息，请参阅 [Shrink a Database](../../relational-databases/databases/shrink-a-database.md)。  
  
 **自动更新统计信息**  
 指定数据库是否自动更新过期的优化统计信息。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则将在优化过程中自动生成优化查询需要但已过期的所有统计信息。 有关详细信息，请参阅 [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)。  
  
 **自动异步更新统计信息**  
 如果设置为“True”，则启动自动更新过期统计信息的查询在编译前不会等待统计信息被更新。 后续查询会在更新统计信息可用时使用。  
  
 如果设置为 **False**，则启动自动更新过期统计信息的查询将等待，直到更新的统计信息可在查询优化计划中使用。  
  
 将该选项设置为 **True** 不会产生任何影响，除非 **“自动更新统计信息”** 也设置为 **True**。  

## <a name="azure"></a>Azure
如果连接到 Azure SQL 数据库，此部分具有控制服务级别目标 (SLO) 的设置。 新数据库的默认 SLO 为标准 S2。

  **当前服务级别目标** 要使用的特定 SLO。 有效值受所选版本约束。 如果列表中没有所需的 SLO 值，则可键入值。

  **版本** 要使用的 Azure SQL 数据库版本，例如“基本”或“高级”。 如果列表中没有所需的版本值，则可键入值，该值必须与 Azure REST API 中使用的值匹配。
  
  **最大大小** 数据库的最大大小。 如果列表中没有所需的大小值，则可键入值。 保留空白则会使用给定版本和 SLO 的默认大小。
  
## <a name="containment"></a>Containment  
 在包含数据库中，可以在数据库级别配置通常在服务器级别配置的某些设置。  
  
 **默认全文语言 LCID**  
 指定全文检索列的默认语言。 全文检索数据的语言分析取决于数据的语言。 该选项的默认值为服务器的语言。 有关与所显示设置相对应的语言，请参阅 [sys.fulltext_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。  
  
 **默认语言**  
 所有新包含数据库用户的默认语言，除非另行指定。  
  
 **启用嵌套的触发器**  
 允许触发器激发其他触发器。 触发器最多可以嵌套 32 级。 有关详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md) 中的“嵌套触发器”部分。  
  
 **转换干扰词**  
 取消干扰词（即非索引字）导致全文查询的布尔操作返回零行时所产生的错误消息。 有关详细信息，请参阅 [transform noise words Server Configuration Option](../../database-engine/configure-windows/transform-noise-words-server-configuration-option.md)。  
  
 **两位数年份截止**  
 指示可作为两位数年份输入的最高年数。 可将所列年份及其之前的 99 年作为两位数年份输入。 所有其他年份必须作为四位数年份输入。  
  
 例如，2049 的默认设置表明：作为 '3/14/49' 输入的日期将被解释为 2049 年 3 月 14 日，而作为 '3/14/50' 输入的日期则将被解释为 1950 年 3 月 14 日。 有关详细信息，请参阅 [配置两位数年份截止服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。  
  
## <a name="cursor"></a>游标  
 **提交时关闭游标功能已启用**  
 指定在提交了打开游标的事务之后是否关闭游标。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则会关闭在提交或回滚事务时打开的游标。 如果设置为 **False**，则这些游标会在提交事务时保持打开状态。 如果设置为 **False**，则在回滚事务时会关闭所有游标（那些定义为 INSENSITIVE 或 STATIC 的游标除外）。 有关详细信息，请参阅 [SET CURSOR_CLOSE_ON_COMMIT (Transact-SQL)](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)。  
  
 **默认游标**  
 指定默认的游标行为。 如果设置为 **True**，则游标声明默认为 LOCAL。 如果设置为 **False**，则 [!INCLUDE[tsql](../../includes/tsql-md.md)] 游标默认为 GLOBAL。  
  
## <a name="database-scoped-configurations"></a>数据库作用域配置  
 在 SQL Server 2016 和 Azure SQL 数据库中，有多个可以作用于数据库级别作用域的配置属性。 有关所有这些设置的详细信息，请参阅 [ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。  
  
 **旧版基数估计**  
 指定独立于数据库兼容级别的主查询优化器基数估计模型。 这与 [跟踪标志 9481](https://support.microsoft.com/kb/2801413)是等效的。  
  
 **辅助数据库的旧版基数估计**  
 指定独立于数据库兼容级别的辅助查询优化器基数估计模型。 这与 [跟踪标志 9481](https://support.microsoft.com/kb/2801413)是等效的。  
  
 **Max DOP**  
 指定应用于语句的主默认 [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) 设置。  
  
 **辅助 Max DOP**  
 指定应用于语句的辅助默认 [MAXDOP](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) 设置（如有）。  
  
 **参数截取**  
 启用或禁用主参数截取。 这与 [Trace Flag 4136](https://support.microsoft.com/kb/980653)是等效的。  
  
 **辅助参数截取**  
 启用或禁用辅助参数截取。 这与 [Trace Flag 4136](https://support.microsoft.com/kb/980653)是等效的。  
  
 **查询优化器修复程序**  
 启用或禁用主查询优化修补程序，而不考虑数据库的兼容级别。 这与 [Trace Flag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)是等效的。 有关详细信息，请参阅 [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes)。  
  
 **辅助查询优化器修补程序**  
 启用或禁用辅助查询优化修补程序（如有），而不考虑数据库的兼容级别。 这与 [Trace Flag 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)是等效的。 有关详细信息，请参阅 [QUERY_OPTIMIZER_HOTFIXES](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#qo_hotfixes)。  
  
## <a name="filestream"></a>FILESTREAM  
 **FILESTREAM 目录名称**  
 为与所选数据库相关联的 FILESTREAM 数据指定目录名称。  
  
 **FILESTREAM 非事务访问**  
 为从文件系统到 FileTables 中存储的 FILESTREAM 数据的非事务性访问指定以下选项之一：**OFF**、**READ_ONLY** 或 **FULL**。 如果在服务器上未启用 FILESTREAM，则该值将设置为 OFF 并且被禁用。 有关详细信息，请参阅 [FileTables (SQL Server)](../../relational-databases/blob/filetables-sql-server.md)。  
  
## <a name="miscellaneous"></a>杂项  
允许快照隔离  
启用此功能。  

 **ANSI NULL 默认值**  
 **CREATE TABLE** 或 **ALTER TABLE** 语句执行过程中，没有显式定义为 **NOT NULL** 的所有用户定义的数据类型或列都将默认为允许空值。 有关详细信息，请参阅 [SET ANSI_NULL_DFLT_ON (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md) 和 [SET ANSI_NULL_DFLT_OFF (Transact-SQL)](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)。  
  
 **ANSI NULLS 已启用**  
 指定等于 (`=`) 和不等于 (`<>`) 比较运算符在与 Null 值一起使用时的行为。 可能的值包括 **True** （开）和 **False** （关）。 如果设置为 **True**，则所有与 Null 值的比较求得的值均为 UNKNOWN。 如果设置为 **False**，则非 UNICODE 值与 null 值比较求得的值为 **True**（如果这两个值均为 NULL）。 有关详细信息，请参阅 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)。  
  
 **ANSI 填充已启用**  
 指定 ANSI 填充状态是开还是关。 可能的值为 **True**（开）和 **False**（关）。 有关详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
 **ANSI 警告已启用**  
 对于几种错误条件指定 ISO 标准行为。 如果设置为 **True**，则会在聚合函数（如 SUM、AVG、MAX、MIN、STDEV、STDEVP、VAR、VARP 或 COUNT）中出现 null 值时生成警告消息。 如果设置为 **False**，则不会发出任何警告。 有关详细信息，请参阅 [SET ANSI_WARNINGS (Transact-SQL)](../../t-sql/statements/set-ansi-warnings-transact-sql.md)。  
  
 **算术中止已启用**  
 指定是否启用数据库的算术中止选项。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则溢出错误或被零除错误会导致查询或批处理终止。 如果错误发生在事务内，则回滚事务。 如果设置为 **False**，则会显示一条警告消息，但是会继续执行查询、批处理或事务，就像没有出错一样。 有关详细信息，请参阅 [SET ARITHABORT (Transact-SQL)](../../t-sql/statements/set-arithabort-transact-sql.md)。  
  
 **串联的 Null 结果为 Null**  
 指定在与 Null 值连接时的行为。 当属性值为 **True** 时， **字符串** + NULL 会返回 NULL。 如果设置为 **False**，则结果为 **string**。 有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。  
  
 **跨数据库所有权链接已启用**  
 该只读值指示跨数据库所有权链接是否已启用。 如果设置为 **True**，则数据库可以为跨数据库所有权链的源或目标。 使用 ALTER DATABASE 语句设置此属性。  
  
 **日期相关性优化已启用**  
 如果设置为 **True**，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 维护数据库中由 FOREIGN KEY 约束所链接并包含 **datetime** 列的任意两个表之间的相关统计信息。  
  
 如果设置为 **False**，则不维护相关统计信息。  
 
 延迟持续性  
 启用此功能。 有关详细信息，请参阅[控制事务持续性](../logs/control-transaction-durability.md)。
 
 读提交快照处于打开状态  
 启用此功能。  
 
 **数值舍入中止**  
 指定数据库处理舍入错误的方式。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则当表达式出现精度降低的情况时生成错误。 如果设置为 **False**，则在精度降低时不生成错误消息，并按存储结果的列或变量的精度对结果进行四舍五入。 有关详细信息，请参阅 [SET NUMERIC_ROUNDABORT (Transact-SQL)](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)。  
  
 **参数化**  
 如果设置为 **SIMPLE**，则基于数据库的默认行为使查询参数化。 如果设置为 **FORCED**，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使数据库中的所有查询参数化。  
  
 **允许带引号的标识符**  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关键字在用引号引起来时是否可以用作标识符（对象名或变量名）。 可能的值包括 **True** 和 **False**。 有关详细信息，请参阅 [SET QUOTED_IDENTIFIER (Transact-SQL)](../../t-sql/statements/set-quoted-identifier-transact-sql.md)。  
  
 **递归触发器已启用**  
 指定触发器是否可以由其他触发器激发。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则会启用对触发器的递归激发。 如果设置为 **False**，则只禁用直接递归。 若要禁用间接递归，请使用 sp_configure 将 nested triggers 服务器选项设置为 0。 有关详细信息，请参阅 [创建嵌套触发器](../../relational-databases/triggers/create-nested-triggers.md)。  
  
 **可信**  
 当显示 **True** 时，该只读选项指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 允许在数据库中建立的模拟上下文内访问数据库以外的资源。 可以使用 EXECUTE AS 用户语句或数据库模块上的 EXECUTE AS 子句在数据库内建立模拟上下文。  
  
 若要具有访问权限，数据库的所有者也需要具有服务器级的 AUTHENTICATE SERVER 权限。  
  
 使用此属性，还可以在数据库内创建和执行不安全的程序集和外部访问程序集。 除了将此属性设置为 **True** 以外，数据库的所有者还必须拥有服务器级的 EXTERNAL ACCESS ASSEMBLY 或 UNSAFE ASSEMBLY 权限。  
  
 默认情况下，所有用户数据库和所有系统数据库（ **MSDB** 除外）都将此属性设置为 **False**。 对于 **model** 和 **tempdb** 数据库，不能更改此值。  
  
 每当数据库附加到服务器时，都要将 TRUSTWORTHY 设置为 **False** 。  
  
 在模拟上下文内访问数据库以外资源的建议方法是使用证书和签名替代 **Trustworthy** 选项。  
  
 若要设置此属性，请使用 ALTER DATABASE 语句。  
  
 **VarDecimal 存储格式已启用**  
 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]开始，此选项为只读选项。 为 **True** 时，此数据库将启用 vardecimal 存储格式。 数据库中的任何表使用 vardecimal 存储格式时，无法禁用该存储格式。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中，对于所有的用户数据库都将启用 vardecimal 存储格式。 此选项使用 [sp_db_vardecimal_storage_format](../../relational-databases/system-stored-procedures/sp-db-vardecimal-storage-format-transact-sql.md)。  
  
## <a name="recovery"></a>恢复  
 **页验证**  
 指定用于发现和报告由磁盘 I/O 错误导致的不完整 I/O 事务的选项。 可能的值为 **None**、 **TornPageDetection** 和 **Checksum**。 有关详细信息，请参阅 [管理 suspect_pages 表 (SQL Server)](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)在  中还原页。  
  
 **目标恢复时间（秒）**  
 指定在发生崩溃的情况下恢复指定数据库的最长时间（秒）。 有关详细信息，请参阅[数据库检查点 (SQL Server)](../../relational-databases/logs/database-checkpoints-sql-server.md)。  

## <a name="service-broker"></a>Service Broker  
启用 Broker  
启用或禁用 Service Broker。  

优先处理 Broker 优先级  
只读的 Service Broker 属性。  

Service Broker 标识符  
只读的标识符。  

## <a name="state"></a>状态  
 **数据库为只读**  
 指定数据库是否为只读。 可能的值包括 **True** 和 **False**。 如果设置为 **True**，则用户只能读取数据库中的数据。 用户不能修改数据或数据库对象；然而，数据库本身可以通过使用 `DROP DATABASE` 语句自行删除。 在为 **“数据库为只读”** 选项指定新值时，数据库不能处于使用状态。 master 数据库是个例外，在设置该选项时，只有系统管理员才能使用 master 数据库。  
  
 **数据库状态**  
 查看数据库的当前状态。 该策略不能编辑。 有关 **“数据库状态”** 的详细信息，请参阅 [Database States](../../relational-databases/databases/database-states.md)。  

 **已启用加密**  
 设置为 **True** 时，会对此数据库启用数据库加密。 加密时需要数据库加密密钥。 有关详细信息，请参阅[透明数据加密 (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md)。  
 
 **限制访问**  
 指定哪些用户可以访问该数据库。 可能的值包括：  
  
-   **多个**  
  
     生产数据库的正常状态，允许多个用户同时访问该数据库。  
  
-   **Single**  
  
     用于维护操作，一次只允许一个用户访问该数据库。  
  
-   **Restricted**  
  
     只有 db_owner、dbcreator 或 sysadmin 角色的成员才能使用该数据库。  
  


## <a name="see-also"></a>另请参阅  
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)  
