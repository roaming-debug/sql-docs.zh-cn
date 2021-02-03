---
description: MSSQL_ENG014150
title: MSSQL_ENG014150 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014150 error
ms.assetid: c3dd3109-abf3-4b38-a4e9-ef48d0235656
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 6b6ad0060a3d039db1e46fdaab13251e28f5a3f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198776"
---
# <a name="mssql_eng014150"></a>MSSQL_ENG014150
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14150|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|复制 - %s：代理 %s 成功。 %s|  
  
## <a name="explanation"></a>说明  
 此消息指出复制代理已成功完成运行。 复制使用下列代理：  
  
-   快照代理。 该代理由所有发布使用。  
  
-   日志读取器代理。 该代理由所有事务发布使用。  
  
-   队列读取器代理。 该代理由为排队更新订阅启用的事务发布使用。  
  
-   分发代理。 该代理将订阅同步到事务发布和快照发布。  
  
-   合并代理。 该代理将订阅同步到合并发布。  
  
-   复制维护作业。  
  
## <a name="user-action"></a>用户操作  
 日志读取器代理、队列读取器代理和分发代理通常连续运行，而其他代理通常则按需要或按计划运行。 如果认为某个代理无法完成运行，请检查该代理的状态。 有关详细信息，请参阅 [Monitor Replication Agents](../../relational-databases/replication/monitor/monitor-replication-agents.md)。  
  
## <a name="see-also"></a>另请参阅  
 [复制代理管理](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)   
 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Log Reader Agent](../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Merge Agent](../../relational-databases/replication/agents/replication-merge-agent.md)   
 [复制队列读取器代理](../../relational-databases/replication/agents/replication-queue-reader-agent.md)   
 [Replication Snapshot Agent](../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
  
