---
description: 'Progress Report: Online Index Operation 事件类'
title: 'Progress Report: Online Index Operation 事件类'
ms.date: 06/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 'Progress Report: Online Index Operation event class [SQL Server]'
ms.assetid: 491616c1-f666-4b16-a5ea-1192bf156692
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: c612dfd308f20c56f1e216861c1f59e6e169107a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205660"
---
# <a name="progress-report-online-index-operation-event-class"></a>Progress Report: Online Index Operation 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  Progress Report: Online Index Operation 事件类指示在联机索引生成进程运行时联机索引生成操作的进度。  
  
## <a name="progress-report-online-index-operation-event-class-data-columns"></a>Progress Report: Online Index Operation 事件类数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|BigintData1|**bigint**|插入的行数。|52|是|  
|BigintData2|**bigint**|0 = 串行计划，否则为并行执行过程中的线程 ID。|53|是|  
|ClientProcessID|**int**|主机为运行该客户端应用程序的进程分配的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 如果在跟踪中捕获 ServerName 数据列而且服务器可用，则将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|Duration|**bigint**|事件占用的时间（微秒）。|13|是|  
|EndTime|**datetime**|联机索引操作完成的时间。|15|是|  
|EventClass|**int**|事件类型 = 190。|27|否|  
|EventSequence|**int**|给定事件在请求中的顺序。|51|否|  
|EventSubClass|**int**|事件子类的类型。<br /><br /> 1 = 启动<br /><br /> 2 = 阶段 1 执行开始<br /><br /> 3 = 阶段 1 执行结束<br /><br /> 4 = 阶段 2 执行开始<br /><br /> 5 = 阶段 2 执行结束<br /><br /> 6 = 已插入行计数<br /><br /> 7 = 完成<br /><br /> 阶段 1 表示基对象（聚集索引或堆）或者索引操作是否仅涉及一个非聚集索引。 在索引生成操作同时涉及原始重新生成以及附加的非聚集索引时，使用阶段 2。  例如，如果某个对象具有一个聚集索引和多个非聚集索引，则“全部重新生成”可重新生成所有索引。 基对象（聚集索引）在阶段 1 中重新生成，然后，所有非聚集索引在阶段 2 中重新生成。|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IndexID|**int**|受事件影响的对象的索引的 ID。|24|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|LoginSid|**图像**|登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|Windows 用户名。|6|是|  
|ObjectID|**int**|系统分配的对象 ID。|22|是|  
|ObjectName|**nvarchar**|引用的对象名。|34|是|  
|PartitionId|**bigint**|要生成的分区的 ID。|65|是|  
|PartitionNumber|**int**|要生成的分区的序号。|25|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果您使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 SessionLoginName 将显示 Login1，而 LoginName 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|事件的启动时间。|14|是|  
|TransactionID|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
