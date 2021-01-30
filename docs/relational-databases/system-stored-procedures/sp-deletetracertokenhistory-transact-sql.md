---
description: sp_deletetracertokenhistory (Transact-SQL)
title: sp_deletetracertokenhistory (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletetracertokenhistory
- sp_deletetracertokenhistory_TSQL
helpviewer_keywords:
- sp_deletetracertokenhistory
ms.assetid: 9ae1be14-0d2f-40b1-9d6e-22d79726abf4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5bc5a6d5191f89d8b31b48939c83674bf0302e15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178161"
---
# <a name="sp_deletetracertokenhistory-transact-sql"></a>sp_deletetracertokenhistory (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

删除 [MStracer_tokens &#40;transact-sql&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) 的跟踪令牌记录，并 [MStracer_history &#40;transact-sql&#41;](../../relational-databases/system-tables/mstracer-history-transact-sql.md) 系统表。 此存储过程在发布服务器上的发布数据库中执行，或者在分发服务器上的分发数据库中执行。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```
sp_deletetracertokenhistory [ @publication = ] 'publication'
    [ , [ @tracer_id = ] tracer_id ]
    [ , [ @cutoff_date = ] cutoff_date ]
    [ , [ @publisher = ] 'publisher' ]
    [ , [ @publisher_db = ] 'publisher_db' ]
```

## <a name="arguments"></a>参数

`@publication= 'publication'`  
已插入跟踪令牌的发布的名称。 数据类型为 **sysname**。 此参数是必需的。

`[ @tracer_id= ] tracer_id`  
要删除的跟踪令牌的 ID。 数据类型为 **int**。默认值为 *null*。 如果 *为 null*，则删除属于该发布的所有跟踪令牌。

`[ @cutoff_date= ] cutoff_date`  
删除此日期之前插入到发布中的跟踪令牌。 数据类型为 **datetime**。 默认值为 *null*。

`[ @publisher= ] 'publisher'`  
发布服务器的名称。 数据类型为 **sysname**。 默认值为 *null*。

> [!NOTE]
> 只应为非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器或从分发服务器执行存储过程时指定此参数。

`[ @publisher_db= ] 'publisher_db'`  
发布数据库的名称。 数据类型为 **sysname**。 默认值为 NULL。 如果在发布服务器上执行该存储过程，将忽略此参数。

> [!NOTE]
> 从分发服务器执行存储过程时，应指定此参数。

## <a name="return-code-values"></a>返回代码值

**0** (成功) 或 **1** (失败) 

## <a name="remarks"></a>备注

**sp_deletetracertokenhistory** 用于事务复制。  

如果同时指定 *tracer_id* 和 *cutoff_date* 参数，则会出现错误。

如果不执行 **sp_deletetracertokenhistory** 来删除跟踪令牌元数据，则在进行定期计划的历史记录清除时将删除此信息。

跟踪令牌 Id 可以通过执行 [sp_helptracertokens &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md) 来确定，也可以通过查询 [MStracer_tokens &#40;transact-sql&#41;](../../relational-databases/system-tables/mstracer-tokens-transact-sql.md) 系统表来确定。

## <a name="permissions"></a>权限

只有以下人员才有权执行 **sp_deletetracertokenhistory**：

- 分发数据库中 **replmonitor** 角色的成员
- **Sysadmin** 固定服务器角色的成员。
- 发布数据库中 **db_owner** 固定数据库角色的成员。
- 固定数据库的 **db_owner** 。

## <a name="see-also"></a>另请参阅

[为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)

[sp_helptracertokenhistory &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md)
