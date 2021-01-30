---
description: sp_change_log_shipping_primary_database (Transact-SQL)
title: sp_change_log_shipping_primary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_change_log_shipping_primary_database
- sp_change_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_primary_database
ms.assetid: 8c9dce6b-d2a3-4ca7-a832-8f59a5adb214
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e1bcd150ff810f5a4a16deb09a7ea72956ccd681
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181361"
---
# <a name="sp_change_log_shipping_primary_database-transact-sql"></a>sp_change_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改主数据库设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_change_log_shipping_primary_database [ @database = ] 'database'  
[, [ @backup_directory = ] 'backup_directory']   
[, [ @backup_share = ] 'backup_share']   
[, [ @backup_retention_period = ] 'backup_retention_period']  
[, [ @monitor_server_security_mode = ] 'monitor_server_security_mode']  
[, [ @monitor_server_login = ] 'monitor_server_login']  
[, [ @monitor_server_password = ] 'monitor_server_password']  
[, [ @backup_threshold = ] 'backup_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
[, [ @backup_compression = ] backup_compression_option ]   
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database'` 主服务器上的数据库的名称。 *primary_database* **sysname**，无默认值。  
  
`[ @backup_directory = ] 'backup_directory'` 主服务器上备份文件夹的路径。 *backup_directory* 为 **nvarchar (500)**，无默认值，且不能为 NULL。  
  
`[ @backup_share = ] 'backup_share'` 主服务器上的备份目录的网络路径。 *backup_share* 为 **nvarchar (500)**，无默认值，且不能为 NULL。  
  
`[ @backup_retention_period = ] 'backup_retention_period'` 在主服务器上的备份目录中保留日志备份文件的时间长度（以分钟为单位）。 *backup_retention_period* 为 **int**，没有默认值，且不能为 NULL。  
  
`[ @monitor_server_security_mode = ] 'monitor_server_security_mode'` 用于连接到监视服务器的安全模式。  
  
 1 = Windows 身份验证。  
  
 0 = SQL Server 身份验证。  
  
 *monitor_server_security_mode* 是 **bit** ，并且不能为 NULL。  
  
`[ @monitor_server_login = ] 'monitor_server_login'` 用于访问监视服务器的帐户的用户名。  
  
`[ @monitor_server_password = ] 'monitor_server_password'` 用于访问监视服务器的帐户的密码。  
  
`[ @backup_threshold = ] 'backup_threshold'` 在引发 *threshold_alert* 错误之前，最后一次备份之后的时间长度（以分钟为单位）。 *backup_threshold* 的值为 **int**，默认值为60分钟。  
  
`[ @threshold_alert = ] 'threshold_alert'` 超过备份阈值时引发的警报。 *threshold_alert* 为 **int** ，且不能为 NULL。  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` 指定在超出 *backup_threshold* 时是否引发警报。  
  
 1 = 启用。  
  
 0 = 禁用。  
  
 *threshold_alert_enabled* 是 **bit** ，并且不能为 NULL。  
  
`[ @history_retention_period = ] 'history_retention_period'` 保留历史记录的时间长度（分钟）。 *history_retention_period* 是 **int**。如果未指定值14420，则使用该值。  
  
`[ @backup_compression = ] backup_compression_option` 指定日志传送配置是否使用 [备份压缩](../../relational-databases/backup-restore/backup-compression-sql-server.md)。 只有 [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)]（或更高版本）支持此参数。  
  
 0 = 禁用。 从不压缩日志备份。  
  
 1 = 启用。 始终压缩日志备份。  
  
 2 = 使用视图的设置 [或配置备份压缩默认服务器配置选项](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)。 这是默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从主服务器上的 **master** 数据库运行 **sp_change_log_shipping_primary_database** 。 此存储过程执行以下操作：  
  
1.  如有必要，更改 **log_shipping_primary_database** 记录中的设置。  
  
2.  必要时，使用提供的参数更改主服务器上 **log_shipping_monitor_primary** 中的本地记录。  
  
3.  如果监视服务器与主服务器不同，则使用提供的参数更改监视服务器 **log_shipping_monitor_primary** 中的记录（如有必要）。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例演示如何使用 **sp_change_log_shipping_primary_database** 来更新与主数据库关联的设置 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 。  
  
```  
EXEC master.dbo.sp_change_log_shipping_primary_database   
 @database = N'AdventureWorks'   
, @backup_directory = N'c:\LogShipping'   
, @backup_share = N'\\tribeca\LogShipping'   
, @backup_retention_period = 1440   
, @backup_threshold = 60   
, @threshold_alert = 0   
, @threshold_alert_enabled = 1   
, @history_retention_period = 1440   
,@monitor_server_security_mode = 1  
,@backup_compression = 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [log_shipping_primary_databases (Transact-SQL)](../../relational-databases/system-tables/log-shipping-primary-databases-transact-sql.md)  
  
  
