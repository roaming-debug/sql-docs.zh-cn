---
title: sp_repldone (Transact-sql) |Microsoft Docs
description: 更新用于标识服务器的最后一个已分发事务的记录。 此存储过程在发布服务器的发布数据库上运行。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repldone
- sp_repldone_TSQL
helpviewer_keywords:
- sp_repldone
ms.assetid: 045d3cd1-712b-44b7-a56a-c9438d4077b9
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4889616070df4a42150272e0c9cd17aee941b063
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203152"
---
# <a name="sp_repldone-transact-sql"></a>sp_repldone (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  更新用于标识服务器的最后一个已分发事务的记录。 此存储过程在发布服务器上对发布数据库执行。  
  
> [!CAUTION]  
>  如果手动执行 **sp_repldone**，则可以使已传送的事务的次序和一致性无效。 **sp_repldone** 应仅用于排查有经验的复制支持专业人员指导的复制问题。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```
sp_repldone [ @xactid= ] xactid   
        , [ @xact_seqno= ] xact_seqno   
    [ , [ @numtrans= ] numtrans ]   
    [ , [ @time= ] time   
    [ , [ @reset= ] reset ]  
```  
  
## <a name="arguments"></a>参数  
`[ @xactid = ] xactid` 服务器的最后一个分布式事务的第一条记录 (LSN) 的日志序列号。 *xactid* 是 **二进制 (10)**，无默认值。  
  
`[ @xact_seqno = ] xact_seqno` 服务器最后一次分布式事务的最后一条记录的 LSN。 *xact_seqno* 是 **二进制 (10)**，无默认值。  
  
`[ @numtrans = ] numtrans` 分布式事务的数目。 *numtrans* 的值为 **int**，无默认值。  
  
`[ @time = ] time` 分发最后一批事务所需的毫秒数（如果提供）。 *time* 为 **int**，没有默认值。  
  
`[ @reset = ] reset` 重置状态。 *reset* 是 **int**，没有默认值。 如果为 **1**，则日志中的所有复制的事务都被标记为已分发。 如果为 **0**，则事务日志将重置为第一个复制的事务，并且不会将复制的事务标记为已分发。 仅当 *xactid* 和 *XACT_SEQNO* 都为 NULL 时， *reset* 才有效。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_repldone** 用于事务复制。  
  
 日志读取器进程使用 **sp_repldone** 来跟踪已分发的事务。  
  
 使用 **sp_repldone**，你可以手动通知服务器事务已复制 (发送到分发服务器的) 。 它还允许您更改被标记为下一个等待复制的事务。 您可以在已复制事务的列表中前后移动。 （所有小于或等于该事务的事务都将标记为已分发。）  
  
 可以通过使用 **sp_repltrans** 或 **sp_replcmds** 获取所需的参数 *xactid* 和 *xact_seqno* 。  
  
## <a name="permissions"></a>权限  
 **Sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员可以执行 **sp_repldone**。  
  
## <a name="examples"></a>示例  
 当 *xactid* 为 null 时， *xact_seqno* 为 null，而 *reset* 为 **1**，则日志中的所有复制的事务都被标记为已分发。 当事务日志中存在不再有效的复制事务并且想截断该日志时，此过程很有用，例如：  
  
```sql
EXEC sp_repldone @xactid = NULL, @xact_seqno = NULL, @numtrans = 0, @time = 0, @reset = 1  
```  
  
> [!CAUTION]  
>  可以在紧急情况下使用此过程，以允许在有事务挂起复制时截断事务日志。  
  
## <a name="see-also"></a>另请参阅  
 [sp_replcmds (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replflush &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
