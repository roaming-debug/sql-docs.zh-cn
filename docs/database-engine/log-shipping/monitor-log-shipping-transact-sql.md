---
title: 监视日志传送 (Transact-SQL) | Microsoft Docs
description: 了解哪些表存储包含监视信息的历史记录，以及用于监视 SQL Server 中的日志传送的存储过程。
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], status
- history tables [SQL Server]
- historical information [SQL Server], log shipping
- log shipping [SQL Server], monitoring
- alerts [SQL Server], log shipping
- status information [SQL Server], log shipping
- monitoring log shipping [SQL Server]
ms.assetid: acf3cd99-55f7-4287-8414-0892f830f423
author: cawrites
ms.author: chadam
ms.openlocfilehash: c328ae66b2ec85080b6da14ff054c824405b8be2
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130759"
---
# <a name="monitor-log-shipping-transact-sql"></a>监视日志传送 (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  配置日志传送后，就可以监视有关所有日志传送服务器状态的信息。 日志传送操作的历史记录和状态始终由日志传送作业保存在本地。 备份操作的历史记录和状态存储在主服务器上，复制和还原操作的历史记录和状态存储在辅助服务器上。 如果使用了远程监视服务器，此信息还将存储在监视服务器上。  
  
 您可以配置警报，当日志传送操作无法按计划发生时激发该警报。 监视备份和还原操作状态的警报作业将引发错误。 您可以定义警报，以便在引发这些错误时通知操作员。 如果配置了监视服务器，该监视服务器上将运行一个警报作业，它可以引发日志传送配置中所有操作的错误。 如果未指定监视服务器，警报作业将在主服务器实例上运行，以便监视备份操作。 警报作业还将在每个辅助服务器实例上运行，以便监视本地复制和还原操作。  
  
> [!IMPORTANT]  
>  若要监视日志传送配置，在启用日志传送时必须添加监视服务器。 如果之后添加监视服务器，则必须首先删除日志传送配置，然后将其替换为包含监视服务器的新配置。 有关详细信息，请参阅[配置日志传送 (SQL Server)](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)。 此外，在配置了监视服务器之后，只有首先删除日志传送才能对其进行更改。  
  
## <a name="history-tables-containing-monitoring-information"></a>包含监视信息的历史记录表  
 监视历史记录表包含监视服务器上存储的元数据。 与给定的主服务器或辅助服务器相关的信息副本也存储在本地。  
  
 可以查询这些表，以监视日志传送会话的状态。 例如，了解日志传送的状态，查看备份作业、复制作业和还原作业的状态和历史记录。 通过查询下列监视表，可以查看特定的日志传送历史记录和错误详细信息。  
  
|表|说明|  
|-----------|-----------------|  
|[log_shipping_monitor_alert](../../relational-databases/system-tables/log-shipping-monitor-alert-transact-sql.md)|存储警报作业 ID。|  
|[log_shipping_monitor_error_detail](../../relational-databases/system-tables/log-shipping-monitor-error-detail-transact-sql.md)|存储日志传送作业的错误详细信息。 可以查询此表来查看某个代理会话的错误。 还可以按每个错误的记录日期和时间对错误进行排序。 每个错误都记录为一个异常序列，多个错误（序列）可以形成一个代理会话。|  
|[log_shipping_monitor_history_detail](../../relational-databases/system-tables/log-shipping-monitor-history-detail-transact-sql.md)|存储日志传送代理的历史记录详细信息。 可以查询此表来查看某个代理会话的历史记录详细信息。|  
|[log_shipping_monitor_primary](../../relational-databases/system-tables/log-shipping-monitor-primary-transact-sql.md)|在每个日志传送配置中对主数据库存储一条监视记录，包括有关对监视有用的最新备份文件和最新还原文件的信息。|  
|[log_shipping_monitor_secondary](../../relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)|对每个辅助数据库存储一条监视记录，包括有关对监视有用的最新备份文件和最新还原文件的信息。|  
  
## <a name="stored-procedures-for-monitoring-log-shipping"></a>监视日志传送的存储过程  
 监视和历史记录信息存储在 **msdb** 的表中，可以通过日志传送存储过程来访问它。 请在下表中指定的服务器上运行下列存储过程。  
  
|存储过程|说明|运行存储过程的服务器|  
|----------------------|-----------------|---------------------------|  
|[sp_help_log_shipping_monitor_primary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)|从 **log_shipping_monitor_primary** 表中返回指定的主数据库的监视记录。|监视服务器或主服务器|  
|[sp_help_log_shipping_monitor_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)|从 **log_shipping_monitor_secondary** 表中返回指定的辅助数据库的监视记录。|监视服务器或辅助服务器|  
|[sp_help_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-help-log-shipping-alert-job-transact-sql.md)|返回警报作业的作业 ID。|监视服务器或主/辅助服务器（如果未定义监视服务器）|  
|[sp_help_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)|检索主数据库设置并显示 **log_shipping_primary_databases** 和 **log_shipping_monitor_primary** 表中的值。|主服务器|  
|[sp_help_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-secondary-transact-sql.md)|检索主数据库的辅助数据库名称。|主服务器|  
|[sp_help_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-database-transact-sql.md)|从 **log_shipping_secondary**、 **log_shipping_secondary_databases** 和 **log_shipping_monitor_secondary** 表中检索辅助数据库设置。|辅助服务器|  
|[sp_help_log_shipping_secondary_primary (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-secondary-primary-transact-sql.md)|此存储过程将在辅助服务器上检索给定的主数据库的设置。|辅助服务器|  
  
## <a name="see-also"></a>另请参阅  
 [查看日志传送报告 (SQL Server Management Studio)](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)   
 [日志传送存储过程和表](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
