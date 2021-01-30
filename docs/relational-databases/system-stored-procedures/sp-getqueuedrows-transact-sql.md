---
description: sp_getqueuedrows (Transact-SQL)
title: sp_getqueuedrows (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_getqueuedrows_TSQL
- sp_getqueuedrows
helpviewer_keywords:
- sp_getqueuedrows
ms.assetid: 139e834f-1988-4b4d-ac81-db1f89ea90e8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ad904065a8270266e7010ff49889d04a8625296f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183141"
---
# <a name="sp_getqueuedrows-transact-sql"></a>sp_getqueuedrows (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在订阅服务器上检索在队列中有未决更新的行。 此存储过程在订阅服务器的订阅数据库中执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_getqueuedrows [ @tablename = ] 'tablename'  
    [ , [ @owner = ] 'owner'  
    [ , [ @tranid = ] 'transaction_id' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @tablename = ] 'tablename'` 表的名称。 *tablename* 的值为 **sysname**，无默认值。 该表必须是排队订阅的一部分。  
  
`[ @owner = ] 'owner'` 是订阅所有者。 *所有者* 为 **sysname**，默认值为 NULL。  
  
`[ @tranid = ] 'transaction_id'` 允许按事务 ID 筛选输出。 *transaction_id* 为 **nvarchar (70)**，默认值为 NULL。 如果已指定，则显示与排队命令关联的事务 ID。 如果为 NULL，则显示队列中的所有命令。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 显示所有当前对订阅表至少有一个排队事务的行。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**Action**|**nvarchar (10)**|同步发生时采取的操作类型。<br /><br /> INS= 插入 <br /><br /> DEL = 删除<br /><br /> UPD = 更新|  
|**Tranid**|**nvarchar (70)**|执行命令的事务 ID。|  
|**table column1...n**||*Tablename* 中指定的表的每一列的值。|  
|**msrepl_tran_version**|**uniqueidentifier**|该列用于跟踪对已复制数据的更改以及在发布服务器上执行冲突检测。 该列自动添加到表中。|  
  
## <a name="remarks"></a>备注  
 **sp_getqueuedrows** 用于参与排队更新的订阅服务器。  
  
 **sp_getqueuedrows** 在已参与排队更新的订阅数据库上查找给定表的行，但目前尚未被队列读取器代理解析。  
  
## <a name="permissions"></a>权限  
 **sp_getqueuedrows** 要求对 *tablename* 中指定的表具有 SELECT 权限。  
  
## <a name="see-also"></a>另请参阅  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [排队更新冲突的检测和解决](../../relational-databases/replication/transactional/updatable-subscriptions-queued-updating-conflict-resolution.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
