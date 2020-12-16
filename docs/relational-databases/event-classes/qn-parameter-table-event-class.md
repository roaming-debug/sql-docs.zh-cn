---
description: QN:Parameter Table 事件类
title: QN:Parameter Table 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- event classes [SQL Server], QN:Parameter Table
ms.assetid: 292da1ed-4c7e-4bd2-9b84-b9ee09917724
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ab228ac611b5e0267d9e1238d5269824020d5d3a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97420052"
---
# <a name="qnparameter-table-event-class"></a>QN:Parameter Table 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  QN:Parameter table 事件报告有关创建、删除存储参数信息的内部表和为其保留引用计数所需操作的信息。 另外，此事件还报告重置参数表的使用计数的内部活动。  
  
## <a name="qnparameter-table-event-class-data-columns"></a>QN:Parameter table 事件类的数据列  
  
|数据列|类型|说明|列号|可筛选|  
|-----------------|----------|-----------------|-------------------|----------------|  
|ApplicationName|**nvarchar**|客户端应用程序的名称，该客户端应用程序创建了指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的连接。 此列由应用程序传递的值填充，而不是由所显示的程序名填充。|10|是|  
|ClientProcessID|**int**|由主机分配给正在运行客户端应用程序的进程的 ID。 如果客户端提供了客户端进程 ID，则填充此数据列。|9|是|  
|DatabaseID|**int**|由 USE *database* 语句指定的数据库的 ID；如果未对给定实例发出 USE *database* 语句，则为默认数据库的 ID。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 将显示数据库名。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|DatabaseName|**nvarchar**|正在运行用户语句的数据库的名称。|35|是|  
|EventClass|**Int**|事件类型 = 200。|27|否|  
|EventSequence|**int**|此事件的序列号。|51|否|  
|EventSubClass|**nvarchar**|事件子类的类型，提供有关每个事件类的进一步信息。 此列可能包含下列值：<br /><br /> **表已创建**：指示已在数据库中创建参数表。<br /><br /> **表删除尝试**：指示数据库已尝试自动删除未使用的参数表以释放资源。<br /><br /> **表删除尝试失败**：指示数据库已尝试删除未使用的参数表但失败。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将自动重新计划删除参数表以释放资源。<br /><br /> **表已删除**：指示数据库已成功删除参数表。<br /><br /> **表已驻留**：指示参数表标记为当前供内部处理使用。<br /><br /> **表已取消驻留**：指示参数表不驻留。 内部处理已使用表完成。<br /><br /> **用户数已增加**：指示引用参数表的查询通知订阅数已增加。<br /><br /> **用户数已减少**：指示引用参数表的查询通知订阅数已减少。<br /><br /> **LRU 计数器已重置**：指示参数表的使用计数已重置。<br /><br /> **清除任务已启动**：指示清除此参数表中的所有订阅的启动时间。 启动数据库或删除此参数表订阅下的表时执行此操作。<br /><br /> **清除任务已完成**：指示清除此参数表中的所有订阅的完成时间。|21|是|  
|GroupID|**int**|在其中激发 SQL 跟踪事件的工作负荷组的 ID。|66|是|  
|HostName|**nvarchar**|正在运行客户端程序的计算机的名称。 如果客户端提供了主机名，则填充此数据列。 若要确定主机名，请使用 HOST_NAME 函数。|8|是|  
|IsSystem|**int**|指示事件是发生在系统进程中还是发生在用户进程中。<br /><br /> 0 = 用户<br /><br /> 1 = 系统|60|否|  
|LoginName|**nvarchar**|用户的登录名（ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全登录名或 Windows 登录凭据，格式为 *DOMAIN*\\*Username*）。|11|否|  
|LoginSID|**图像**|已登录用户的安全标识号 (SID)。 您可以在 sys.server_principals 目录视图中找到此信息。 服务器中的每个登录名都具有唯一的 SID。|41|是|  
|NTDomainName|**nvarchar**|用户所属的 Windows 域。|7|是|  
|NTUserName|**nvarchar**|拥有生成此事件的连接的用户的名称。|6|是|  
|RequestID|**int**|包含该语句的请求的标识符。|49|是|  
|ServerName|**nvarchar**|所跟踪的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。|26|否|  
|SessionLoginName|**nvarchar**|发起会话的用户的登录名。 例如，如果应用程序使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并以 Login2 的身份执行语句，则 SessionLoginName 将显示“Login1”，而 LoginName 将显示“Login2”。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 Windows 登录名。|64|是|  
|SPID|**int**|发生该事件的会话的 ID。|12|是|  
|StartTime|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|TextData|**ntext**|返回包含特定于此事件的信息的 XML 文档。 此文档符合 [SQL Server Query Notification Profiler Event Schema](https://go.microsoft.com/fwlink/?LinkId=63331) 页上提供的 XML 架构。|1|是|  
  
  
