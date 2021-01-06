---
title: 数据库镜像历史记录 | Microsoft Docs
description: 了解如何在 SQL Server Management Studio 中查看位于指定服务器实例中的镜像数据库的镜像状态历史记录。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.databasemirroringhistory.f1
ms.assetid: 1d6e4b10-4a23-47d7-9918-c417992f09d3
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: eb42f9910acbc412117713b7be7d08fe4c6ee78f
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642005"
---
# <a name="database-mirroring-history"></a>数据库镜像历史记录
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用该对话框可以查看位于指定服务器实例中的镜像数据库的镜像状态历史记录。  
  
 **使用 SQL Server Management Studio 监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>选项  
 **服务器实例**  
 从中报告历史记录的服务器实例的名称。  
  
 **Database**  
 正报告其历史记录的数据库名称。  
  
 **筛选列表依据**  
 列出筛选器值。 选择新值时将自动刷新网格。  
  
 可能的筛选器值有：  
  
-   前两个小时  
  
-   前四个小时  
  
-   前八个小时  
  
-   前一天  
  
-   前两天  
  
-   前 100 条记录  
  
-   前 500 条记录  
  
-   前 1000 条记录  
  
-   所有记录  
  
 **“刷新”**  
 单击它可刷新历史记录列表。  
  
> [!NOTE]  
>  该对话框不自动刷新历史记录列表。 若要刷新该列表，可单击 **“刷新”** 或选择另一个筛选选项。 只有 **sysadmin** 固定服务器角色的成员才能更新镜像历史记录。  
  
 **History**  
 显示历史记录列表。 单击列标题，可以按该列对网格进行排序。 该列表包含以下列：  
  
|列名称|说明|  
|-----------------|-----------------|  
|**记录时间**|历史记录行的时间戳。|  
|**角色**|数据库的服务器实例的当前镜像角色，即主体或镜像。|  
|**镜像状态**|数据库的状态：<br /><br /> 已断开连接<br /><br /> 正在挂起故障转移<br /><br /> Suspended<br /><br /> 已同步<br /><br /> 正在同步<br /><br /> 未知|  
|**见证服务器连接**|见证服务器连接在数据库镜像会话中的状态，即连接或已断开连接。 如果没有见证服务器，则该值为 NULL。|  
|**未发送日志**|主体服务器实例上的发送队列中的未发送日志的大小（以 KB 计）。|  
|**发送时间**|主体服务器实例将当前位于发送队列中的日志发送到镜像服务器实例所需的近似时间量（发送速率）。 由于传入事务的速率变化很大，因此发送日志的时间只是一个估计值。 但是，发送速率对于粗略估计手动故障转移所需的时间非常有用。|  
|**发送速率**|将事务发送到镜像服务器实例的速率（以 KB/秒计）。|  
|**新事务速率**|将传入事务输入到主体服务器日志的速率（以 KB/秒计）。 若要判断镜像是否落后、停滞或赶上，可将值与 **“发送时间”** 值进行比较。|  
|**最早的未发送事务**|发送队列中最早的未发送事务的保留时间。 事务保留时间指示在将事务发送到镜像服务器实例之前将其保留的分钟数。 该值有助于测量数据丢失的可能性（以时间计）。|  
|**未还原日志**|在重做队列中等待的日志量（以 KB 计）。|  
|**还原时间**|将当前在重做队列中的日志应用于镜像数据库所需的近似分钟数。|  
|**还原速率**|将事务还原到镜像数据库的速率（以 KB/秒计）。|  
|**镜像提交开销**|每个事务平均延迟的毫秒数（仅在同步模式下）。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。|  
  
## <a name="see-also"></a>另请参阅  
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
