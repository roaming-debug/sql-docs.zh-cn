---
description: MSSQL_ENG014161
title: MSSQL_ENG014161 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014161 error
ms.assetid: 4b983e76-bb77-43c5-b44b-19919d3da619
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 4a0549c07e81188955edb2a255f50dda26f8acbe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432217"
---
# <a name="mssql_eng014161"></a>MSSQL_ENG014161
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14161|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|已设置发布 [%s] 的阈值 [%s:%s]。 请确保日志读取器和分发代理正在运行并且可以满足滞后时间要求。|  
  
## <a name="explanation"></a>说明  
 使用复制可以对一些情况启用警告。 这些情况包括超过了事务订阅的指定滞后时间。 滞后时间是指在发布服务器上提交数据更改到在订阅服务器上提交相应更改之间所用的时间。  
  
 使用复制监视器或 [sp_replmonitorchangepublicationthreshold](../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md)启用警告时，请指定阈值以确定何时触发警告。 达到或超过该阈值时，复制监视器中将显示警告，并且将一个事件写入 Windows 事件日志。 达到阈值还会触发 SQL Server 代理警报。 有关详细信息，请参阅[在复制监视器中设置阈值和警告](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)和[以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)。  
  
## <a name="user-action"></a>用户操作  
 如果订阅超过滞后时间阈值，则必须确定是系统发生性能问题还是应当调整阈值。 配置复制后，请确立一个性能基准，以便于确定复制在通常用于应用程序和拓扑的常见工作负荷中的行为方式。 在此基准中包含滞后时间，以便可以设置适当的阈值。  
  
 如果阈值适当，但超过了该阈值，则必须确定系统何处存在性能瓶颈。 有关如何监视复制性能和对其进行故障排除的详细信息，请参阅下列主题：  
  
-   [为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
-   [用复制监视器监视性能](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
