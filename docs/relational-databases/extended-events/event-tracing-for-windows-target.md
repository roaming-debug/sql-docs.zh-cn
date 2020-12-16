---
title: Windows 事件跟踪目标
description: 了解如何将 Windows 事件跟踪 (ETW) 用作目标。 将 ETW 跟踪与扩展事件一起使用，或将其用作扩展事件的事件使用者。
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- event tracing for windows target
- ETW target
- targets [SQL Server extended events], event tracing for windows target
ms.assetid: ca2bb295-b7f6-49c3-91ed-0ad4c39f89d5
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6e458356777d9dfb6784491ae4dbdca6482b943f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465578"
---
# <a name="event-tracing-for-windows-target"></a>Windows 事件跟踪目标

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在将 Windows 事件跟踪 (ETW) 作为目标使用前，建议您先掌握 ETW 的相关使用知识。 ETW 跟踪或者与扩展事件结合使用，或者用作扩展事件的事件使用者。 您可以从以下外部链接入手，获取有关 ETW 的背景信息：  
  
-   [Windows 事件](/windows/win32/events/windows-events)  
  
-   [使用 ETW 改善调试和性能优化](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw)  
  
 ETW 目标是单独目标，尽管该目标可以被添加到多个会话中。 如果某事件在多个会话中被引发，则该事件在每次发生事件时仅被传播给 ETW 目标一次。 每个进程仅限一个扩展事件引擎实例。  
  
> [!IMPORTANT]  
>  为使 ETW 目标能够工作， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务启动帐户必须是性能日志用户组的成员。  
  
 对于 ETW 会话中存在的事件，其配置由承载扩展事件引擎的进程来控制。 该引擎控制着引发哪些事件，以及必须满足哪些条件才能引发事件。  
  
 在绑定到扩展事件会话之后，即在进程的生存期中首次附加 ETW 目标之后，ETW 目标会在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问接口上打开一个单独的 ETW 会话。 如果已经存在 ETW 会话，ETW 目标将获取对现有会话的引用。 此 ETW 会话由给定计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例共享， 并从具有此 ETW 目标的会话接收所有事件。  
  
 由于 ETW 需要启用访问接口才能使用事件并将事件发送给 ETW 会话，因而对此会话启用所有扩展事件包。 激发事件后，ETW 目标将事件发送给对其启用了事件访问接口的会话。  
  
 ETW 目标支持对引发事件的线程同步发布事件。 但是，ETW 目标不支持异步事件发布。  
  
 ETW 目标不支持来自外部 ETW 控制器（例如 Logman.exe）的控制。 若要生成 ETW 跟踪，必须使用 ETW 目标创建事件会话。 有关详细信息，请参阅 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)。  
  
> [!NOTE]  
>  启用 ETW 目标将创建一个名为 XE_DEFAULT_ETW_SESSION 的 ETW 会话。 如果名为 XE_DEFAULT_ETW_SESSION 的会话已经存在，则使用该会话而不修改现有会话的任何属性。 XE_DEFAULT_ETW_SESSION 由所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例共享。 启动了 XE_DEFAULT_ETW_SESSION 后，必须使用 ETW 控制器（如 Logman 工具）停止它。 例如，可以在命令提示符处运行以下命令： **logman stop XE_DEFAULT_ETW_SESSION -ets**。  
  
 下表介绍了配置 ETW 目标时可用的选项。  
  
|选项|允许的值|说明|  
|------------|--------------------|-----------------|  
|default_xe_session_name|任何不超过 256 个字符的字符串。 此值是可选的。|扩展事件会话名称。 默认情况下为 XE_DEFAULT_ETW_SESSION。|  
|default_etw_session_logfile_path|任何不超过 256 个字符的字符串。 此值是可选的。|扩展事件会话日志文件的路径。 默认情况下为 %TEMP%\ XEEtw.etl。|  
|default_etw_session_logfile_size_mb|任何无符号整数。 此值是可选的。|扩展事件会话日志文件的大小 (MB)。 默认值为 20 MB。|  
|default_etw_session_buffer_size_kb|任何无符号整数。 此值是可选的。|扩展事件会话内存缓冲区的大小 (KB)。 默认值为 128 KB。|  
|retries|任何无符号整数。|尝试将事件发布给 ETW 子系统的重试次数，在此次数之后将删除该事件。 默认值为 0。|  
| &nbsp; | &nbsp; | &nbsp; |

 这些设置的配置是可选的。 ETW 目标使用这些设置的默认值。  
  
 ETW 目标负责下列操作：  
  
-   创建默认的 ETW 会话。  
  
-   向 ETW 注册所有扩展事件包。 这样可以确保 ETW 不会删除事件。  
  
-   管理事件向 ETW 的发送。 ETW 目标使用扩展事件数据创建 ETW 事件并将其发送给相应的 ETW 会话。 如果事件的大小超过缓冲区的大小，或数据无法容纳到一个 ETW 事件中，则 ETW 将把事件拆分成几个片段。  
  
-   使扩展事件包在任何时候均保持为启用状态。  
  
 ETW 使用以下默认文件位置：  
  
-   ETW 输出文件的格式为 %TEMP%\XEEtw.etl。  
  
    > [!IMPORTANT]  
    >  当第一个会话启动后不能再更改该文件路径。  
  
-   托管对象格式 (MOF) 文件位于 \<your install path>\Microsoft SQL Server\Shared。 有关详细信息，请参阅 MSDN 上的 [托管对象格式](/windows/win32/wmisdk/managed-object-format--mof-) 。

<!-- ?LinkId=92851  ==  https://docs.microsoft.com/windows/desktop/WmiSdk/managed-object-format--mof-
-->

## <a name="adding-the-target-to-a-session"></a>将目标添加到会话  
 若要将 ETW 目标添加到扩展事件会话中，您必须在创建或更改事件会话时包括下面的语句：  
  
```sql
ADD TARGET package0.etw_classic_sync_target  
```  
  
 有关说明如何使用 ETW 目标的完整示例的详细信息（包括如何查看数据），请参阅 [使用扩展事件监视系统活动](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 扩展事件目标](targets-for-extended-events-in-sql-server.md)   
 [sys.dm_xe_session_targets (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)   
 [CREATE EVENT SESSION (Transact-SQL)](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)  
  
