---
description: log_shipping_monitor_primary (Transact-SQL)
title: log_shipping_monitor_primary (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- log_shipping_monitor_primary
- log_shipping_monitor_primary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- log_shipping_monitor_primary system table
ms.assetid: 5f629a29-1a62-40e6-ae33-6f6b7dd09a36
author: cawrites
ms.author: chadam
ms.openlocfilehash: 076398da53bf4886a0b8e3f387df949d29cb8b03
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098687"
---
# <a name="log_shipping_monitor_primary-transact-sql"></a>log_shipping_monitor_primary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在每个日志传送配置中为每个主数据库存储一条监视记录。 该表存储在 **msdb** 数据库中。  
  
 与历史记录和监视相关的表也用于主服务器和辅助服务器。   
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**primary_id**|**uniqueidentifier**|日志传送配置的主数据库 ID。|  
|**primary_server**|**sysname**|日志传送配置中 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]主实例的名称。|  
|**primary_database**|**sysname**|日志传送配置中主数据库的名称。|  
|**backup_threshold**|**int**|备份操作之间的占用时间阈值（分钟），一旦超过此值，就会生成警报。|  
|**threshold_alert**|**int**|超过备份阈值时引发的警报。|  
|**threshold_alert_enabled**|**bit**|确定是否启用备份阈值警报。 1 = 启用。<br /><br /> 0 = 禁用。|  
|**last_backup_file**|**nvarchar (500)**|最近一次事务日志备份的绝对路径。|  
|**last_backup_date**|**datetime**|上一次在主数据库上执行事务日志备份操作的时间和日期。|  
|**last_backup_date_utc**|**datetime**|上一次在主数据库上执行事务日志备份操作的时间和日期，使用协调世界时表示。|  
|**history_retention_period**|**int**|日志传送历史记录在删除前保留在给定主数据库中的时间（分钟）。|  
  
## <a name="remarks"></a>备注  
 除了存储在远程监视服务器上外，与主服务器有关的信息存储在主服务器上的 **log_shipping_monitor_primary** 表中。  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [sp_add_log_shipping_primary_database &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)   
 [sp_change_log_shipping_primary_database &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-change-log-shipping-primary-database-transact-sql.md)   
 [sp_delete_log_shipping_primary_database &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-database-transact-sql.md)   
 [sp_help_log_shipping_primary_database (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-log-shipping-primary-database-transact-sql.md)   
 [sp_refresh_log_shipping_monitor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-refresh-log-shipping-monitor-transact-sql.md)   
 [sp_help_log_shipping_monitor_primary &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-primary-transact-sql.md)   
 [sp_delete_log_shipping_alert_job &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-alert-job-transact-sql.md)   
 [系统表 (Transact-SQL)](../../relational-databases/system-tables/system-tables-transact-sql.md)  
  
  
