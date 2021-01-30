---
description: sp_add_log_shipping_secondary_database (Transact-SQL)
title: sp_add_log_shipping_secondary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_log_shipping_secondary_database
- sp_add_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_log_shipping_secondary_database
ms.assetid: d29e1c24-3a3c-47a4-a726-4584afa6038a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d13a58428c4fefe2cc19668de22e604f721d0a96
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206707"
---
# <a name="sp_add_log_shipping_secondary_database-transact-sql"></a>sp_add_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为日志传送设置辅助数据库。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_add_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
[ @primary_server = ] 'primary_server',   
[ @primary_database = ] 'primary_database',  
[, [ @restore_delay = ] 'restore_delay']  
[, [ @restore_all = ] 'restore_all']  
[, [ @restore_mode = ] 'restore_mode']  
[, [ @disconnect_users = ] 'disconnect_users']  
[, [ @block_size = ] 'block_size']  
[, [ @buffer_count = ] 'buffer_count']  
[, [ @max_transfer_size = ] 'max_transfer_size']  
[, [ @restore_threshold = ] 'restore_threshold']   
[, [ @threshold_alert = ] 'threshold_alert']   
[, [ @threshold_alert_enabled = ] 'threshold_alert_enabled']   
[, [ @history_retention_period = ] 'history_retention_period']  
```  
  
## <a name="arguments"></a>参数  
`[ @secondary_database = ] 'secondary_database'` 辅助数据库的名称。 *secondary_database* **sysname**，无默认值。  
  
`[ @primary_server = ] 'primary_server'`[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 日志传送配置中的主实例的名称。 *primary_server* 为 **sysname** ，且不能为 NULL。  
  
`[ @primary_database = ] 'primary_database'` 主服务器上的数据库的名称。 *primary_database* **sysname**，无默认值。  
  
`[ @restore_delay = ] 'restore_delay'` 辅助服务器在还原给定备份文件之前等待的时间，以分钟为单位。 *restore_delay* 为 **int** ，且不能为 NULL。 默认值为 0。  
  
`[ @restore_all = ] 'restore_all'` 如果设置为1，则在还原作业运行时，辅助服务器将还原所有可用的事务日志备份。 否则，辅助服务器将在还原一个文件后停止。 *restore_all* 是 **bit** ，并且不能为 NULL。  
  
`[ @restore_mode = ] 'restore_mode'` 辅助数据库的还原模式。  
  
 0 = 用 NORECOVERY 还原日志。  
  
 1 = 用备用还原日志。  
  
 *restore* 为 **bit** ，并且不能为 NULL。  
  
`[ @disconnect_users = ] 'disconnect_users'` 如果设置为1，则在执行还原操作时，用户将与辅助数据库断开连接。 默认值 = 0。 *断开连接* 的用户是 **bit** ，并且不能为 NULL。  
  
`[ @block_size = ] 'block_size'` 用作备份设备的块大小的大小（以字节为单位）。 *block_size* 为 **int** ，默认值为-1。  
  
`[ @buffer_count = ] 'buffer_count'` 备份或还原操作使用的缓冲区总数。 *buffer_count* 为 **int** ，默认值为-1。  
  
`[ @max_transfer_size = ] 'max_transfer_size'` 向备份设备发出的最大输入或输出请求的大小（以字节为单位） [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *max_transfersize* 为 **int** ，并且可以为 NULL。  
  
`[ @restore_threshold = ] 'restore_threshold'` 在生成警报之前，还原操作允许间隔的分钟数。 *restore_threshold* 为 **int** ，且不能为 NULL。  
  
`[ @threshold_alert = ] 'threshold_alert'` 超过备份阈值时引发的警报。 *threshold_alert* 的值为 **int**，默认值为14420。  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` 指定在超出 *backup_threshold* 时是否引发警报。 默认值一 (1) 表示要引发警告。 *threshold_alert_enabled* 为 **bit**。  
  
`[ @history_retention_period = ] 'history_retention_period'` 保留历史记录的时间长度（分钟）。 *history_retention_period* 的值为 **int**，默认值为 NULL。 如果不指定值，则使用值 14420。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从辅助服务器上的 **master** 数据库运行 **sp_add_log_shipping_secondary_database** 。 此存储过程执行以下操作：  
  
1.  在此存储过程之前，应调用 **sp_add_log_shipping_secondary_primary** ，以初始化辅助服务器上的主日志传送数据库信息。  
  
2.  使用提供的参数在 **log_shipping_secondary_databases** 中添加辅助数据库的条目。  
  
3.  使用提供的参数在辅助服务器上 **log_shipping_monitor_secondary** 中添加本地监视记录。  
  
4.  如果监视服务器不同于辅助服务器，则使用提供的参数在监视服务器上 **log_shipping_monitor_secondary** 中添加监视记录。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例演示如何使用 **sp_add_log_shipping_secondary_database** 存储过程将数据库 **LogShipAdventureWorks** 添加为日志传送配置中的辅助数据库，主数据库 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 驻留在主服务器 TRIBECA 上。  
  
```  
EXEC master.dbo.sp_add_log_shipping_secondary_database   
@secondary_database = N'LogShipAdventureWorks'   
,@primary_server = N'TRIBECA'   
,@primary_database = N'AdventureWorks2012'   
,@restore_delay = 0   
,@restore_mode = 1   
,@disconnect_users = 0   
,@restore_threshold = 45     
,@threshold_alert_enabled = 0   
,@history_retention_period = 1440 ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
