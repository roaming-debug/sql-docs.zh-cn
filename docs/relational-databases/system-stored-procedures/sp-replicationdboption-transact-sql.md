---
description: sp_replicationdboption (Transact-SQL)
title: sp_replicationdboption (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replicationdboption_TSQL
- sp_replicationdboption
helpviewer_keywords:
- sp_replicationdboption
ms.assetid: d021864e-3f21-4d1a-89df-6c1086f753bf
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e56271516d9ef2df60e993cd73963d074b503d42
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211877"
---
# <a name="sp_replicationdboption-transact-sql"></a>sp_replicationdboption (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  设置指定数据库的复制数据库选项。 此存储过程在发布服务器或订阅服务器上对任何数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replicationdboption [ @dbname= ] 'db_name'   
        , [ @optname= ] 'optname'   
        , [ @value= ] 'value'   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
    [ , [ @from_scripting = ] from_scripting ]  
```  
  
## <a name="arguments"></a>参数  
`[ @dbname = ] 'dbname'` 正在为其设置复制数据库选项的数据库。 *db_name* **sysname**，无默认值。  
  
`[ @optname = ] 'optname'` 要启用或禁用的复制数据库选项。 *optname* 为 **sysname**，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**合并发布**|数据库可用于合并发布。|  
|**发布**|数据库可用于其他类型的发布。|  
|**订**|数据库为订阅数据库。|  
|**sync with backup**|启用数据库以进行协调备份。 有关详细信息，请参阅为 [事务复制启用协调备份 &#40;复制 Transact-sql 编程&#41;](../../relational-databases/replication/administration/enable-coordinated-backups-for-transactional-replication.md)。|  
  
`[ @value = ] 'value'` 指示是启用还是禁用给定的复制数据库选项。 *值* 为 **sysname**，可以为 **true** 或 **false**。 如果此值为 **false** ，并且 *optname* 为 **merge publish**，则还将删除对合并发布数据库的订阅。  
  
`[ @ignore_distributor = ] ignore_distributor` 指示是否在未连接到分发服务器的情况下执行此存储过程。 *ignore_distributor* 是 **bit**，默认值为 **0**，表示分发服务器应连接到，并使用发布数据库的新状态进行更新。 仅当无法访问分发服务器并且 **sp_replicationdboption** 正在用于禁用发布时，才应指定值 **1** 。  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replicationdboption** 用于快照复制、事务复制和合并复制。  
  
 此过程根据给定的选项创建或删除特定的复制系统表、安全帐户，等等。 设置相应的 **is_published** (transacational 或快照复制) 、 **is_merge_published** (合并复制) 或 **master 数据库** 系统表中的 **is_distributor** 位，并创建所需的系统表。  
  
 若要禁用发布，发布数据库必须联机。 如果发布数据库存在数据库快照，则必须在禁用发布前将快照删除。 数据库快照是数据库的只读脱机副本，与复制快照无关。 有关详细信息，请参阅[数据库快照 (SQL Server)](../../relational-databases/databases/database-snapshots-sql-server.md)。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_replicationdboption** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [删除发布](../../relational-databases/replication/publish/delete-a-publication.md)   
 [禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [复制存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
