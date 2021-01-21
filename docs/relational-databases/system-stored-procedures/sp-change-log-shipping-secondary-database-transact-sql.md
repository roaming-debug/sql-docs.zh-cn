---
description: sp_change_log_shipping_secondary_database (Transact-SQL)
title: sp_change_log_shipping_secondary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_change_log_shipping_secondary_database
- sp_change_log_shipping_secondary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_change_log_shipping_secondary_database
ms.assetid: 3ebcf2f1-980f-4543-a84b-fbaeea54eeac
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 81a5c562ddade5c60458ad491a401dab916c59d4
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620291"
---
# <a name="sp_change_log_shipping_secondary_database-transact-sql"></a>sp_change_log_shipping_secondary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改辅助数据库设置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_change_log_shipping_secondary_database  
[ @secondary_database = ] 'secondary_database',  
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
`[ @restore_delay = ] 'restore_delay'` 辅助服务器在还原给定备份文件之前等待的时间，以分钟为单位。 *restore_delay* 为 **int** ，且不能为 NULL。 默认值为 0。  
  
`[ @restore_all = ] 'restore_all'` 如果设置为1，则在还原作业运行时，辅助服务器将还原所有可用的事务日志备份。 否则，在原还了一个文件之后它将停止。 *restore_all* 是 **bit** ，并且不能为 NULL。  
  
`[ @restore_mode = ] 'restore_mode'` 辅助数据库的还原模式。  
  
 0 = 使用 NORECOVERY 还原日志。  
  
 1 = 用备用还原日志。  
  
 *restore* 为 **bit** ，并且不能为 NULL。  
  
`[ @disconnect_users = ] 'disconnect_users'` 如果设置为1，则在执行还原操作时，用户将与辅助数据库断开连接。 默认值 = 0。 *disconnect_users* 是 **bit** ，并且不能为 NULL。  
  
`[ @block_size = ] 'block_size'` 用作备份设备的块大小的大小（以字节为单位）。 *block_size* 为 **int** ，默认值为-1。  
  
`[ @buffer_count = ] 'buffer_count'` 备份或还原操作使用的缓冲区总数。 *buffer_count* 为 **int** ，默认值为-1。  
  
`[ @max_transfer_size = ] 'max_transfer_size'` 向备份设备发出的最大输入或输出请求的大小（以字节为单位） [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *max_transfersize* 为 **int** ，并且可以为 NULL。  
  
`[ @restore_threshold = ] 'restore_threshold'` 在生成警报之前，还原操作允许间隔的分钟数。 *restore_threshold* 为 **int** ，且不能为 NULL。  
  
`[ @threshold_alert = ] 'threshold_alert'` 超过还原阈值时引发的警报。 *threshold_alert* 的值为 **int**，默认值为14421。  
  
`[ @threshold_alert_enabled = ] 'threshold_alert_enabled'` 指定在超出 *restore_threshold* 时是否引发警报。 1 = 启用；0 = 禁用。 *threshold_alert_enabled* 是 **bit** ，并且不能为 NULL。  
  
`[ @history_retention_period = ] 'history_retention_period'` 将保留历史记录的时间长度（分钟）。 *history_retention_period* 是 **int**。如果未指定值1440，则将使用该值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 必须从辅助服务器上的 **master** 数据库运行 **sp_change_log_shipping_secondary_database** 。 此存储过程执行以下操作：  
  
1.  必要时，更改 **log_shipping_secondary_database** 记录中的设置。  
  
2.  必要时，使用提供的参数更改辅助服务器上 **log_shipping_monitor_secondary** 中的本地监视器记录。  

## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例演示如何使用 **sp_change_log_shipping_secondary_database** 来更新数据库 **LogShipAdventureWorks** 的辅助数据库参数。  
  
```  
EXEC master.dbo.sp_change_log_shipping_secondary_database   
 @secondary_database =  'LogShipAdventureWorks'  
,  @restore_delay = 0  
,  @restore_all = 1  
,  @restore_mode = 0  
,  @disconnect_users = 0  
,  @threshold_alert = 14420  
,  @threshold_alert_enabled = 1  
,  @history_retention_period = 14420;  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
