---
description: Performance Statistics 事件类
title: Performance Statistics 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Performance Statistics event class
ms.assetid: da9cd2c4-6fdd-4ada-b74f-105e3541393c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5f1ffd07f6573a37005fb7bd20bfa015a0a2102b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196322"
---
# <a name="performance-statistics-event-class"></a>Performance Statistics 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Performance Statistics 事件类可用于监视正在执行的查询、存储过程和触发器的性能。 六个事件子类分别表示系统内查询、存储过程和触发器的生存期内的一个事件。 使用这些事件子类的组合以及关联的 sys.dm_exec_query_stats、sys.dm_exec_procedure_stats 和 sys.dm_exec_trigger_stats 动态管理视图，可以重新构建任何给定查询、存储过程或触发器的性能历史记录。  
  
## <a name="performance-statistics-event-class-data-columns"></a>Performance Statistics 事件类的数据列  
 下表介绍了与下面每个事件子类关联的事件类数据列：EventSubClass 0、EventSubClass 1、EventSubClass 2、EventSubClass 3、EventSubClass 4 和 EventSubClass 5。  
  
### <a name="eventsubclass-0"></a>EventSubClass 0  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Null|52|是|  
|BinaryData|**图像**|Null|2|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 0 = 当前未存在于缓存中的新批处理 SQL 文本。<br /><br /> 下列 EventSubClass 类型是在即席批查询的跟踪中生成的。<br /><br /> 对于有 *n* 次查询的即席批查询：<br /><br /> 1 个类型 0 的查询|21|是|  
|IntegerData2|**int**|Null|55|是|  
|ObjectID|**int**|Null|22|是|  
|Offset|**int**|Null|61|是|  
|PlanHandle|**图像**|Null|65|是|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄，可使用该句柄通过 sys.dm_exec_sql_text 动态管理视图来获取批查询 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|批处理的 SQL 文本。|1|是|  
  
### <a name="eventsubclass-1"></a>EventSubClass 1  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|重新编译此计划的累积次数。|52|是|  
|BinaryData|**图像**|已编译计划的二进制 XML。|2|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 1 = 存储过程中的查询已编译。<br /><br /> 下列 EventSubClass 类型是在存储过程的跟踪中生成的。<br /><br /> 对于有 *n* 次查询的存储过程：<br /><br /> *n* 个类型 1 的查询|21|是|  
|IntegerData2|**int**|存储过程内语句的结尾。<br /><br /> 对于存储过程的结尾，此值为 -1。|55|是|  
|ObjectID|**int**|系统分配的对象 ID。|22|是|  
|Offset|**int**|存储过程或批查询中的语句的起始偏移量。|61|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄，可使用该句柄通过 dm_exec_sql_text 动态管理视图来获取存储过程的 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|Null|1|是|  
|PlanHandle|**图像**|存储过程的编译计划的计划句柄。 可使用该句柄通过 sys.dm_exec_query_plan 动态管理视图来获取 XML 计划。|65|是|  
|ObjectType|**int**|表示事件中涉及的对象类型的值。<br /><br /> 8272 = 存储过程|28|是|  
|BigintData2|**bigint**|在编译过程中使用的总内存 (KB)。|53|是|  
|CPU|**int**|编译过程中所用的总 CPU 时间（毫秒）。|18|是|  
|Duration|**int**|编译过程中所用的总时间（毫秒）。|13|是|  
|IntegerData|**int**|编译计划的大小 (KB)。|25|是|  
  
### <a name="eventsubclass-2"></a>EventSubClass 2  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|重新编译此计划的累积次数。|52|是|  
|BinaryData|**图像**|已编译计划的二进制 XML。|2|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 2 = 临时 SQL 语句中的查询已编译。<br /><br /> 下列 EventSubClass 类型是在即席批查询的跟踪中生成的。<br /><br /> 对于有 *n* 次查询的即席批查询：<br /><br /> *n* 个类型 2 的查询|21|是|  
|IntegerData2|**int**|批处理内语句的结尾。<br /><br /> 对于批处理的结尾，此值为 -1。|55|是|  
|ObjectID|**int**|空值|22|是|  
|Offset|**int**|批处理中的语句的起始偏移量。<br /><br /> 对于批处理的开始，此值为 0。|61|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄。 可使用该句柄通过 dm_exec_sql_text 动态管理视图来获取批查询 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|Null|1|是|  
|PlanHandle|**图像**|批处理的编译计划的计划句柄。 可使用该句柄通过 dm_exec_query_plan 动态管理视图来获取批查询 XML 计划。|65|是|  
|BigintData2|**bigint**|在编译过程中使用的总内存 (KB)。|53|是|  
|CPU|**int**|编译过程中所用的总 CPU 时间（毫秒）。|18|是|  
|Duration|**int**|编译过程中所用的总时间（毫秒）。|13|是|  
|IntegerData|**int**|编译计划的大小 (KB)。|25|是|  
  
### <a name="eventsubclass-3"></a>EventSubClass 3  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|重新编译此计划的累积次数。|52|是|  
|BinaryData|**图像**|Null|2|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 3 = 保存在缓存中的某一查询已被破坏，与此计划相关的历史性能数据也将被破坏。<br /><br /> 下列 EventSubClass 类型是在跟踪中生成的。<br /><br /> 对于有 *n* 次查询的即席批查询：<br /><br /> 1 个类型 3 的查询（当从缓存中刷新查询时）<br /><br /> 对于有 *n* 次查询的存储过程：<br /><br /> 1 个类型 3 的查询（当从缓存中刷新查询时）。|21|是|  
|IntegerData2|**int**|存储过程或批处理中的语句的结尾。<br /><br /> 对于存储过程或批处理的结尾，此值为 -1。|55|是|  
|ObjectID|**int**|Null|22|是|  
|Offset|**int**|存储过程或批查询中的语句的起始偏移量。<br /><br /> 对于存储过程或批处理的开始，此值为 0。|61|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄，可使用该句柄通过 dm_exec_sql_text 动态管理视图来获取存储过程或批查询 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|QueryExecutionStats|1|是|  
|PlanHandle|**图像**|存储过程或批处理的编译计划的计划句柄。 可使用该句柄通过 dm_exec_query_plan 动态管理视图来获取 XML 计划。|65|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
  
### <a name="eventsubclass-4"></a>EventSubClass 4  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Null|52|是|  
|BinaryData|**图像**|Null|2|是|  
|DatabaseID|**int**|给定存储过程所在的数据库的 ID。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 4 = 缓存的存储过程已从缓存中删除，与它关联的历史性能数据也即将销毁。|21|是|  
|IntegerData2|**int**|Null|55|是|  
|ObjectID|**int**|存储过程的 ID。 与 sys.procedures 中的 object_id 列相同。|22|是|  
|Offset|**int**|Null|61|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄，可使用该句柄通过 dm_exec_sql_text 动态管理视图来获取所执行的存储过程 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|ProcedureExecutionStats|1|是|  
|PlanHandle|**图像**|存储过程的编译计划的计划句柄。 可使用该句柄通过 dm_exec_query_plan 动态管理视图来获取 XML 计划。|65|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
  
### <a name="eventsubclass-5"></a>EventSubClass 5  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|BigintData1|**bigint**|Null|52|是|  
|BinaryData|**图像**|Null|2|是|  
|DatabaseID|**int**|给定触发器所在的数据库的 ID。|3|是|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 5 = 缓存的触发器已从缓存中删除，与它关联的历史性能数据也即将销毁。|21|是|  
|IntegerData2|**int**|Null|55|是|  
|ObjectID|**int**|触发器的 ID。 与 sys.triggers/sys.server_triggers 目录视图中的 object_id 列相同。|22|是|  
|Offset|**int**|Null|61|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**图像**|SQL 句柄，可使用该句柄通过 dm_exec_sql_text 动态管理视图来获取触发器的 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|TriggerExecutionStats|1|是|  
|PlanHandle|**图像**|触发器的编译计划的计划句柄。 可使用该句柄通过 dm_exec_query_plan 动态管理视图来获取 XML 计划。|65|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [Showplan XML For Query Compile 事件类](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  
