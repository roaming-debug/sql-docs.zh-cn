---
description: SQL:StmtRecompile 事件类
title: SQL:StmtRecompile 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL:StmtRecompile event class
ms.assetid: 3a134751-3e93-4fe8-bf22-1e0561189293
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4ad94f80748aae199d3fa19d6e923630bfa22390
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483629"
---
# <a name="sqlstmtrecompile-event-class"></a>SQL:StmtRecompile 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  SQL:StmtRecompile 事件类指示由下列所有类型的批处理引起的语句级重新编译：存储过程、触发器、即席批查询和查询。 可以通过使用 sp_executesql、动态 SQL、“准备”方法、“执行”方法或类似接口来提交查询。 应使用 SQL:StmtRecompile 事件类而不是 SP:Recompile 事件类。  
  
## <a name="sqlstmtrecompile-event-class-data-columns"></a>SQL:StmtRecompile 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立连接的客户端应用程序的名称。 该列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|正在运行存储过程的数据库的 ID。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在运行存储过程的数据库的名称。|35|是|  
|EventSequence|**int**|事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|说明重新编译的原因：<br /><br /> 1 = 架构的更改<br /><br /> 2 = 统计信息的更改<br /><br /> 3 = 编译的延迟<br /><br /> 4 = 设置选项的更改<br /><br /> 5 = 临时表的更改<br /><br /> 6 = 远程行集的更改<br /><br /> 7 = For Browse 权限的更改<br /><br /> 8 = 查询通知环境的更改<br /><br /> 9 = 分区视图的更改<br /><br /> 10 = 游标选项的更改<br /><br /> 11 = 已请求选项（重新编译）|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|运行提交此语句的客户端的计算机名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IntegerData2|**int**|在存储过程或批处理中导致重新编译的语句的终止偏移量。 如果该语句是其所在批处理中的最后一个语句，则终止偏移量是 -1。|55|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。<br /><br /> 1 = 系统<br /><br /> 0 = 用户|60|是|  
|LineNumber|**int**|此语句在批处理中的序列号（如果适用）。|5|是|  
|LoginName|**nvarchar**|提交此批处理所用的登录名。|11|是|  
|LoginSid|**图像**|当前登录用户的安全标识符 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NestLevel|**int**|存储过程调用的嵌套级别。 例如，my_proc_a 存储过程调用 my_proc_b。 在此示例中，my_proc_a 的 NestLevel 为 1，而 my_proc_b 的 NestLevel 为 2。|29|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|连接用户的 Windows 用户名。|6|是|  
|ObjectID|**int**|系统分配的对象标识符，包含引起重新编译的语句。 此对象可以是存储过程、触发器或用户定义函数。 对于即席批查询或已准备好的 SQL，ObjectID 和 ObjectName 将返回一个 NULL 值。|22|是|  
|ObjectName|**nvarchar**|ObjectID 标识的对象的名称。|34|是|  
|ObjectType|**int**|表示事件中涉及的对象类型的值。 有关详细信息，请参阅 [ObjectType Trace Event Column](../../relational-databases/event-classes/objecttype-trace-event-column.md)。|28|是|  
|Offset|**int**|在存储过程或批处理中导致重新编译的语句的起始偏移量。|61|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|正在跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|连接的服务器进程 ID。|12|是|  
|SqlHandle|**varbinary**|基于即席查询文本或 SQL 对象的数据库和对象 ID 的 64 位哈希运算。 可以将该值传递到 sys.dm_exec_sql_text 以检索关联的 SQL 文本。|63|否|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|重新编译的 Transact-SQL 语句的文本。|1|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|XactSequence|**bigint**|用于说明当前事务的标记。|50|是|  
  
## <a name="see-also"></a>另请参阅  
 [SP:Recompile 事件类](../../relational-databases/event-classes/sp-recompile-event-class.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
