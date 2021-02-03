---
description: MSSQLSERVER_14420
title: MSSQLSERVER_14420 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 14420 (Database Engine error)
ms.assetid: 4a1d72b1-ab1b-4119-a11b-a8a05c6fdb45
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b85c252180248a45cb8f267299bdffccfef16d80
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196981"
---
# <a name="mssqlserver_14420"></a>MSSQLSERVER_14420
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|14420|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQLErrorNum14420|  
|消息正文|日志传送主数据库 %s.%s 的备份阈值为 %d 分钟，在过去的 %d 分钟之内未执行备份日志操作。 请查看代理日志和日志传送监视器信息。|  
  
## <a name="explanation"></a>说明  
日志传送在超出备份阈值的情况下不同步。 备份阈值是生成警报之前在日志传送备份作业之间允许等待的分钟数。 此消息并不一定表示日志传送存在问题。 相反，此消息可能表示存在以下问题之一：  
  
-   备份作业未运行。 导致此问题的可能原因包括：主服务器实例上的 SQL Server 代理服务未运行、作业被禁用或作业的计划已更改。  
  
-   备份作业失败。 导致此问题的可能原因包括：备份文件夹路径无效、磁盘已满或可能导致 BACKUP 语句失败的其他任何原因。  
  
## <a name="user-action"></a>用户操作  
排除错误消息  
  
-   确保主服务器实例的 SQL Server 代理服务处于运行状态，同时启用了该主数据库的备份作业并安排它以适当的频率运行。  
  
-   主服务器上的备份作业可能失败。 在这种情况下，请查看备份作业的作业历史记录以寻找原因。  
  
-   在主服务器实例上运行的日志传送备份作业可能无法连接到监视服务器实例以更新 **log_shipping_monitor_primary** 表。 这可能是由于监视服务器实例和主服务器实例之间的身份验证问题引起的。  
  
-   备份警报阈值可能包含错误的值。 理想情况下，至少将该值设置为备份作业频率的三倍。 如果在配置了日志传送并使之起作用之后更改了备份作业的频率，则必须相应地更新备份警报阈值的值。  
  
-   当监视服务器实例离线并重新在线时，在警报消息作业运行之前不会使用当前值更新 **log_shipping_monitor_primary** 表。 若要使用主数据库的最新数据更新监视表，请在主服务器实例上运行 **sp_refresh_log_shipping_monitor**。  
  
-   在主服务器实例或监视服务器实例上，日期或时间不正确。 这也可能生成警报消息。 可能在其中一个服务器实例上修改了系统日期或时间。  
  
    > [!NOTE]  
    > 两个服务器实例的时区不同不会引发问题。  
  
## <a name="see-also"></a>另请参阅  
[log_shipping_monitor_primary (Transact-SQL)](~/relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)  
[关于日志传送 (SQL Server)](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
[sp_help_log_shipping_monitor_primary (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)  
[sp_refresh_log_shipping_monitor (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)  
[关于日志传送 (SQL Server)](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
