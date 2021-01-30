---
description: sp_requestpeertopologyinfo (Transact-SQL)
title: sp_requestpeertopologyinfo (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_requestpeertopologyinfo
- sp_requestpeertopologyinfo_TSQL
helpviewer_keywords:
- sp_requestpeertopologyinfo
ms.assetid: 15cd28bd-5a72-41fb-ae1b-726baaa6fad5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5cd33688e4a3d5ba5f1b849580f9c180a1ad4d98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182053"
---
# <a name="sp_requestpeertopologyinfo-transact-sql"></a>sp_requestpeertopologyinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用有关对等事务复制拓扑的信息填充 [MSpeer_topologyresponse](../../relational-databases/system-tables/mspeer-topologyresponse-transact-sql.md) 系统表。 执行 [sp_gettopologyinfo](../../relational-databases/system-stored-procedures/sp-gettopologyinfo-transact-sql.md) 以获取 XML 格式的表中的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_requestpeertopologyinfo [ @publication = ] 'publication'  
        [ ,[ @requestid=] request_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [ @publication =] "*发布*"  
 要执行拓扑范围内的状态请求的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
 [ @request_id =] *request_id*  
 分配给拓扑状态请求的 ID 号。 *request_id* 的值为 **int**，默认值为 NULL。 [Sp_gettopologyinfo](../../relational-databases/system-stored-procedures/sp-gettopologyinfo-transact-sql.md)可以使用此 ID。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_requestpeertopologyinfo 用于对等事务复制。 执行 [sp_gettopologyinfo](../../relational-databases/system-stored-procedures/sp-gettopologyinfo-transact-sql.md)之前执行 sp_requestpeertopologyinfo。 这些过程由配置对等拓扑向导使用，但如果需要 XML 格式的拓扑信息，也可以直接使用它们。 如果喜欢表格结果，请查询 [MSpeer_topologyresponse](../../relational-databases/system-tables/mspeer-topologyresponse-transact-sql.md) 系统表。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
