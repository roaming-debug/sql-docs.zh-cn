---
description: FT:Crawl Started 事件类
title: FT:Crawl Started 事件类 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- Crawl Started event class
ms.assetid: 2535b856-97e8-4fb2-8ba0-5d5446355fa6
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 85f6f0be6c2bb1440fc9ffb967c624e092f7d048
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160708"
---
# <a name="ftcrawl-started-event-class"></a>FT:Crawl Started 事件类
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  FT:Crawl Started 事件类指示全文爬网（填充）已开始。 使用此事件类来检查工作线程任务是否正在实际拾取爬网请求。  
  
## <a name="ft-crawl-started-event-class-data-columns"></a>FT: Crawl Started 事件类的数据列  
  
|数据列名称|数据类型|说明|列 ID|可筛选|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**DatabaseID**|**int**|启动了全文爬网的数据库 ID。 可使用 DB_ID 函数来确定数据库的值。|3|是|  
|**EventClass**|**int**|事件类型 = 155。|27|否|  
|**EventSequence**|**int**|给定事件在请求中的顺序。|51|否|  
|**IsSystem**|**int**|指示事件是发生在系统进程中还是发生在用户进程中。 1 = 系统，0 = 用户。|60|是|  
|**Exchange Spill**|**int**|系统分配的对象 ID。 对关于此对象的全文索引启动了全文爬网。|22|是|  
|**SessionLoginName**|**nvarchar**|发起会话的用户的登录名。 例如，如果你使用 Login1 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，再以 Login2 的身份执行语句，则 **SessionLoginName** 将显示 Login1，而 **LoginName** 将显示 Login2。 此列将同时显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登录名。|64|是|  
|**SPID**|**int**|发生该事件的会话的 ID。|12|是|  
|**StartTime**|**datetime**|该事件（如果存在）的启动时间。|14|是|  
|**TextData**|**ntext**|全文爬网类型。 该值可以是“完整”、“增量”、“手动”或“自动”。|1|是|  
|**TransactionID**|**bigint**|系统分配的事务 ID。|4|是|  
  
## <a name="see-also"></a>另请参阅  
 [sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
