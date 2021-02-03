---
description: Audit Server Operation 事件类
title: Audit Server Operation 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Audit Server Operation event class
ms.assetid: 6cc3dbb9-817e-4329-9f45-c3adcff3b511
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 850b31341c6002e5d30f230130726f149a3eabba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180872"
---
# <a name="audit-server-operation-event-class"></a>Audit Server Operation 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  当执行更改设置、资源、外部访问或授权等安全审核操作时，会发生 **Audit Server Operation** 事件类。  
  
## <a name="audit-server-operation-event-class-data-columns"></a>Audit Server Operation 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立连接的客户端应用程序的名称。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 数据列而且服务器可用，则 **ServerName** 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**DBUserName**|**nvarchar**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户名。|40|是|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|事件子类的类型。<br /><br /> 1 = 管理大容量操作<br /><br /> 2 = 更改设置<br /><br /> 3 = 更改资源<br /><br /> 4 = 身份验证<br /><br /> 5 = 外部访问<br /><br /> 6 = 更改服务器状态<br /><br /> 7 = 不安全的程序集<br /><br /> 8 = 更改连接<br /><br /> 9 = 更改资源调控器<br /><br /> 10 = 使用任何工作负荷组<br /><br /> 11 = 查看服务器状态|21|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**ObjectName**|**nvarchar**|引用的对象名。|34|是|  
|**ObjectType**|**int**|表示事件中涉及的对象类型的值。 此值对应于 **sys.objects** 目录视图中的类型列。 有关值的信息，请参阅 [ObjectType 跟踪事件列](../../relational-databases/event-classes/objecttype-trace-event-column.md)。|28|是|  
|**OwnerName**|**nvarchar**|对象所有者的数据库用户名。|37|是|  
|**RequestID**|**int**|包含该语句的请求的 ID。|49|是|  
|**ServerName**|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**Success**|**int**|1 = 成功。 0 = 失败。 例如，值为 1 时表示权限检查成功；值为 0 时表示权限检查失败。|23|是|  
|**TextData**|**ntext**|依赖于跟踪中捕获的事件类的文本值。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
|**XactSequence**|**bigint**|用于说明当前事务的标记。|50|是|  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
