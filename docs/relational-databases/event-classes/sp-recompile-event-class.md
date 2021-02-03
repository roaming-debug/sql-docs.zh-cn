---
description: SP:Recompile 事件类
title: SP:Recompile 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SP:Recompile event class
ms.assetid: 526c8eae-a07b-4d0e-b91e-8e537835d77d
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3772823e0d7dba3ac83e152da2634b8841102520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161578"
---
# <a name="sprecompile-event-class"></a>SP:Recompile 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  SP:Recompile 事件类指示存储过程、触发器或用户定义的函数已被重新编译。 此事件类报告的重新编译在语句级上发生。  
  
 跟踪语句级重新编译的首选方法是使用 SQL:StmtRecompile 事件类。 已弃用 SP:Recompile 事件类。 有关详细信息，请参阅 [SQL:StmtRecompile Event Class](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md)。  
  
## <a name="sprecompile-event-class-data-columns"></a>SP:Recompile 事件类的数据列  
  
|数据列名称|**Data type**|说明|列 ID|可筛选|  
|----------------------|-------------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立连接的客户端应用程序的名称。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|正在运行存储过程的数据库的 ID。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在运行存储过程的数据库的名称。|35|是|  
|EventClass|**int**|事件类型 = 37。|27|否|  
|EventSequence|**int**|特定事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|事件子类的类型。 指示重新编译的原因。<br /><br /> 1 = 架构已更改<br /><br /> 2 = 统计已更改<br /><br /> 3 = 重新编译 DNR<br /><br /> 4 = 所设置的选项已更改<br /><br /> 5 = 临时表已更改<br /><br /> 6 = 远程行集已更改<br /><br /> 7 = 浏览 Perm 的方式已更改<br /><br /> 8 = 查询通知环境已更改<br /><br /> 9 = MPI 视图已更改<br /><br /> 10 = 游标选项已更改<br /><br /> 11 = 使用重新编译选项|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IntegerData2|**int**|在存储过程或批处理中导致重新编译的语句的终止偏移量。 如果该语句是其所在批处理中的最后一个语句，则终止偏移量是 -1。|55|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NestLevel|**int**|存储过程的嵌套级别。|29|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ObjectID|**int**|系统分配的存储过程 ID。|22|是|  
|ObjectName|**nvarchar**|触发重新编译的对象的名称。|34|是|  
|ObjectType|**int**|表示事件中涉及的对象类型的值。 有关详细信息，请参阅 [ObjectType Trace Event Column](../../relational-databases/event-classes/objecttype-trace-event-column.md)。|28|是|  
|Offset|**int**|在存储过程或批处理中导致重新编译的语句的起始偏移量。|61|是|  
|RequestID|**int**|包含该语句的请求的 ID。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|SqlHandle|**varbinary**|基于即席查询文本或 SQL 对象的数据库和对象 ID 的 64 位哈希运算。 可以将该值传递到 sys.dm_exec_sql_text 以检索关联的 SQL 文本。|63|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|导致语句级重新编译的 Transact-SQL 语句文本。|1|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
|XactSequence|**bigint**|用于说明当前事务的标记。|50|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [SQL:StmtRecompile 事件类](../../relational-databases/event-classes/sql-stmtrecompile-event-class.md)  
  
  
