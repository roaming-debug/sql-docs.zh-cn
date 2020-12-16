---
description: SET 语句 (Transact-SQL)
title: SET 语句 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SET
- SET_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ISO SET statements
- queries [SQL Server], executing
- dates [SQL Server], SET statements
- time [SQL Server], SET statements
- SET statement, about SET statement
- SET statement
- statistical information [SQL Server], SET statements
- locking [SQL Server], SET statements
ms.assetid: f7e107f8-0fcf-408b-b30f-da2323eeb714
author: markingmyname
ms.author: maghan
monikerRange: = azure-sqldw-latest ||= azuresqldb-current || >= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 7ee970fa48cfe96e5c8c233a2691184f6801593d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463890"
---
# <a name="set-statements-transact-sql"></a>SET 语句 (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

[!INCLUDE[tsql](../../includes/tsql-md.md)] 编程语言提供了一些 SET 语句，这些语句可以更改特定信息的当前会话处理。 SET 语句可分为下表中列出的几个类别。  
  
有关使用 SET 语句设置局部变量的信息，请参阅 [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)。  
  
|类别|语句|  
|--------------|----------------|  
|日期和时间语句|[SET DATEFIRST](../../t-sql/statements/set-datefirst-transact-sql.md)<br /><br /> [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md)|  
|锁定语句|[SET DEADLOCK_PRIORITY](../../t-sql/statements/set-deadlock-priority-transact-sql.md)<br /><br /> [SET LOCK_TIMEOUT](../../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|杂项语句|[SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)<br /><br /> [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md)<br /><br /> [SET FIPS_FLAGGER](../../t-sql/statements/set-fips-flagger-transact-sql.md)<br /><br /> [SET IDENTITY_INSERT](../../t-sql/statements/set-identity-insert-transact-sql.md)<br /><br /> [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md)<br /><br /> [SET OFFSETS](../../t-sql/statements/set-offsets-transact-sql.md)<br /><br /> [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md)|  
|查询执行语句|[SET ARITHABORT](../../t-sql/statements/set-arithabort-transact-sql.md)<br /><br /> [SET ARITHIGNORE](../../t-sql/statements/set-arithignore-transact-sql.md)<br /><br /> [SET FMTONLY](../../t-sql/statements/set-fmtonly-transact-sql.md)<br /> 请注意：[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]<br /><br /> [SET NOCOUNT](../../t-sql/statements/set-nocount-transact-sql.md)<br /><br /> [SET NOEXEC](../../t-sql/statements/set-noexec-transact-sql.md)<br /><br /> [SET NUMERIC_ROUNDABORT](../../t-sql/statements/set-numeric-roundabort-transact-sql.md)<br /><br /> [SET PARSEONLY](../../t-sql/statements/set-parseonly-transact-sql.md)<br /><br /> [SET QUERY_GOVERNOR_COST_LIMIT](../../t-sql/statements/set-query-governor-cost-limit-transact-sql.md)<br /><br /> [SET RESULT_SET_CACHING](../../t-sql/statements/set-result-set-caching-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)（预览版）<br /> 注意：此功能仅适用于 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]。<br /><br /> [SET ROWCOUNT](../../t-sql/statements/set-rowcount-transact-sql.md)<br /><br /> [SET TEXTSIZE](../../t-sql/statements/set-textsize-transact-sql.md)|  
|ISO 设置语句|[SET ANSI_DEFAULTS](../../t-sql/statements/set-ansi-defaults-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_OFF](../../t-sql/statements/set-ansi-null-dflt-off-transact-sql.md)<br /><br /> [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md)<br /><br /> [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md)<br /><br /> [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md)<br /><br /> [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md)|  
|统计语句|[SET FORCEPLAN](../../t-sql/statements/set-forceplan-transact-sql.md)<br /><br /> [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md)<br /><br /> [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md)<br /><br /> [SET SHOWPLAN_XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)<br /><br /> [SET STATISTICS IO](../../t-sql/statements/set-statistics-io-transact-sql.md)<br /><br /> [SET STATISTICS XML](../../t-sql/statements/set-statistics-xml-transact-sql.md)<br /><br /> [SET STATISTICS PROFILE](../../t-sql/statements/set-statistics-profile-transact-sql.md)<br /><br /> [SET STATISTICS TIME](../../t-sql/statements/set-statistics-time-transact-sql.md)|  
|事务语句|[SET IMPLICIT_TRANSACTIONS](../../t-sql/statements/set-implicit-transactions-transact-sql.md)<br /><br /> [SET REMOTE_PROC_TRANSACTIONS](../../t-sql/statements/set-remote-proc-transactions-transact-sql.md)<br /><br /> [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)<br /><br /> [SET XACT_ABORT](../../t-sql/statements/set-xact-abort-transact-sql.md)| 
  
## <a name="considerations-when-you-use-the-set-statements"></a>使用 SET 语句时的注意事项  
  
- 在执行过程中或运行时运行的所有 SET 语句，除了在分析时运行的这些语句之外：

  - SET FIPS_FLAGGER
  - SET OFFSETS
  - SET PARSEONLY
  - 和 SET QUOTED_IDENTIFIER  
  
- 如果是在存储过程或触发器中运行 SET 语句，则存储过程或触发器返回控制后，将恢复 SET 选项的值。 同样，如果是在使用 sp_executesql 或 EXECUTE 运行的动态 SQL 字符串中指定 SET 语句，则从在动态 SQL 字符串中指定的批处理返回控制后，将恢复 SET 选项的值。  
  
- 存储过程使用执行时指定的 SET 设置执行，但 SET ANSI_NULLS 和 SET QUOTED_IDENTIFIER 除外。 指定 SET ANSI_NULLS 或 SET QUOTED_IDENTIFIER 的存储过程使用创建存储过程时指定的设置。 如果在存储过程内使用任何 SET 设置，则该设置将被忽略。  
  
- sp_configure 的 user options 设置允许服务器范围的设置，并可以跨多个数据库运行。 此设置的行为还类似于显式 SET 语句，只是后者发生在登录时。  
  
- 使用 ALTER DATABASE 设置的数据库设置仅在数据库级有效，并且仅在显式设置时有效。 数据库设置优先于使用 sp_configure 设置的实例选项设置。  
  
- 如果 SET 语句使用 ON 和 OFF 时，则可为一个语句指定多个 SET 选项。
  
    > [!NOTE]  
    >  这不适用与统计相关的 SET 选项。
  
     例如，`SET QUOTED_IDENTIFIER, ANSI_NULLS ON` 可将 QUOTED_IDENTIFIER 和 ANSI_NULLS 设置为 ON。  
  
- SET 语句设置优先于使用 ALTER DATABASE 设置的等价数据库选项设置。 例如，SET ANSI_NULLS 语句中指定的值将覆盖 ANSI_NULL 的数据库设置。 此外，如果用户在连接到数据库时依据的值是因为先前使用 sp_configure user options 设置而生效的，或者所依据的值适用于所有 ODBC 和 OLE/DB 连接，则一些连接设置将自动设置为 ON。  
  
- ALTER、CREATE 和 DROP DATABASE 语句不提供 SET LOCK_TIMEOUT 设置。  
  
- 当全局或快捷 SET 语句设置多个设置时，发出快捷 SET 语句后，将重设所有受该快捷 SET 语句影响的选项的先前设置。 如果在发出快捷 SET 语句后设置了受快捷 SET 语句影响的 SET 选项，则该单个 SET 语句将覆盖相应的快捷设置。 快捷 SET 语句的示例将为 SET ANSI_DEFAULTS。  
  
- 使用批处理时，数据库上下文由使用 USE 语句建立的批处理决定。 在存储过程的外部运行的以及批处理中的计划外查询和所有其他语句，将继承通过 USE 语句建立的数据库和连接的选项设置。  
  
- 多个活动的结果集 (MARS) 请求共享一个全局状态，该状态包含最新会话 SET 选项设置。 每个请求执行时都可以修改 SET 选项。 更改特定于设置这些更改所在的请求上下文，不影响其他并发 MARS 请求。 但是，请求执行完成后，新的 SET 选项会被复制到全局会话状态。 在此更改之后，同一会话中执行的新请求将使用这些新的 SET 选项设置。  
  
- 当从批处理或其他存储过程运行某个存储过程时，运行该存储过程时使用的选项值，就是在包含该存储过程的数据库中设置的选项值。 例如，存储过程 db1.dbo.sp1 调用存储过程 db2.dbo.sp2 时，执行存储过程 sp1 时将使用数据库 db1 的当前兼容级别设置，执行存储过程 sp2 时将使用数据库 db2 的当前兼容级别设置。  
  
- 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句关注的对象驻留在多个数据库中，则将对该语句应用当前数据库上下文和当前连接上下文。 在这种情况下，如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在批处理中，则当前连接上下文是 USE 语句定义的数据库；如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句在存储过程中，则连接上下文是包含该存储过程的数据库。  
  
- 针对计算列或索引视图创建并操作索引时，必须将这些 SET 选项设置为 ON：ARITHABORT、CONCAT_NULL_YIELDS_NULL、QUOTED_IDENTIFIER、ANSI_NULLS、ANSI_PADDING 和 ANSI_WARNINGS。 将选项 NUMERIC_ROUNDABORT 设置为 OFF。  
  
  如果未将以上任一选项设置为要求的值，则对索引视图或带计算列索引的表进行 INSERT、UPDATE、DELETE、DBCC CHECKDB 和 DBCC CHECKTABLE 操作时将失败。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将发出一个错误，并列出所有设置不正确的选项。 同时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将对这些表或索引视图运行 SELECT 语句，就好像计算列或视图中不存在索引。 

- 如果 SET RESULT_SET_CACHING 设置为 ON，它为当前客户端会话启用结果集缓存功能。   如果已在数据库级别将 Result_set_caching 设置为 OFF，就无法为会话将它设置为 ON。    如果 SET RESULT_SET_CACHING 设置为 OFF，它为当前客户端会话禁用结果集缓存功能。 必须有公共角色的成员身份，才能更改此设置。
适用对象：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] Gen2
