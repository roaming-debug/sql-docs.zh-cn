---
title: SQL Server Extended Events Sessions
description: SQL Server 扩展事件会话是在用于承载 SQL Server 扩展事件引擎的进程中创建的。 了解会话状态和会话内容。
ms.date: 05/26/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- xe
- sessions
- extend events [SQL Server]
ms.assetid: c3c92544-351a-4bce-a06a-1f2a47e494e9
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bf399ec51bf77e7d2da86a4135453e1535b84974
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481358"
---
# <a name="sql-server-extended-events-sessions"></a>SQL Server Extended Events Sessions

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 扩展事件会话是在用于承载 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 扩展事件引擎的进程中创建的。 扩展事件会话的以下各方面信息可为您理解扩展事件基础结构和所发生的常规处理提供有关背景知识：  
  
-   会话状态。 执行 CREATE EVENT SESSION 和 ALTER EVENT SESSION 语句时扩展事件会话所处的不同状态。  
  
-   会话内容和特征。 扩展事件会话的内容（例如目标和事件）以及这些对象在某个会话中或多个会话间的关系。  
  
## <a name="session-states"></a>会话状态  
 下图说明了扩展事件会话的不同状态。  

![扩展事件会话状态](../../relational-databases/extended-events/media/xesessionstate.png "扩展事件会话状态")

 对照前面的图，可以注意到在对事件会话发出不同的 DDL 命令时会话状态将发生更改。 下表说明了这些状态更改。  
  
|图例标签|DDL 语句|说明|  
|------------------------|-------------------|-----------------|  
|创建|CREATE EVENT SESSION|主机进程将创建一个会话对象，其中包含由 CREATE EVENT SESSION 提供的元数据。 主机进程将验证会话定义和用户权限级别，并将元数据存储在 master 数据库中。 此时该会话处于不活动状态。|  
|更改|ALTER EVENT SESSION, STATE=START|主机进程启动会话。 主机进程将读取存储的元数据、验证会话定义、验证用户权限级别并创建会话。 此操作还将载入会话对象（例如事件和目标），此时事件处理即处于活动状态。|  
|更改|ALTER EVENT SESSION, STATE=STOP|主机进程将停止活动会话，但会保留元数据。|  
|丢弃|删除事件会话|此“删除”(DROP SESSION) 操作将删除元数据并关闭活动会话，或仅删除会话元数据；具体取决于会话是否处于活动状态。|  
  
> [!NOTE]  
>  ALTER EVENT SESSION 和 DROP EVENT SESSION 均可应用于元数据或者应用于活动会话与元数据。  
  
## <a name="session-content-and-characteristics"></a>会话内容和特征  
 扩展事件会话包括隐含的边界，因为一个会话的配置不会更改另一个会话的配置。 但是，这些边界不会阻止在多个会话中同时使用某个事件或目标。  
  
 下图说明了会话内容以及包和会话之间的关系。  
  
 ![会话中的对象共存和共享。](../../relational-databases/extended-events/media/xesessions.gif "会话中的对象共存和共享。")  
  
 对照前面的图，可注意到：  
  
-   包对象与会话之间的映射是多对多映射，也就是说，一个对象可在多个会话中出现，一个会话也可包含多个对象。  
  
-   可在多个会话中启用同一个事件 (Event 1) 或目标 (Target 1)。  
  
 会话具有以下特征：  
  
-   对于每个会话，操作和谓词均将绑定到事件。 如果在会话 A 中包含事件 1（绑定有操作 1 和谓词 Z），这将不会对会话 B 中包含事件 1（绑定有操作 2 和操作 3，但没有谓词）有任何影响。  
  
-   策略将被附加到会话以处理缓冲和调度以及因果关系跟踪。  
  
 **缓冲和调度**  
  
 缓冲指运行事件会话期间事件数据的存储方式。  缓冲策略将指定用于事件数据的内存大小以及针对事件的丢失策略。 调度指事件在被传递给目标进行处理之前驻留在缓冲区的时间。  
  
 **因果关系跟踪**  
  
 因果关系跟踪提供了跨越多个任务来跟踪工作的功能。 当启用因果关系跟踪时，每个激发的事件在系统中都具有一个唯一的活动 ID。 活动 ID 是 GUID 值和序列号的组合，GUID 值对于某项任务的所有事件将保持不变，而序列号将随着事件的每次激发而递增。 当一项任务导致对另一项任务执行工作时，父任务的活动 ID 将发送到子任务。 子任务将在其第一次激发事件时输出父任务的活动 ID。  
  
 扩展事件体系结构提供了一个灵活的系统，允许同时使用多个对象以解决特定的问题。  
  
## <a name="see-also"></a>另请参阅  
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
