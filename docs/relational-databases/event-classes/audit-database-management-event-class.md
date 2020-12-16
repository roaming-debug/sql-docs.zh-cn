---
description: Audit Database Management 事件类
title: Audit Database Management 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Audit Database Management event class
ms.assetid: 3e3de528-c3f8-413f-a6b9-d324ca95ad8e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 62f1bf732b57e8db0ec6bf1dcc9a5e6e2b1c6795
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440073"
---
# <a name="audit-database-management-event-class"></a>Audit Database Management 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  创建、更改或删除数据库时，会发生 **Audit Database Management** 事件类。  
  
## <a name="audit-database-management-event-class-data-columns"></a>Audit Database Management 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|ApplicationName|**nvarchar**|与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例建立连接的客户端应用程序的名称。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|**DatabaseID**|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**DatabaseName**|**nvarchar**|正在其中运行用户语句的数据库的名称。|35|是|  
|**DBUserName**|**nvarchar**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户名。|40|是|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**EventSubClass**|**int**|事件子类的类型。<br /><br /> 1 = 创建<br /><br /> 2 = 更改<br /><br /> 3 = 删除<br /><br /> 4 = 转储<br /><br /> 11 = 加载|21|是|  
|**HostName**|**nvarchar**|正在运行客户端的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**LoginName**|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录凭据，格式为“DOMAIN\username”）。|11|是|  
|**LoginSid**|**图像**|登录用户的安全标识号 (SID)。 你可以在 **sys.server_principals** 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|**NTDomainName**|**nvarchar**|用户所属的 Windows 域。|7|是|  
|**NTUserName**|**nvarchar**|Windows 用户名。|6|是|  
|**ObjectName**|**nvarchar**|引用的对象名。|34|是|  
|**ObjectType**|**int**|表示事件中涉及的对象类型的值。 此值对应于 **sysobjects** 表中的类型列。 有关值的信息，请参阅 [ObjectType 跟踪事件列](../../relational-databases/event-classes/objecttype-trace-event-column.md)。|28|是|  
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
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [DROP DATABASE (Transact SQL)](../../t-sql/statements/drop-database-transact-sql.md)  
  
