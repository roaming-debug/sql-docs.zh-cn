---
description: sp_add_log_shipping_primary_database (Transact-SQL)
title: sp_add_log_shipping_primary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_primary_database
- sp_add_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_primary_database
ms.assetid: 69531611-113f-46b5-81a6-7bf496d0353c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4c967127366c935b42ac33158982c6734114b2d8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206716"
---
# <a name="sp_add_log_shipping_primary_database-transact-sql"></a>sp_add_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  设置日志传送配置（包括备份作业、本地监视记录及远程监视记录）的主数据库。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_log_shipping_primary_database [ @database = ] 'database',   
[ @backup_directory = ] 'backup_directory',   
[ @backup_share = ] 'backup_share',   
[ @backup_job_name = ] 'backup_job_name',   
[, [ @backup_retention_period = ] backup_retention_period]  
[, [ @monitor_server = ] 'monitor_server']  
[, [ @monitor_server_security_mode = ] monitor_server_security_mode]  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] backup_threshold ]   
[, [ @threshold_alert = ] threshold_alert ]   
[, [ @threshold_alert_enabled = ] threshold_alert_enabled ]   
[, [ @history_retention_period = ] history_retention_period ]  
[, [ @backup_job_id = ] backup_job_id OUTPUT ]  
[, [ @primary_id = ] primary_id OUTPUT]  
[, [ @backup_compression = ] backup_compression_option ]  
  
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database'` 日志传送主数据库的名称。 *数据库* 为 **sysname**，无默认值，且不能为 NULL。  
  
`[ @backup_directory = ] 'backup_directory'` 主服务器上备份文件夹的路径。 *backup_directory* 为 **nvarchar (500)**，无默认值，且不能为 NULL。  
  
`[ @backup_share = ] 'backup_share'` 主服务器上的备份目录的网络路径。 *backup_share* 为 **nvarchar (500)**，无默认值，且不能为 NULL。  
  
`[ @backup_job_name = ] 'backup_job_name'` 将备份复制到备份文件夹中的主服务器上 SQL Server 代理作业的名称。 *backup_job_name* 为 **sysname** ，且不能为 NULL。  
  
`[ @backup_retention_period = ] backup_retention_period` 在主服务器上的备份目录中保留日志备份文件的时间长度（以分钟为单位）。 *backup_retention_period* 为 **int**，没有默认值，且不能为 NULL。  
  
`[ @monitor_server = ] 'monitor_server'` 监视服务器的名称。 *Monitor_server* **sysname**，无默认值，且不能为 NULL。  
  
`[ @monitor_server_security_mode = ] monitor_server_security_mode` 用于连接到监视服务器的安全模式。  
  
 1 = Windows 身份验证。  
  
 0 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证。 *monitor_server_security_mode* 是 **bit** ，并且不能为 NULL。  
  
`[ @monitor_server_login = ] 'monitor_server_login'` 用于访问监视服务器的帐户的用户名。  
  
`[ @monitor_server_password = ] 'monitor_server_password'` 用于访问监视服务器的帐户的密码。  
  
`[ @backup_threshold = ] backup_threshold` 在引发 *threshold_alert* 错误之前，最后一次备份之后的时间长度（以分钟为单位）。 *backup_threshold* 的值为 **int**，默认值为60分钟。  
  
`[ @threshold_alert = ] threshold_alert` 超过备份阈值时引发的警报。 *threshold_alert* 的值为 **int**，默认值为14420。  
  
`[ @threshold_alert_enabled = ] threshold_alert_enabled` 指定在超出 *backup_threshold* 时是否引发警报。 默认值零 (0) 表示警报被禁用，将不会引发警报。 *threshold_alert_enabled* 为 **bit**。  
  
`[ @history_retention_period = ] history_retention_period` 将保留历史记录的时间长度（分钟）。 *history_retention_period* 的值为 **int**，默认值为 NULL。 如果未指定值，则使用值 14420。  
  
`[ @backup_job_id = ] backup_job_id OUTPUT`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]与主服务器上的备份作业相关联的代理作业 ID。 *backup_job_id* 为 **uniqueidentifier** ，且不能为 NULL。  
  
`[ @primary_id = ] primary_id OUTPUT` 日志传送配置的主数据库 ID。 *primary_id* 为 **uniqueidentifier** ，且不能为 NULL。  
  
`[ @backup_compression = ] backup_compression_option` 指定日志传送配置是否使用 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 只有 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]（或更高版本）支持此参数。  
  
 0 = 禁用。 从不压缩日志备份。  
  
 1 = 启用。 始终压缩日志备份。  
  
 2 = 使用视图的设置 [或配置备份压缩默认服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。 这是默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从主服务器上的 **master** 数据库运行 **sp_add_log_shipping_primary_database** 。 此存储过程可执行以下功能：  
  
1.  使用提供的参数生成主 ID 并为表中的主数据库添加条目 **log_shipping_primary_databases** 。  
  
2.  为被禁用的主数据库创建一个备份作业。  
  
3.  将 **log_shipping_primary_databases** 条目中的备份作业 id 设置为备份作业的作业 id。  
  
4.  使用提供的参数，在主服务器上的表 **log_shipping_monitor_primary** 中添加本地监视记录。  
  
5.  如果监视服务器不同于主服务器，则使用提供的参数在监视服务器上 **log_shipping_monitor_primary** 中添加一条监视器记录。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例将在日志传送配置中添加 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库作为主数据库。  
  
```  
DECLARE @LS_BackupJobId AS uniqueidentifier ;  
DECLARE @LS_PrimaryId AS uniqueidentifier ;  
  
EXEC master.dbo.sp_add_log_shipping_primary_database   
@database = N'AdventureWorks'   
,@backup_directory = N'c:\lsbackup'   
,@backup_share = N'\\tribeca\lsbackup'   
,@backup_job_name = N'LSBackup_AdventureWorks'   
,@backup_retention_period = 1440  
,@monitor_server = N'rockaway'   
,@monitor_server_security_mode = 1   
,@backup_threshold = 60   
,@threshold_alert = 0   
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440   
,@backup_job_id = @LS_BackupJobId OUTPUT   
,@primary_id = @LS_PrimaryId OUTPUT   
,@overwrite = 1   
,@backup_compression = 0;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
