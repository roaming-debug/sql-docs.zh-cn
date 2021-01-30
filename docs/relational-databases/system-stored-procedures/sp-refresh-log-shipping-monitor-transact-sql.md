---
description: sp_refresh_log_shipping_monitor (Transact-SQL)
title: sp_refresh_log_shipping_monitor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_refresh_log_shipping_monitor
- sp_refresh_log_shipping_monitor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refresh_log_shipping_monitor
ms.assetid: edefb912-31c5-4d99-9aba-06629afd0171
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 16c1fb1de57fb0e1c61410071ec8fde31951f2ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185717"
---
# <a name="sp_refresh_log_shipping_monitor-transact-sql"></a>sp_refresh_log_shipping_monitor (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此存储过程使用指定日志传送代理的给定主服务器或辅助服务器中的最新信息来刷新远程监视器表。 此过程将在主服务器或辅助服务器上被调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_refresh_log_shipping_monitor  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
[ @database = ] 'database'  
[ @mode ] n  
```  
  
## <a name="arguments"></a>参数  
`[ @agent_id = ] 'agent_id'` 用于备份的主 ID 或者用于复制或还原的辅助 ID。 *agent_id* 为 **uniqueidentifier** ，且不能为 NULL。  
  
`[ @agent_type = ] 'agent_type'` 日志传送作业的类型。  
  
 0 = 备份。  
  
 1 = 复制。  
  
 2 = 还原。  
  
 *agent_type* 为 **tinyint** ，且不能为 NULL。  
  
`[ @database = ] 'database'` 备份或还原代理按日志记录使用的主或辅助数据库。  
  
`[ @mode ] n` 指定是否刷新或清除监视器数据。 *M* 的数据类型为 tinyint，支持的值为：  
  
 1 = 刷新（默认值）。  
  
 2 = 删除  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 **sp_refresh_log_shipping_monitor** 将用尚未传输的任何会话信息刷新 **log_shipping_monitor_primary**、 **log_shipping_monitor_secondary**、 **log_shipping_monitor_history_detail** 和 **log_shipping_monitor_error_detail** 表。 当监视已有一段时间不同步时，您便可以使监视服务器与主服务器或辅助服务器同步。 此外，如果需要，还可以从监视服务器中清除监视信息。  
  
 必须从主服务器或辅助服务器上的 **master** 数据库运行 **sp_refresh_log_shipping_monitor** 。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
