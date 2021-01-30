---
description: sp_help_log_shipping_primary_database (Transact-SQL)
title: sp_help_log_shipping_primary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_log_shipping_primary_database_TSQL
- sp_help_log_shipping_primary_database
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_log_shipping_primary_database
ms.assetid: e711b01c-ef29-4eb6-a016-0e647e337818
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: bbae989332c0aa998699aacd4f8e9d16f61e0180
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200070"
---
# <a name="sp_help_log_shipping_primary_database-transact-sql"></a>sp_help_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  检索主数据库设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_log_shipping_primary_database  
[ @database = ] 'database' OR  
[ @primary_id = ] 'primary_id'  
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database'` 日志传送主数据库的名称。 *数据库* 为 **sysname**，无默认值，且不能为 NULL。  
  
`[ @primary_id = ] 'primary_id'` 日志传送配置的主数据库 ID。 *primary_id* 为 **uniqueidentifier** ，且不能为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|说明|  
|-----------------|-----------------|  
|**primary_id**|日志传送配置的主数据库 ID。|  
|**primary_database**|日志传送配置中主数据库的名称。|  
|**backup_directory**|存储主服务器的事务日志备份文件的目录。|  
|**backup_share**|备份目录的网络或 UNC 路径。|  
|**backup_retention_period**|日志备份文件在删除之前保留在备份目录中的时间长度（分钟）。|  
|**backup_compression**|指示日志传送配置是否使用 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。<br /><br /> **0** = 禁用。 从不压缩日志备份。<br /><br /> **1** = 已启用。 始终压缩日志备份。<br /><br /> **2** = 使用视图的设置 [或配置备份压缩默认服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。 这是默认值。<br /><br /> 只有 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]（或更高版本）支持备份压缩。 在其他版本中，该值始终为 2。|  
|**backup_job_id**|与主服务器上的备份作业相关联的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业 ID。|  
|**monitor_server**|在日志传送配置中用作监视服务器的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 实例的名称。|  
|**monitor_server_security_mode**|用于连接到监视服务器的安全模式。<br /><br /> 1 = [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 身份验证。<br /><br /> 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。|  
|**backup_threshold**|备份操作之间的占用时间阈值（分钟），一旦超过此值，就会生成警报。|  
|**threshold_alert**|超过备份阈值时引发的警报。|  
|**threshold_alert_enabled**|确定是否启用备份阈值警报。<br /><br /> **1** = 已启用。<br /><br /> **0** = 禁用。|  
|**last_backup_file**|最近一次事务日志备份的绝对路径。|  
|**last_backup_date**|上一次日志备份操作的时间和日期。|  
|**last_backup_date_utc**|上一次在主数据库上执行事务日志备份操作的时间和日期，使用协调世界时表示。|  
|**history_retention_period**|日志传送历史记录在删除前保留在给定主数据库中的时间（分钟）。|  
  
## <a name="remarks"></a>备注  
 必须从主服务器上的 **master** 数据库运行 **sp_help_log_shipping_primary_database** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例演示如何使用 **sp_help_log_shipping_primary_database** 检索数据库的主数据库设置 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 。  
  
```  
EXEC master.dbo.sp_help_log_shipping_primary_database @database=N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
