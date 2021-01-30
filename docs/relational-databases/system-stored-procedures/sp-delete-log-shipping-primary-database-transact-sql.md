---
description: sp_delete_log_shipping_primary_database (Transact-SQL)
title: sp_delete_log_shipping_primary_database (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_log_shipping_primary_database
- sp_delete_log_shipping_primary_database_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_log_shipping_primary_database
ms.assetid: cb1d5d00-2805-4d47-bd04-545232067345
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a42bb060044c35e4d2569e23e24eb8ba9bc46fd7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211141"
---
# <a name="sp_delete_log_shipping_primary_database-transact-sql"></a>sp_delete_log_shipping_primary_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此存储过程删除主数据库的日志传送，包括备份作业、本地历史记录以及远程历史记录。 仅在使用 **sp_delete_log_shipping_primary_secondary** 删除辅助数据库后，才使用此存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_delete_log_shipping_primary_database  
[ @database = ] 'database'  
```  
  
## <a name="arguments"></a>参数  
`[ @database = ] 'database'` 日志传送主数据库的名称。 *数据库* 为 **sysname**，无默认值，且不能为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 必须从主服务器上的 **master** 数据库运行 **sp_delete_log_shipping_primary_database** 。 此存储过程执行以下操作：  
  
1.  为指定的主数据库删除备份作业。  
  
2.  删除主服务器上 **log_shipping_monitor_primary** 中的本地监视记录。  
  
3.  删除 **log_shipping_monitor_history_detail** 和 **log_shipping_monitor_error_detail** 中的相应条目。  
  
4.  如果监视服务器不同于主服务器，则会在监视服务器 **log_shipping_monitor_primary** 上删除监视记录。  
  
5.  删除监视服务器 **log_shipping_monitor_history_detail** 和 **log_shipping_monitor_error_detail** 中的相应条目。  
  
6.  删除此主数据库 **log_shipping_primary_databases** 中的条目。  
  
7.  调用监视服务器上 **sp_delete_log_shipping_alert_job** 。  

## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能运行此过程。  
  
## <a name="examples"></a>示例  
 此示例演示如何使用 **sp_delete_log_shipping_primary_database** 删除主数据库 **AdventureWorks2012**。  
  
```  
EXEC master.dbo.sp_delete_log_shipping_primary_database @database = N'AdventureWorks2012';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
