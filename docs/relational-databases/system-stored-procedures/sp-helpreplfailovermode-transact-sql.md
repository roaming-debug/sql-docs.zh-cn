---
description: sp_helpreplfailovermode (Transact-SQL)
title: sp_helpreplfailovermode (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpreplfailovermode
- sp_helpreplfailovermode_TSQL
helpviewer_keywords:
- sp_helpreplfailovermode
ms.assetid: d1090e42-6840-4bf6-9aa9-327fd8987ec2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 11ebe0df55bb0b6b5a2b7f009a0be4be28c788c1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210835"
---
# <a name="sp_helpreplfailovermode-transact-sql"></a>sp_helpreplfailovermode (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  显示订阅的当前故障转移模式。 此存储过程在订阅服务器上对任何数据库执行。 有关故障转移模式的详细信息，请参阅 [事务复制的可更新订阅](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helpreplfailovermode [ @publisher= ] 'publisher'   
    [ , [ @publisher_db = ] 'publisher_db' ]   
    [ , [ @publication = ] 'publication' ]   
    [ , [ @failover_mode_id= ] 'failover_mode_id'OUTPUT]   
    [ , [ @failover_mode = ] 'failover_mode'OUTPUT]   
```  
  
## <a name="arguments"></a>参数  
`[ @publisher = ] 'publisher'` 参与此订阅服务器的更新的发布服务器的名称。 *发布服务器* 的 **sysname**，无默认值。 必须已为发布配置了发布服务器。  
  
`[ @publisher_db = ] 'publisher_db'` 发布数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @publication = ] 'publication'` 参与此订阅服务器更新的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @failover_mode_id = ] 'failover_mode_id' OUTPUT` 返回故障转移模式的整数值，它是一个 **OUTPUT** 参数。 *failover_mode_id* 为 **tinyint** ，默认值为 **0**。 对于立即更新，它将返回 **0** ; 对于排队更新，则返回 **1** 。  
  
`[ @failover_mode = ] 'failover_mode' OUTPUT` 返回在订阅服务器上进行数据修改时所采用的模式。 *failover_mode* 为 **nvarchar (10)** ，默认值为 NULL。 为 **输出** 参数。  
  
|值|说明|  
|-----------|-----------------|  
|**版**|立即更新：使用两阶段提交协议 (2PC)，将订阅服务器中的更新立即传播到发布服务器。|  
|**好**|排队更新：将订阅服务器中的更新存储在队列中。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_helpreplfailovermode** 用于快照复制或事务复制，在出现故障的情况下，将使用排队更新为其启用立即更新，并将排队更新作为故障转移。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_helpreplfailovermode**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_setreplfailovermode &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-setreplfailovermode-transact-sql.md)  
  
  
