---
title: 所需的权限
titleSuffix: SQL Server Profiler
description: 了解运行 SQL Server Profiler 和重播跟踪所需的权限，以及在重播期间执行的检查。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: profiler
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 13f7b1044d5a83d9042e78de1cad1a2cf54aa790
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172447"
---
# <a name="permissions-required-to-run-sql-server-profiler"></a>运行 SQL Server Profiler 所需的权限

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

默认情况下，用户运行 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 所需的权限与执行用于创建跟踪的 Transact-SQL 存储过程所需的权限相同。 若要运行 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]，用户必须拥有 ALTER TRACE 权限。 有关详细信息，请参阅 [GRANT 服务器权限 (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)。

> [!IMPORTANT]
> 拥有 SHOWPLAN、ALTER TRACE 或 VIEW SERVER STATE 权限的用户可以对显示计划输出中捕获的查询进行查看。 这些查询可能包含敏感信息，例如密码。 因此，建议您仅将这些权限授予有权查看敏感信息的一类用户，例如 db_owner 固定数据库角色的成员或 sysadmin 固定服务器角色的成员。 此外，建议您最好将包含显示计划相关事件的显示计划文件或跟踪文件保存到使用 NTFS 文件系统的某个位置，并且只允许有权查看敏感信息的用户对之进行访问。

> [!IMPORTANT]
> 已弃用 SQL 跟踪和 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。 包含 Microsoft SQL Server 跟踪和重播对象的“Microsoft.SqlServer.Management.Trace”命名空间也已遭弃用。
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]
> 请改用扩展事件。 有关[扩展事件](../../relational-databases/extended-events/extended-events.md)的详细信息，请参阅[快速入门：SQL Server 中的扩展事件](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md)和 [SSMS XEvent 探查器](../../relational-databases/extended-events/use-the-ssms-xe-profiler.md)。

> [!NOTE]
> 不支持针对 Analysis Services 工作负载的 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]。

> [!NOTE]
> 尝试从 SQL Server Profiler 连接到 Azure SQL 数据库时，会错误地引发误导性错误消息，如下所示：
>
> - 若要对 SQL Server 运行跟踪，你必须是 sysadmin 固定服务器角色的成员或拥有 ALTER TRACE 权限。
>
> 该消息应说明了 SQL Server Profiler 不支持 Azure SQL 数据库。

## <a name="permissions-used-to-replay-traces"></a>用于重播跟踪的权限  
重播跟踪也要求重播跟踪的用户拥有 ALTER TRACE 权限。  

但是，如果重播期间在重播的跟踪中遇到 Audit Login 事件， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将使用 EXECUTE AS 命令。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 使用 EXECUTE AS 命令模拟与登录事件关联的用户。  

如果 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 在重播的跟踪中遇到登录事件，将执行下列权限检查：

1. 拥有 ALTER TRACE 权限的用户 1 开始重播跟踪。

2. 在重播的跟踪中遇到用户 2 的登录事件。

3. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 使用 EXECUTE AS 命令模拟用户 2。

4. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 尝试验证用户 2 的身份，根据结果的不同会出现下列情况之一：

    1. 如果用户 2 无法通过身份验证， [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将返回一个错误，并以用户 1 的身份继续重播跟踪。
  
    2. 如果用户 2 成功通过身份验证，将以用户 2 的身份继续重播跟踪。
  
5. 检查用户 2 对目标数据库的权限，根据结果的不同会出现下列情况之一：
  
    1. 如果用户 2 拥有对目标数据库的权限，则模拟成功，并以用户 2 的身份重播跟踪。
  
    2. 如果用户 2 不拥有对目标数据库的权限，则服务器将检查该数据库的 Guest 用户。

6. 将检查目标数据库中是否存在 Guest 用户，根据结果的不同会出现下列情况之一：
 
    1.  如果 Guest 帐户存在，将以 Guest 帐户重播跟踪。
  
    2.  如果目标数据库中不存在 Guest 帐户，将返回一个错误，并以用户 1 的身份重播跟踪。
 
以下关系图说明了重播跟踪时此检查权限的过程：

![SQL Server Profiler 重播跟踪权限。](../../tools/sql-server-profiler/media/replaytracedecisiontree.gif)

## <a name="see-also"></a>另请参阅
- [SQL Server Profiler 存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)
- [重播跟踪](../../tools/sql-server-profiler/replay-traces.md)
- [创建跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)
- [重播跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-table-sql-server-profiler.md)
- [重播跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/replay-a-trace-file-sql-server-profiler.md)
