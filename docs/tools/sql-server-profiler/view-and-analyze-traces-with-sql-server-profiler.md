---
title: 查看和分析跟踪
titleSuffix: SQL Server Profiler
description: 了解如何使用 SQL Server Profiler 查看跟踪数据、查找特定事件、显示对象名称和解决问题。
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 17e821ca-a12e-4192-acc1-96765d9ae266
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: b95380a4a99e525cc97588b2051d1812c6e2d34a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338644"
---
# <a name="view-and-analyze-traces-with-sql-server-profiler"></a>使用 SQL Server Profiler 查看和分析跟踪

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

可以使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 查看跟踪中捕获的事件数据。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 显示基于定义的跟踪属性的数据。 分析 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据的一种方式是将数据复制到其他程序中，例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]如果跟踪中包括“文本”数据列，则优化顾问可以使用包含 SQL 批处理和远程过程调用 (RPC) 事件的跟踪文件。 为了确保捕获正确的事件和列以便与 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问一起使用，请使用随 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]一起提供的预定义优化模板。  
  
 使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]打开跟踪时，如果跟踪文件是由 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 或 SQL 跟踪系统存储过程创建的，则该文件不需要带 .trc 文件扩展名。  
  
> [!NOTE]  
>  [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 还可以读取 SQL 跟踪 .log 文件和通用 SQL 脚本文件。 打开不带 .log 文件扩展名的 SQL 跟踪 .log 文件（例如 trace.txt）时，应将文件格式指定为 **SQLTrace_Log** 。  
  
 您可以配置 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 日期和时间显示格式以便有助于跟踪分析。  
  
## <a name="troubleshooting-data"></a>排除数据故障  
 使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]时，您可以按 **“持续时间”** 、 **CPU**、 **“读”** 或 **“写”** 数据列将跟踪或跟踪文件分组来排除数据故障。 例如，您可以对性能差的查询或逻辑读取操作数特别高的查询进行数据故障排除。  
  
 通过将跟踪保存至表和使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询事件数据，可以找到其他信息。 例如，若要确定哪些 **SQL:BatchCompleted** 事件的等待时间过长，可执行：  
  
```  
SELECT  TextData, Duration, CPU  
FROM    trace_table_name  
WHERE   EventClass = 12 -- SQL:BatchCompleted events  
AND     CPU < (Duration * 1000)  
```  
  
> [!NOTE]  
>  服务器以微秒（10^-6 秒）为单位报告事件的持续时间，以毫秒（10^-3 秒）为单位报告事件占用的 CPU 时间。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 图形用户界面默认以毫秒为单位显示 **“持续时间”** 列，但是当跟踪保存到文件或数据库表中时，将以微秒为单位写入“持续时间”  列值。  
  
## <a name="displaying-object-names-when-viewing-traces"></a>查看跟踪时显示对象名称  
 如果你要显示对象名称而不是对象标识符（**对象 ID**），必须捕获“服务器名称”和“数据库 ID”数据列以及“对象名称”数据列。  
  
 如果您选择按 **“对象 ID”** 数据列分组，请确保先按 **“服务器名称”** 和 **“数据库 ID”** 数据列分组，然后按 **“对象 ID”** 数据列分组。 同样，如果您选择按 **“索引 ID”** 数据列分组，请确保先按 **“服务器名称”**、 **“数据库 ID”** 和 **“对象 ID”** 数据列分组，然后按 **“索引 ID”** 数据列分组。 您必须按照此顺序分组，因为对象 ID 和索引 ID 在服务器和数据库之间并不是唯一的，而索引 ID 甚至在各对象之间都不是唯一的。  
  
## <a name="finding-specific-events-within-a-trace"></a>查找跟踪内的特定事件  
 若要查找跟踪中的事件并对事件进行分组，请按下列步骤执行操作：  
  
1.  创建跟踪。  
  
    -   定义跟踪时，除了要捕获的任何其他数据列外，还要捕获 **“事件类”**、 **ClientProcessID** 和 **“开始时间”** 数据列。 有关详细信息，请参阅[创建跟踪 (SQL Server Profiler)](../../tools/sql-server-profiler/create-a-trace-sql-server-profiler.md)。  
  
    -   按“事件类” **“事件类”** 数据列对捕获的数据进行分组，并将跟踪内容捕获到文件或表中。 若要将捕获的数据分组，请单击“跟踪属性”对话框中 **“事件选择”** 选项卡中的 **“组织列”** 。 有关详细信息，请参阅[组织跟踪中显示的列 (SQL Server Profiler)](../../tools/sql-server-profiler/organize-columns-displayed-in-a-trace-sql-server-profiler.md)。  
  
    -   开始跟踪，并在经过适当时间或捕获了一定数量的事件后停止。  
  
2.  查找目标事件。  
  
    -   打开跟踪文件或表，并展开所需事件类的节点，例如， **Deadlock Chain**。 有关详细信息，请参阅 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md) 或 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)一起提供的预定义优化模板。  
  
    -   在跟踪数据中搜索直到找到所需的事件（使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 的“编辑”菜单上的“查找”命令帮助查找跟踪中的值）。 记录所跟踪事件的“ClientProcessID”和“开始时间”数据列中的值。  
  
3.  在上下文中显示事件。  
  
    -   显示跟踪属性，并按“ClientProcessID” **ClientProcessID** 数据列（而不是“事件类” **“事件类”** 数据列。  
  
    -   展开要查看的每个客户端进程 ID 的节点。 手动搜索整个跟踪内容，或使用“查找” **的** 直到找到目标事件的“开始时间” **“开始时间”** 值（如上所述）。 这些事件与属于每个选定客户端进程 ID 的其他事件一起按时间顺序进行显示。 例如，跟踪内容中捕获的“死锁”和“死锁链”事件在展开的客户端进程 ID 内紧跟在“SQL:BatchStarting”事件之后显示。  
  
 可以使用相同的方法查找任何已分组的事件。 找到要查找的事件后，按 **ClientProcessID**、 **ApplicationName** 或其他事件类分组，以便按时间顺序查看相关活动。  
  
## <a name="see-also"></a>另请参阅  
 [查看保存的跟踪 (Transact-SQL)](../../relational-databases/sql-trace/view-a-saved-trace-transact-sql.md)   
 [sys.fn_trace_getinfo (Transact-SQL)](../../relational-databases/system-functions/sys-fn-trace-getinfo-transact-sql.md)   
 [查看筛选器信息 (SQL Server Profiler)](../../tools/sql-server-profiler/view-filter-information-sql-server-profiler.md)   
 [查看筛选器信息 (Transact-SQL)](../../relational-databases/sql-trace/view-filter-information-transact-sql.md)   
 [打开跟踪文件 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-file-sql-server-profiler.md)   
 [打开跟踪表 (SQL Server Profiler)](../../tools/sql-server-profiler/open-a-trace-table-sql-server-profiler.md)  
  
  
