---
description: sp_help_log_shipping_monitor_secondary (Transact-SQL)
title: sp_help_log_shipping_monitor_secondary (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_secondary
- sp_help_log_shipping_monitor_secondary_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor_secondary
ms.assetid: 3ac091ea-c9a8-4c05-a0b6-1ccf4e001339
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7a16b96c6edd21e2af3a78b5fa8f91e2c6b98880
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200057"
---
# <a name="sp_help_log_shipping_monitor_secondary-transact-sql"></a>sp_help_log_shipping_monitor_secondary (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从监视表返回关于辅助数据库的信息。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_log_shipping_monitor_secondary  
[ @secondary_server = ] 'secondary_server',  
[ @secondary_database = ] 'secondary_database'  
```  
  
## <a name="arguments"></a>参数  
`[ @secondary_server = ] 'secondary_server'` 辅助服务器的名称。 *secondary_server* **sysname**，无默认值。  
  
`[ @secondary_database = ] 'secondary_database'` 辅助数据库的名称。 *secondary_database* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列|说明|  
|------------|-----------------|  
|**secondary_server**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 日志传送配置中的辅助实例的名称。|  
|**secondary_database**|日志传送配置中辅助数据库的名称。|  
|**secondary_id**|日志传送配置中辅助服务器的 ID。|  
|**primary_server**|日志传送配置中 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]主实例的名称。|  
|**primary_database**|日志传送配置中主数据库的名称。|  
|**restore_threshold**|两次还原操作之间允许的间隔时间（分钟），一旦超过此值，就会生成警报。|  
|**threshold_alert**|超过还原阈值时引发的警报。|  
|**threshold_alert_enabled**|确定是否启用了还原阈值警报。<br /><br /> 1 = 启用。<br /><br /> 0 = 禁用。|  
|**last_copied_file**|上次复制到辅助服务器的备份文件的文件名。|  
|**last_copied_date**|上次复制到辅助服务器的时间和日期。|  
|**last_copied_date_utc**|上次对辅助服务器执行复制操作的时间和日期，以通用协调时间表示。|  
|**last_restored_file**|上次还原到辅助数据库的备份文件的文件名。|  
|**last_restored_date**|上次对辅助数据库执行还原操作的时间和日期。|  
|**last_restored_date_utc**|上次对辅助数据库执行还原操作的时间和日期，以通用协调时间表示。|  
|**history_retention_period**|日志传送历史记录在删除前保留在给定辅助数据库中的时间（分钟）。|  
  
## <a name="remarks"></a>备注  
 必须从监视服务器上的 **master** 数据库运行 **sp_help_log_shipping_monitor_secondary** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="see-also"></a>请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
