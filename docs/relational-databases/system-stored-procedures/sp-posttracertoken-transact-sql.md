---
description: sp_posttracertoken (Transact-SQL)
title: sp_posttracertoken (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- posttracerttoken
- posttracerttoken_TSQL
- sp_posttracertoken
- sp_posttracertoken_TSQL
helpviewer_keywords:
- sp_posttracertoken
ms.assetid: 24da5cd2-1c45-475e-93db-5bdf660f1c2c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13331e0cb1c01aa2b8ec5203793b40d3a5c65ea4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196021"
---
# <a name="sp_posttracertoken-transact-sql"></a>sp_posttracertoken (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此过程将跟踪令牌发布到发布服务器上的事务日志，并开始跟踪滞后时间统计信息的过程。 如果将跟踪令牌写入了事务日志，则当日志读取器代理选中该令牌，以及分发代理应用该令牌时，均会记录信息。 此存储过程在发布服务器上对发布数据库执行。 有关详细信息，请参阅 [为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_posttracertoken [ @publication = ] 'publication'   
    [ , [ @tracer_token_id = ] tracer_token_id OUTPUT  
    [ , [ @publisher = ] 'publisher'   
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要测量其滞后时间的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @tracer_token_id = ] _tracer_token_id OUTPUT` 插入的跟踪令牌的 ID。 *tracer_token_id* 的默认值为 **int** ，默认值为 NULL，它是一个 OUTPUT 参数。 此值可用于执行 [sp_helptracertokenhistory &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokenhistory-transact-sql.md) 或 [sp_deletetracertokenhistory &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-deletetracertokenhistory-transact-sql.md) ，而无需首先执行 [sp_helptracertokens &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helptracertokens-transact-sql.md)。  
  
`[ @publisher = ] 'publisher'` 指定一个非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器。 *发布服务器* 的 **sysname** 为，默认值为 NULL，不应为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器指定。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_posttracertoken** 用于事务复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#sp_tracertokens](../../relational-databases/replication/codesnippet/tsql/sp-posttracertoken-trans_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_posttracertoken**。  
  
## <a name="see-also"></a>另请参阅  
 [为事务复制测量滞后时间和验证连接](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)  
  
  
