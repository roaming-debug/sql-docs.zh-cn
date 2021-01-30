---
description: sp_browsereplcmds (Transact-SQL)
title: sp_browsereplcmds (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_browsereplcmds_TSQL
- sp_browsereplcmds
helpviewer_keywords:
- sp_browsereplcmds
ms.assetid: 30abcb41-1d18-4f43-a692-4c80914c0450
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f798dcb9689221d9a8ef1964d4237a1fe649a4c5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206623"
---
# <a name="sp_browsereplcmds-transact-sql"></a>sp_browsereplcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回分发数据库中存储的可读版本复制命令的结果集，并将其用作诊断工具。 此存储过程在分发服务器上对分发数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_browsereplcmds [ [ @xact_seqno_start = ] 'xact_seqno_start' ]  
    [ , [ @xact_seqno_end = ] 'xact_seqno_end' ]   
    [ , [ @originator_id = ] 'originator_id' ]  
    [ , [ @publisher_database_id = ] 'publisher_database_id' ]  
    [ , [ @article_id = ] 'article_id' ]  
    [ , [ @command_id= ] command_id ]  
    [ , [ @agent_id = ] agent_id ]  
    [ , [ @compatibility_level = ] compatibility_level ]  
```  
  
## <a name="arguments"></a>参数  
`[ @xact_seqno_start = ] 'xact_seqno_start'` 指定要返回的最低值确切序列号。 *xact_seqno_start* 为 **nchar (22)**，默认值为0x00000000000000000000。  
  
`[ @xact_seqno_end = ] 'xact_seqno_end'` 指定要返回的最大确切序列号。 *xact_seqno_end* 为 **nchar (22)**，默认值为0xFFFFFFFFFFFFFFFFFFFF。  
  
`[ @originator_id = ] 'originator_id'` 指定是否返回具有指定 *originator_id* 的命令。 *originator_id* 的值为 **int**，默认值为 NULL。  
  
`[ @publisher_database_id = ] 'publisher_database_id'` 指定是否返回具有指定 *publisher_database_id* 的命令。 *publisher_database_id* 的值为 **int**，默认值为 NULL。  
  
`[ @article_id = ] 'article_id'` 指定是否返回具有指定 *article_id* 的命令。 *article_id* 的值为 **int**，默认值为 NULL。  
  
`[ @command_id = ] command_id` 命令在 MSrepl_commands &#40;要解码的 [transact-sql&#41;](../../relational-databases/system-tables/msrepl-commands-transact-sql.md) 的位置。 *command_id* 的值为 **int**，默认值为 NULL。 如果已指定，则还必须指定所有其他参数，并且 *xact_seqno_start* 必须与 *xact_seqno_end* 相同。  
  
`[ @agent_id = ] agent_id` 指定仅返回特定复制代理的命令。 *agent_id* 的值为 **int**，默认值为 NULL。  
  
`[ @compatibility_level = ] compatibility_level`Compatibility_level 的版本 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，其默认值为 9000000 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**varbinary(16)**|命令的序列号。|  
|**originator_srvname**|**sysname**|发起事务的服务器。|  
|**originator_db**|**sysname**|发起事务的数据库。|  
|**article_id**|**int**|项目的 ID。|  
|type |**int**|命令的类型。|  
|**partial_command**|**bit**|指示是否为部分命令。|  
|**hashkey**|**int**|仅限内部使用。|  
|**originator_publication_id**|**int**|发起事务的发布的 ID。|  
|**originator_db_version**|**int**|发起事务的数据库的版本。|  
|**originator_lsn**|**varbinary(16)**|标识初始发布中命令的日志序列号 (LSN)。 在对等事务复制中使用。|  
|**command**|**nvarchar(1024)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] command.|  
|**command_id**|**int**|[MSrepl_commands](../../relational-databases/system-tables/msrepl-commands-transact-sql.md)中的命令 ID。|  
  
 在结果集中，长命令可以拆分为若干行。  
  
## <a name="remarks"></a>备注  
 **sp_browsereplcmds** 用于事务复制。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或分发数据库上的 **db_owner** 或 **replmonitor** 固定数据库角色的成员才能执行 **sp_browsereplcmds**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_replcmds (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_replshowcmds &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replshowcmds-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
