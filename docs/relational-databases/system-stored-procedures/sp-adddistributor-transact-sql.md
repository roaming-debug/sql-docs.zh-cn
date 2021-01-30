---
description: sp_adddistributor (Transact-SQL)
title: sp_adddistributor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/09/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_adddistributor
- sp_adddistributor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_adddistributor
ms.assetid: 35415502-68d0-40f6-993c-180e50004f1e
author: mashamsft
ms.author: mathoma
ms.openlocfilehash: 2e84824fec7a45fcfea24a2d97ced622b2147f34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182807"
---
# <a name="sp_adddistributor-transact-sql"></a>sp_adddistributor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  在 " [sys.sys服务器](../../relational-databases/system-compatibility-views/sys-sysservers-transact-sql.md) " 表中创建一个条目 (如果不存在一个) ，则将服务器条目标记为分发服务器，并存储属性信息。 此存储过程在分发服务器上对主数据库执行以注册服务器，并将其标记为分发服务器。 如果是远程分发服务器，此存储过程还在发布服务器上对主数据库执行以注册远程分发服务器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_adddistributor [ @distributor= ] 'distributor'   
    [ , [ @heartbeat_interval= ] heartbeat_interval ]   
    [ , [ @password= ] 'password' ]   
    [ , [ @from_scripting= ] from_scripting ]  
```  
  
## <a name="arguments"></a>参数  
`[ @distributor = ] 'distributor'` 分发服务器名称。 *分发服务器* 的 **sysname**，无默认值。 只有在设置远程分发服务器时才使用此参数。 它在 msdb 中添加分发服务器属性的条目。 **MSdistributor** 表。  

> [!NOTE]
> 服务器名称可指定为 `<Hostname>,<PortNumber>` 。 如果在 Linux 或 Windows 上使用自定义端口部署 SQL Server，并且禁用了 browser 服务，则可能需要指定连接的端口号。

`[ @heartbeat_interval = ] heartbeat_interval` 代理在不记录进度消息的情况下可以执行的最大分钟数。 *heartbeat_interval* 的值为 **int**，默认值为10分钟。 创建按此间隔运行的 SQL Server 代理作业，以检查正在运行的复制代理的状态。  
  
`[ @password = ] 'password']` 是 **distributor_admin** 登录名的密码。 *password* 的值为 **sysname**，默认值为 NULL。 如果为 NULL 或空字符串，则密码重置为随机值。 必须在添加第一个远程分发服务器时配置密码。 为 *分发* 服务器 RPC 连接（包括本地连接）的链接服务器条目存储 **distributor_admin** 登录名和 *密码*。 如果 *分发服务器* 是本地的，则 **distributor_admin** 的密码设置为新值。 对于具有远程分发服务器的发布服务器，在发布服务器和分发服务器上执行 **sp_adddistributor** 时必须指定相同的 *密码* 值。 [sp_changedistributor_password](../../relational-databases/system-stored-procedures/sp-changedistributor-password-transact-sql.md) 可用于更改分发服务器密码。  
  
> [!IMPORTANT]  
>  如果可能，请在运行时提示用户输入安全凭据。 如果必须在脚本文件中存储凭据，则必须保护文件以防止未经授权的访问。  
  
`[ @from_scripting = ] from_scripting` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_adddistributor** 用于快照复制、事务复制和合并复制。  
  
## <a name="example"></a>示例  
 [!code-sql[HowTo#AddDistPub](../../relational-databases/replication/codesnippet/tsql/sp-adddistributor-transa_1.sql)]  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_adddistributor** 执行。  
  
## <a name="see-also"></a>另请参阅  
 [配置发布和分发](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [sp_changedistributor_property &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_dropdistributor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropdistributor-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [配置分发](../../relational-databases/replication/configure-distribution.md)  
  
  
