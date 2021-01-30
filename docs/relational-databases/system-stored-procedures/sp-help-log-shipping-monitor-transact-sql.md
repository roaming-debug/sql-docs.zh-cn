---
description: sp_help_log_shipping_monitor (Transact-SQL)
title: sp_help_log_shipping_monitor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_monitor_TSQL
- sp_help_log_shipping_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_monitor
ms.assetid: a4e96c45-6dcd-471a-a494-b5c619459855
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4f8e1dbff1f96489895e95899d8595e8686e5c0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200048"
---
# <a name="sp_help_log_shipping_monitor-transact-sql"></a>sp_help_log_shipping_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回一个结果集，其中包含主服务器、辅助服务器或监视服务器上注册的主数据库和辅助数据库的状态和其他信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_log_shipping_monitor  
```  
  
## <a name="arguments"></a>参数  
 无。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**status**|**bit**|日志传送数据库代理的共同状态。<br /><br /> **0** = 正常和无代理故障。<br /><br /> **1** = 否则为。|  
|**is_primary**|**bit**|指示该行是否用于主数据库：<br /><br /> **1** = 该行用于主数据库。<br /><br /> **0** = 该行适用于辅助数据库。|  
|服务器|**sysname**|此数据库所在的主服务器或辅助服务器的名称。|  
|**database_name**|**sysname**|数据库名称。|  
|**time_since_last_backup**|**int**|最后一次备份日志以来的时间，以分钟为单位。<br /><br /> NULL = 信息不可用或者不相关。|  
|**last_backup_file**|**nvarchar (500)**|上一个成功的备份日志文件的名称。<br /><br /> NULL = 信息不可用或者不相关。|  
|**backup_threshold**|**int**|上一次备份到引发 threshold_alert 错误之间的时间，以分钟为单位。 **backup_threshold** 的值为 **int**，默认值为 **60 分钟**。<br /><br /> NULL = 信息不可用或者不相关。<br /><br /> 可以使用 [sp_add_log_shipping_primary_database &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)更改此值。|  
|**is_backup_alert_enabled**|**bit**|指定在超出 **backup_threshold** 时是否引发警报。 一个 (**1**) 的值（默认值）表示将引发警报。<br /><br /> NULL = 信息不可用或者不相关。<br /><br /> 可以使用 [sp_add_log_shipping_primary_database &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md)更改此值。|  
|**time_since_last_copy**|**int**|上次复制日志备份以来的时间，以分钟为单位。<br /><br /> NULL = 信息不可用或者不相关。|  
|**last_copied_file**|**nvarchar (500)**|上一次成功复制的日志备份文件的名称。<br /><br /> NULL = 信息不可用或者不相关。|  
|**time_since_last_restore**|**int**|上一次还原日志备份以来的时间，以分钟为单位。<br /><br /> NULL = 信息不可用或者不相关。|  
|**last_restored_file**|**nvarchar (500) 。**|上一次成功还原的日志备份文件的名称。<br /><br /> NULL = 信息不可用或者不相关。|  
|**last_restored_latency**|**int**|上一次创建备份到还原该备份的时间，以分钟为单位。<br /><br /> NULL = 信息不可用或者不相关。|  
|**restore_threshold**|**int**|两次还原操作之间允许的间隔时间（分钟），一旦超过此值，就会生成警报。 **restore_threshold** 不能为 NULL。|  
|**is_restore_alert_enabled**|**bit**|指定在超出 **restore_threshold** 时是否引发警报。 一个 (**1**) 的值（默认值）表示将引发警报。<br /><br /> NULL = 信息不可用或者不相关。<br /><br /> 若要设置还原阈值，请使用 [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md)。|  
  
## <a name="remarks"></a>备注  
 必须从监视服务器上的 **master** 数据库运行 **sp_help_log_shipping_monitor** 。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
