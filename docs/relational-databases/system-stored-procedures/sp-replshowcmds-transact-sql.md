---
description: sp_replshowcmds (Transact-SQL)
title: sp_replshowcmds (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replshowcmds
- sp_replshowcmds_TSQL
helpviewer_keywords:
- sp_replshowcmds
ms.assetid: 199f5a74-e08e-4d02-a33c-b8ab0db20f44
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb46a1c57dc44f9fb90bbee84906c7bcd8b86d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157503"
---
# <a name="sp_replshowcmds-transact-sql"></a>sp_replshowcmds (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  以可读格式返回标记为复制的事务的命令。 仅当客户端连接 (包括当前连接时，才可以运行 **sp_replshowcmds**) 不从日志中读取复制的事务。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replshowcmds [ @maxtrans = ] maxtrans  
```  
  
## <a name="arguments"></a>参数  
`[ @maxtrans = ] maxtrans` 要返回其信息的事务数。 *maxtrans* 的值为 **int**，默认值为 **1**，它指定 **sp_replshowcmds** 为其返回信息的最大事务挂起复制数。  
  
## <a name="result-sets"></a>结果集  
 **sp_replshowcmds** 是一种诊断过程，该过程返回有关从中执行它的发布数据库的信息。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**xact_seqno**|**binary(10)**|命令的序列号。|  
|**originator_id**|**int**|命令发起方的 ID，始终为 **0**。|  
|**publisher_database_id**|**int**|发布服务器数据库的 ID，始终为 **0**。|  
|**article_id**|**int**|项目的 ID。|  
|type |**int**|命令的类型。|  
|**command**|**nvarchar(1024)**|[!INCLUDE[tsql](../../includes/tsql-md.md)] command.|  
  
## <a name="remarks"></a>备注  
 **sp_replshowcmds** 用于事务复制。  
  
 使用 **sp_replshowcmds**，可以查看当前未分发 (事务日志中尚未发送到分发服务器) 的事务的事务。  
  
 在同一数据库中运行 **sp_replshowcmds** 和 **sp_replcmds** 的客户端将收到错误18752。  
  
 若要避免此错误，必须通过执行 **sp_replflush**，第一个客户端必须断开连接或客户端作为日志读取器的角色。 在所有客户端都与日志读取器断开连接后， **sp_replshowcmds** 可以成功运行。  
  
> [!NOTE]  
>  应该仅运行 **sp_replshowcmds** 以解决复制问题。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_replshowcmds**。  
  
## <a name="see-also"></a>另请参阅  
 [错误消息](../../relational-databases/native-client-odbc-error-messages/error-messages.md)   
 [sp_replcmds (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md)   
 [sp_repldone &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [sp_repltrans &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-repltrans-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
