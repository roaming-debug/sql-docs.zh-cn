---
description: sp_setsubscriptionxactseqno (Transact-SQL)
title: sp_setsubscriptionxactseqno (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setsubscriptionxactseqno
- sp_setsubscriptionxactseqno_TSQL
helpviewer_keywords:
- sp_setsubscriptionxactseqno
ms.assetid: cdb4e0ba-5370-4905-b03f-0b0c6f080ca6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3ec2589b3cacb2e4426793b6adb7814c03e19fce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176406"
---
# <a name="sp_setsubscriptionxactseqno-transact-sql"></a>sp_setsubscriptionxactseqno (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在故障排除过程中使用，以使用日志序列号 (LSN) 指定上次传递的事务，这允许分发代理在下一个事务中开始传递。 重新启动时，分发代理会从分发数据库缓存中返回大于此水印 (LSN) 的事务 (msrepl_commands) 。 此存储过程在订阅服务器的订阅数据库中执行。 非 SQL Server 订阅服务器不支持该过程。  
  
> [!CAUTION]  
>  如果未正确使用此存储过程或指定了错误的 LSN 值，则将导致分发代理还原已应用于订阅服务器的更改，或跳过所有剩余的更改。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_setsubscriptionxactseqno [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @xact_seqno = ] xact_seqno   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。 对于非 SQL Server 发布服务器， *publisher_db* 是分发数据库的名称。  
  
`[ @publication = ] 'publication'` 发布的名称。 *发布* 为 **sysname**，无默认值。 如果分发代理由多个发布共享，则必须为 " *发布*" 指定一个值。  
  
`[ @xact_seqno = ] xact_seqno` 是要应用于订阅服务器的分发服务器上的下一个事务的 LSN。 *xact_seqno* 为 **varbinary (16)**，无默认值。  
  
## <a name="result-set"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**ORIGINAL XACT_SEQNO**|**varbinary(16)**|要应用于订阅服务器的下一个事务的原始 LSN。|  
|**UPDATED XACT_SEQNO**|**varbinary(16)**|要应用于订阅服务器的下一个事务的更新后的 LSN。|  
|**SUBSCRIPTION STREAM COUNT**|**int**|上次同步期间使用的订阅流数。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_setsubscriptionxactseqno** 用于事务复制。  
  
 不能在对等事务复制拓扑中使用 **sp_setsubscriptionxactseqno** 。  
  
 当应用于订阅服务器时， **sp_setsubscriptionxactseqno** 可以用于跳过导致错误的特定事务。 如果出现故障，在分发代理停止后，请在分发服务器上调用 [sp_helpsubscriptionerrors &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriptionerrors-transact-sql.md) 以检索失败事务的 xact_seqno 值，然后调用 **sp_setsubscriptionxactseqno**，同时为 *xact_seqno* 传递此值。 这将确保只处理此 LSN 之后的命令。  
  
 将 *xact_seqno* 的值指定为 **0** ，将分发数据库中的所有挂起的命令传递到订阅服务器。  
  
 如果分发代理使用多订阅流， **sp_setsubscriptionxactseqno** 可能会失败。  
  
 如果出现此错误，必须运行使用单个订阅流的分发代理。 有关详细信息，请参阅 [Replication Distribution Agent](../../relational-databases/replication/agents/replication-distribution-agent.md)。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_setsubscriptionxactseqno**。  
  
## <a name="see-more"></a>查看详细信息

[博客：如何跳过事务](https://repltalk.com/2019/05/28/how-to-skip-a-transaction/)  
