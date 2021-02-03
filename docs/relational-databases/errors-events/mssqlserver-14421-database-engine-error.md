---
description: MSSQLSERVER_14421
title: MSSQLSERVER_14421 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 14421 (Database Engine error)
ms.assetid: 03e76d4a-d463-4673-8843-08e4ecaefe27
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0c492f6492ba38a00548d21052150e5f703db4c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196967"
---
# <a name="mssqlserver_14421"></a>MSSQLSERVER_14421
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|14421|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|SQLErrorNum14421|  
|消息正文|日志传送辅助数据库 %s.%s 的还原阈值为 %d 分钟，并且现在不同步。在过去的 %d 分钟之内未执行任何还原操作。 还原操作滞后了 %d 分钟。 请查看代理日志和日志传送监视器信息。|  
  
## <a name="explanation"></a>说明  
此消息指出日志传送在超出还原阈值的情况下不同步。 还原阈值是生成消息之前在还原操作之间允许等待的分钟数。  
  
### <a name="possible-causes"></a>可能的原因  
此消息并不一定表示日志传送存在问题。 相反，此消息可能表示存在以下问题之一：  
  
-   还原作业未运行。  
  
    导致作业未运行的可能原因包括：辅助服务器实例上的 SQL Server 代理服务未运行、作业被禁用或作业计划已更改。  
  
-   还原作业失败。  
  
    导致作业失败的可能原因包括：还原文件夹路径无效、磁盘已满或可能导致 RESTORE 语句失败的其他任何原因。  
  
## <a name="user-action"></a>用户操作  
排除错误消息  
  
-   确保辅助服务器实例的 SQL Server 代理服务处于运行状态，同时启用了该辅助数据库的还原作业并安排它以适当的频率运行。  
  
-   辅助服务器上的还原作业可能失败。 在这种情况下，请查看还原作业的作业历史记录以寻找原因。  
  
-   在辅助服务器实例上运行的日志传送还原作业可能无法连接到监视服务器实例以更新 **log_shipping_monitor_secondary** 表。 这可能是由于监视服务器实例和辅助服务器实例之间的身份验证问题引起的。  
  
-   备份警报阈值可能包含错误的值。 理想情况下，至少将该值设置为还原作业频率的三倍。 如果在配置了日志传送并使之起作用之后更改了还原作业的频率，则必须相应地更新备份警报阈值的值。  
  
-   当监视服务器实例离线并重新在线时，在警报消息作业运行之前不会使用当前值更新 **log_shipping_monitor_secondary** 表。 还原作业虽然成功但显示消息“找不到可以应用到辅助数据库的日志备份文件”时，可能引发错误 14421。 出现这种情况时，将不更新还原时间。 在这种情况下，错误原因可能是复制作业存在问题。  
  
    若要使用辅助数据库的最新数据更新监视表，请在辅助服务器实例上运行 **sp_refresh_log_shipping_monitor**。  
  
-   在辅助服务器实例或监视服务器实例上，日期或时间不正确。 这也可能生成警报消息。 可能在其中一个服务器实例上修改了系统日期或时间。  
  
    > [!NOTE]  
    > 两个服务器实例的时区不同不会引发问题。  
  
## <a name="see-also"></a>另请参阅  
[log_shipping_monitor_secondary (Transact-SQL)](~/relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)  
[关于日志传送 (SQL Server)](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
[sp_help_log_shipping_monitor_secondary (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)  
[sp_refresh_log_shipping_monitor (Transact-SQL)](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)  
[关于日志传送 (SQL Server)](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
