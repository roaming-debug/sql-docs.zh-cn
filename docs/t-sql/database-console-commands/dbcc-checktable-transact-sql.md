---
description: DBCC CHECKTABLE (Transact-SQL)
title: DBCC CHECKTABLE (Transact-SQL) | Microsoft Docs
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKTABLE_TSQL
- DBCC_CHECKTABLE_TSQL
- DBCC CHECKTABLE
- CHECKTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], DBCC CHECKTABLE
- page integrity checks [SQL Server]
- consistency [SQL Server], tables
- consistency [SQL Server], indexed views
- DBCC CHECKTABLE statement
- integrity [SQL Server]
- low overhead checks
- table integrity checks [SQL Server]
ms.assetid: 0d6cb620-eb58-4745-8587-4133a1b16994
author: pmasl
ms.author: umajay
ms.openlocfilehash: 78a84099b202ca55588e1365b27d80004d2cdaa5
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172609"
---
# <a name="dbcc-checktable-transact-sql"></a>DBCC CHECKTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

检查组成表或索引视图的所有页和结构的完整性。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
    
## <a name="syntax"></a>语法    
    
```syntaxsql
DBCC CHECKTABLE     
(    
    table_name | view_name    
    [ , { NOINDEX | index_id }    
     |, { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD }     
    ]     
)    
    [ WITH     
        { [ ALL_ERRORMSGS ]    
          [ , EXTENDED_LOGICAL_CHECKS ]     
          [ , NO_INFOMSGS ]    
          [ , TABLOCK ]     
          [ , ESTIMATEONLY ]     
          [ , { PHYSICAL_ONLY | DATA_PURITY } ]     
          [ , MAXDOP = number_of_processors ]    
        }    
    ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 table_nameview_name |   
 要进行完整性检查的表或索引视图。 表名或视图名必须符合有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
    
NOINDEX  
 指定不应对用户表的非聚集索引执行会占用很大系统开销的检查。 这将减少总执行时间。 NOINDEX 不会影响系统表，因为完整性检查的执行对象始终是所有系统表索引。  
    
 index_id  
 要进行完整性检查的索引标识 (ID) 号。 如果指定了 index_id，则 DBCC CHECKTABLE 只对该索引以及堆或聚集索引执行完整性检查。  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 指定 DBCC CHECKTABLE 修复发现的错误。 若要使用修复选项，数据库必须处于单用户模式。  
    
REPAIR_ALLOW_DATA_LOSS  
 尝试修复报告的所有错误。 这些修复可能会导致一些数据丢失。  
    
REPAIR_FAST  
 保留语法只是为了向后兼容。 未执行修复操作。  
    
REPAIR_REBUILD  
 执行不会丢失数据的修复。 这包括快速修复（如修复非聚集索引中缺少的行）以及更耗时的修复（如重新生成索引）。  
 此参数不修复涉及 FILESTREAM 数据的错误。  
    
 > [!NOTE]  
 > 仅将 REPAIR 选项作为最后手段使用。 若要修复错误，建议您通过备份进行还原。 修复操作不会考虑表本身或表之间可能存在的任何约束。 如果指定的表与一个或多个约束有关，建议在修复操作后运行 `DBCC CHECKCONSTRAINTS`。
 > 如果必须使用 REPAIR，则运行不带有修复选项的 `DBCC CHECKTABLE` 来查找要使用的修复级别。 如果要使用 REPAIR_ALLOW_DATA_LOSS 级别，建议在运行带有此选项的 `DBCC CHECKTABLE` 之前备份数据库。  
    
ALL_ERRORMSGS  
 显示不受限制的错误数。 默认情况下显示所有错误消息。 指定或省略此选项都不起作用。  
    
EXTENDED_LOGICAL_CHECKS  
 如果兼容性级别为 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) 或更高，则对索引视图、XML 索引和空间索引（如果存在）执行逻辑一致性检查。  
 有关详细信息，请参阅本主题后面[备注](#remarks)部分中的“对索引执行逻辑一致性检查”。  
    
NO_INFOMSGS  
 取消显示所有信息性消息。  
    
TABLOCK  
 可使 DBCC CHECKTABLE 获得一个共享表锁，而不使用内部数据库快照。 TABLOCK 可使 DBCC CHECKTABLE 在负荷较重的表上运行得更快，但 DBCC CHECKTABLE 运行时会减少表上可获得的并发性。  
    
ESTIMATEONLY  
 显示运行 DBCC CHECKTABLE 和所有其他指定选项时，所需的 tempdb 空间的估计大小。  
    
PHYSICAL_ONLY  
 限制为检查页、记录标头以及 B 树的物理结构的完整性。 此选项旨在以较低的开销检查表的物理一致性，同时，此项检查还可以检测可能危及用户数据安全的残缺页和常见的硬件故障。 DBCC CHECKTABLE 完全运行的时间可能比在早期版本中完全运行的时间长得多。 导致此现象发生的原因如下：  
 -   逻辑检查更加全面。  
 -   要检查的某些基础结构更为复杂。  
 -   引入了许多新的检查以包含新增功能。  
   
因此，使用 PHYSICAL_ONLY 选项可能会使 DBCC CHECKTABLE 在大型表上运行的时间短得多，所以对需要频繁检查的生产系统，建议使用 DBCC CHECKTABLE。 我们仍然建议定期执行 DBCC CHECKTABLE 的完整运行。 这些运行的执行频率取决于各业务和生产环境特定的因素。 PHYSICAL_ONLY 始终表示 NO_INFOMSGS，不能与任何一个修复选项一同使用。  
    
 > [!NOTE]  
 > 指定 PHYSICAL_ONLY 会导致 DBCC CHECKTABLE 跳过对 FILESTREAM 数据的所有检查。  
    
DATA_PURITY  
 使 DBCC CHECKTABLE 检查表中是否存在无效或越界的列值。 例如， DBCC CHECKTABLE 检测日期和时间值大于或小于 datetime 数据类型的可接受范围的列，或者小数位数或精度值无效的 decimal 或近似 numeric 数据类型列。  
 默认情况下将启用列值完整性检查，并且不需要使用 DATA_PURITY 选项。 对于从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本升级的数据库，您可以使用 DBCC CHECKTABLE WITH DATA_PURITY 查找和更正特定表中的错误；但是，默认情况下不会对该表启用列值检查，直到 DBCC CHECKDB WITH DATA_PURITY 在数据库中正确运行时为止。 然后，DBCC CHECKDB 和 DBCC CHECKTABLE 将默认检查列值完整性。  
 无法使用 DBCC 修复选项来纠正该选项所报告的验证错误。 若要了解如何手动更正这些错误，请参阅知识库文章 923247：[解决 SQL Server 2005 及更高版本中的 DBCC 错误 2570](https://support.microsoft.com/kb/923247)。  
 如果指定了 PHYSICAL_ONLY，则不执行列完整性检查。  
    
MAXDOP  
 **适用对象**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 和更高版本）。  
 
 对于语句，替代 sp_configure 的“max degree of parallelism”配置选项 。 MAXDOP 可以超出使用 sp_configure 配置的值。 如果 MAXDOP 超出使用资源调控器配置的值，则数据库引擎会使用资源调控器 MAXDOP 值（如 ALTER WORKLOAD GROUP (Transact-SQL) 中所述）。 当使用 MAXDOP 查询提示时，所有和 max degree of parallelism 配置选项一起使用的语义规则均适用。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。  
    
 > [!NOTE]  
 > 如果 MAXDOP 设置为零，服务器将选择最大并行度。  
    
## <a name="remarks"></a>备注    
    
> [!NOTE]    
> 若要对数据库中的每个表执行 DBCC CHECKTABLE，请使用 [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)。    
    
对于指定的表，DBCC CHECKTABLE 将检查以下内容：
-   是否已正确链接索引、行内、LOB 以及行溢出数据页。    
-   索引是否按照正确的顺序排列。    
-   各指针是否一致。    
-   每页上的数据是否合理（包括计算列）。    
-   页面偏移量是否合理。    
-   基表的每一行是否在每个非聚集索引中具有匹配的行，以及非聚集索引的每一行是否在基表中具有匹配的行。    
-   已分区表或索引的每一行是否都位于正确的分区中。    
-   使用 FILESTREAM 将 varbinary(max) 数据存储在文件系统中时，文件系统与表之间是否保持链接级一致性。    
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>对索引执行逻辑一致性检查    
对索引进行的逻辑一致性检查因数据库兼容级别而异，如下所示：
-   如果兼容性级别为 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) 或更高：    
    -   除非指定 NOINDEX，否则 DBCC CHECKTABLE 将对单个表及其所有非聚集索引同时执行物理和逻辑一致性检查。 但是，在默认情况下，仅对 XML 索引、空间索引和索引视图执行物理一致性检查。    
    -   如果指定了 WITH EXTENDED_LOGICAL_CHECKS，则将对索引视图、XML 索引和空间索引（如果存在）执行逻辑检查。 默认情况下，先执行物理一致性检查，然后执行逻辑一致性检查。 如果还指定了 NOINDEX，则仅执行逻辑检查。    
         这些逻辑一致性检查可对索引对象的内部索引表及其引用的用户表进行交叉检查。 为了查找外部行，将构造内部查询来对内部表和用户表的完整交集执行查询。 运行此查询可能会对性能产生很大影响，并且无法跟踪其进度。 因此，建议您仅在以下情况下才指定 WITH EXTENDED_LOGICAL_CHECKS：怀疑存在与物理损坏无关的索引问题，或者已关闭页级校验并且怀疑存在列级硬件损坏。    
    -   如果索引为筛选索引，DBCC CHECKDB 将执行一致性检查以验证索引项是否满足筛选谓词的要求。   
      
- 从 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 开始，不再默认对持久化计算列、UDT 列和筛选索引运行其他检查，以避免昂贵的表达式计算。 此更改显著减少了针对包含这些对象的数据库的 CHECKDB 持续时间。 但是，始终会完成这些对象的物理一致性检查。 仅当指定了 EXTENDED_LOGICAL_CHECKS 选项时，才会在 EXTENDED_LOGICAL_CHECKS 选项中已包含的逻辑检查（索引视图、XML 索引和空间索引）之外，执行表达式计算。
-  如果兼容级别为 90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) 或更低，则除非指定 NOINDEX，否则 DBCC CHECKTABLE 将对单个表或索引视图及其所有非聚集索引和 XML 索引同时执行物理和逻辑一致性检查。 不支持空间索引。
    
 **了解数据库的兼容性级别**    
[查看或更改数据库的兼容级别](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>内部数据库快照    
DBCC CHECKTABLE 使用内部数据库快照提供其执行这些检查必需的事务一致性。 有关详细信息，请参阅[查看数据库快照的稀疏文件大小 (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) 以及 [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md) 中的“DBCC 内部数据库快照使用情况”部分。
如果无法创建快照，或指定了 TABLOCK，则 DBCC CHECKTABLE 将获取一个共享表锁来获得所需的一致性。
    
> [!NOTE]    
> 如果对 tempdb 运行 DBCC CHECKTABLE，则必须获取一个共享表锁。 这是因为，为了提高性能，不允许对 tempdb 使用数据库快照。 这意味着，无法获得所需的事务一致性。    
    
## <a name="checking-and-repairing-filestream-data"></a>检查和修复 FILESTREAM 数据    
对数据库和表启用 FILESTREAM 后，便可选择将 varbinary(max) 二进制大型对象 (BLOB) 存储在文件系统中。 对在文件系统中存储 BLOB 的表使用 DBCC CHECKTABLE 时，DBCC 会检查文件系统与数据库之间的链接级一致性。
例如，如果表包含使用 FILESTREAM 属性的 varbinary(max) 列，则 DBCC CHECKTABLE 会检查文件系统目录和文件与表行、表列和列值之间是否存在一对一映射。 如果指定了 REPAIR_ALLOW_DATA_LOSS 选项，DBCC CHECKTABLE 便可修复损坏。 为了修复 FILESTREAM 损坏，DBCC 将删除缺少文件系统数据的任何表行，并将删除未映射到表行、表列或列值的任何目录和文件。
    
## <a name="checking-objects-in-parallel"></a>并行检查对象    
默认情况下，DBCC CHECKTABLE 对对象执行并行检查。 并行度由查询处理器自动确定。 最大并行度的配置方式与并行查询相同。 若要限制 DBCC 检查可使用的处理器的最大数目，请使用 [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)。 有关详细信息，请参阅 [配置 max degree of parallelism 服务器配置选项](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)。
通过使用跟踪标志 2528 可以禁用并行检查。 有关详细信息，请参阅[跟踪标志 (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。
    
> [!NOTE]    
> 在运行 DBCC CHECKTABLE 期间时，以字节顺序排列的用户定义类型列中存储的字节必须与用户定义类型值的计算序列相等。 否则，DBCC CHECKTABLE 例程将报告一致性错误。    
    
## <a name="understanding-dbcc-error-messages"></a>了解 DBCC 错误消息    
DBCC CHECKTABLE 命令完成后，将向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中写入一条消息。 如果 DBCC 命令成功执行，则消息指示成功完成以及命令运行的时间。 如果 DBCC 命令在完成检查之前由于错误而停止，则消息将指示命令已终止，并指示状态值和命令运行的时间。 下表列出并说明了此消息中可包含的状态值。
    
|状态|描述|    
|-----------|-----------------|    
|0|出现错误号 8930。 这指示导致 DBCC 命令终止的元数据损坏。|    
|1|出现错误号 8967。 存在一个内部 DBCC 错误。|    
|2|在紧急模式数据库修复过程中出错。|    
|3|这指示导致 DBCC 命令终止的元数据损坏。|    
|4|检测到断言或访问违规。|    
|5|出现终止了 DBCC 命令的未知错误。|    
    
## <a name="error-reporting"></a>错误报告    
只要 DBCC CHECKTABLE 检测到损坏错误，就会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] LOG 目录中创建一个微型转储文件 (`SQLDUMP*nnnn*.txt`)。 如果为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用了“功能使用情况数据收集”和“错误报告”功能，该文件将被自动转发给 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 。 收集的数据将用于改进 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能。
转储文件包含 DBCC CHECKTABLE 命令的结果以及其他诊断输出数据。 该文件拥有任意访问控制列表 (DACL)。 只有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户和 sysadmin 角色的成员有权进行访问。 默认情况下，sysadmin 角色包含 Windows BUILTIN\Administrators 组和本地管理员组的所有成员。 如果数据收集进程失败，DBCC 命令不会失败。
    
## <a name="resolving-errors"></a>纠正错误    
 如果 DBCC CHECKTABLE 报告了任何错误，那么，我们建议从数据库备份中还原数据库，而不是使用某个 REPAIR 选项来运行 REPAIR。 如果没有备份，则运行 REPAIR 也可以更正报告的错误。 要使用的修复选项在报告的错误的末尾处指定。 但是，使用 REPAIR_ALLOW_DATA_LOSS 选项更正错误可能需要删除一些页面（从而也删除了数据）。    
修复操作可以在用户事务下执行，以允许用户回滚所做的更改。 如果回滚修复，数据库仍会包含错误，因而必须通过备份进行还原。 全部修复完成后，请备份数据库。
    
## <a name="result-sets"></a>结果集    
DBCC CHECKTABLE 返回以下结果集。 如果您仅指定了表名或任意选项，则返回相同的结果集。
    
```
DBCC results for 'HumanResources.Employee'.    
There are 288 rows in 13 pages for object 'Employee'.    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
如果指定了 ESTIMATEONLY 选项，则 DBCC CHECKTABLE 将返回以下结果集：
    
```
Estimated TEMPDB space needed for CHECKTABLES (KB)     
--------------------------------------------------     
21    
(1 row(s) affected)    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
## <a name="permissions"></a>权限    
用户必须是表所有者，或者是 sysadmin 固定服务器角色、db_owner 固定数据库角色或 db_ddladmin 固定数据库角色的成员。    
    
## <a name="examples"></a>示例    
    
### <a name="a-checking-a-specific-table"></a>A. 检查特定表    
以下示例将检查 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `HumanResources.Employee` 表的数据页完整性。
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee');    
GO    
```    
    
### <a name="b-performing-a-low-overhead-check-of-the-table"></a>B. 以较低的开销检查表    
 以下示例将以较低的开销检查 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `Employee` 表。    
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee') WITH PHYSICAL_ONLY;    
GO    
```    
    
### <a name="c-checking-a-specific-index"></a>C. 检查特定索引    
 以下示例将检查通过访问 `sys.indexes` 获得的特定索引。    
    
```sql    
DECLARE @indid int;    
SET @indid = (SELECT index_id     
              FROM sys.indexes    
              WHERE object_id = OBJECT_ID('Production.Product')    
                    AND name = 'AK_Product_Name');    
DBCC CHECKTABLE ('Production.Product',@indid);    
```    
    
## <a name="see-also"></a>另请参阅    
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)     
[DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)    
    
  
