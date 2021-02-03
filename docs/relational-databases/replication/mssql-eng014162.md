---
description: MSSQL_ENG014162
title: MSSQL_ENG014162 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014162 error
ms.assetid: a15eef3f-219f-45d3-8286-6a864c85a723
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 9e11f6fd5f00dde8f2d2e77e41d7a30d64c917b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191630"
---
# <a name="mssql_eng014162"></a>MSSQL_ENG014162
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14162|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|已设置发布 [%s] 的阈值 [%s:%s]。 请确保合并代理正在运行且符合要求。|  
  
## <a name="explanation"></a>说明  
 使用复制可以对一些情况启用警告。 这包括超出了为同步合并发布服务器和订阅服务器间的更改所指定的时间长度。 可以为 LAN 连接和拨号连接指定不同的连接次数。  
  
 使用复制监视器或 [sp_replmonitorchangepublicationthreshold](../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md)启用警告时，请指定阈值以确定何时触发警告。 达到或超过该阈值时，复制监视器中将显示警告，并且将一个事件写入 Windows 事件日志。 达到阈值还会触发 SQL Server 代理警报。 有关详细信息，请参阅[在复制监视器中设置阈值和警告](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)和[以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)。  
  
## <a name="user-action"></a>用户操作  
 如果订阅超过持续时间阈值，则必须确定系统是否存在性能问题，或是否应调整阈值。 配置复制后，请确立一个性能基准，以便于确定复制在通常用于应用程序和拓扑的常见工作负荷中的行为方式。 将同步持续时间包括在该基准中，以便可以为阈值设置合适的值。  
  
 如果阈值适当，但超过了该阈值，则必须确定系统何处存在性能瓶颈。 有关如何监视复制性能和对其进行故障排除的详细信息，请参阅下列主题：  
  
-   [用复制监视器监视性能](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)（特别是“查看合并复制的详细同步性能”一节）  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
