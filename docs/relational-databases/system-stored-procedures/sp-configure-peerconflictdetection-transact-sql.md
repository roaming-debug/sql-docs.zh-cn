---
description: sp_configure_peerconflictdetection (Transact-SQL)
title: sp_configure_peerconflictdetection (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_configure_peerconflictdetection_TSQL
- sp_configure_peerconflictdetection
helpviewer_keywords:
- sp_configure_peerconflictdetection
ms.assetid: 45117cb2-3247-433f-ba3d-7fa19514b1c3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 58e583191ba0c18824715664162f89a7911c466a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198707"
---
# <a name="sp_configure_peerconflictdetection-transact-sql"></a>sp_configure_peerconflictdetection (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为对等事务复制拓扑中包含的发布配置冲突检测。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_configure_peerconflictdetection [ @publication = ] 'publication'  
    [ , [ @action = ] 'action']  
    [ , [ @originator_id = ] originator_id ]  
    [ , [ @conflict_retention = ] conflict_retention ]  
    [ , [ @continue_onconflict = ] 'continue_onconflict']  
    [ , [ @local = ] 'local']  
    [ , [ @timeout = ] timeout ]  
  
```  
  
## <a name="arguments"></a>参数  
 [ @publication =] "*发布*"  
 要配置冲突检测的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
 [ @action =] "*action*"  
 指定是否为发布启用或禁用冲突检测。 *操作* 为 **nvarchar (5)**，可以是以下值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**enable**|为发布启用冲突检测。|  
|**disable**|为发布禁用冲突检测。|  
|NULL（默认值）||  
  
 [ @originator_id =] *originator_id*  
 指定对等拓扑中某个节点的 ID。 *originator_id* 的值为 **int**，默认值为 NULL。 如果 " *操作* " 设置为 " **启用**"，则此 ID 用于冲突检测。 请指定拓扑中从未使用过的非零、正值 ID。 有关已经使用过的 ID 的列表，请查询 [Mspeer_originatorid_history](../../relational-databases/system-tables/mspeer-originatorid-history-transact-sql.md) 系统表。  
  
 [ @conflict_retention =] *conflict_retention*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @continue_onconflict =] "*continue_onconflict*"]  
 确定检测到冲突后分发代理是否继续处理更改。 *continue_onconflict* 为 **nvarchar (5)** ，默认值为 FALSE。  
  
> [!CAUTION]  
>  建议您使用默认值 FALSE。 如果此选项设置为 TRUE，则分发代理会尝试应用来自具有最高发起方 ID 的节点的冲突行来收敛拓扑中的数据。 此方法不保证将会收敛。 您应确保检测到冲突之后拓扑保持一致。 有关详细信息，请参阅 [Conflict Detection in Peer-to-Peer Replication](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)中的“处理冲突”。  
  
 [ @local =] "*local*"  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
 [ @timeout =] *超时*  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_configure_peerconflictdetection 用于对等事务复制。 若要使用冲突检测，所有节点都必须运行 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或更高版本; 并且必须为所有节点启用检测。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [对等复制中的冲突检测](../../relational-databases/replication/transactional/peer-to-peer-conflict-detection-in-peer-to-peer-replication.md)   
 [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
