---
description: sp_repldropcolumn (Transact-SQL)
title: sp_repldropcolumn (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repldropcolumn_TSQL
- sp_repldropcolumn
helpviewer_keywords:
- sp_repldropcolumn
ms.assetid: fdc1ec5f-f108-42b4-a2d8-f06a71913ab8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6c5fdabf724c284e8a75244269321b92c81b46ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206432"
---
# <a name="sp_repldropcolumn-transact-sql"></a>sp_repldropcolumn (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  从已发布的现有表项目中删除列。 此存储过程在发布服务器上对发布数据库执行。  
  
> [!IMPORTANT]
>  已不推荐使用此存储过程，支持它主要是为了能够向后兼容。 它只能与 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 发布服务器和重新 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 发布订阅服务器一起使用。 不应对列将此过程与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本中引入的数据类型一起使用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_repldropcolumn [ @source_object = ] 'source_object', [ @column = ] 'column'   
    [ , [ @from_agent = ] from_agent]   
    [ , [ @schema_change_script = ] 'schema_change_script' ]   
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>参数  
 [ @source_object =] "*source_object*"  
 包含要删除的列的表项目的名称。 *source_object* 为 nvarchar (258) ，无默认值。  
  
 [ @column =] "*column*"  
 表中要删除的列的名称。 *列* 的值为 sysname，无默认值。  
  
 [ @from_agent =] *from_agent*  
 表示是否由复制代理执行该存储过程。 *from_agent* 为 int，默认值为0，其中默认值为0，在复制代理执行此存储过程时，将使用值1，而在每个其他情况下，应使用默认值0。  
  
 [ @schema_change_script =] "*schema_change_script*"  
 指定用于修改系统生成的自定义存储过程的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 脚本的名称和路径。 *schema_change_script* 为 nvarchar (4000) ，默认值为 NULL。 复制允许用户定义的自定义存储过程替换事务复制中使用的一个或多个默认过程。 使用 sp_repldropcolumn 对复制的表项目进行架构更改后，将执行 *schema_change_script* ，并可用于执行以下操作之一：  
  
-   如果自定义存储过程是自动重新生成的，则 *schema_change_script* 可用于删除这些自定义存储过程，并使用支持新架构的用户定义的自定义存储过程替换这些存储过程。  
  
-   如果自定义存储过程不是自动重新生成的，则 *schema_change_script* 可用于重新生成这些存储过程或创建用户定义的自定义存储过程。  
  
 [ @force_invalidate_snapshot =] *force_invalidate_snapshot*  
 启用或禁用使快照失效的功能。 *force_invalidate_snapshot* 为一个位，默认值为1。  
  
 1 指定对项目的更改可能导致快照无效，如果发生这种情况，值 1 提供了新建快照所需的权限。  
  
 0 指定对项目所做的更改不会导致快照失效。  
  
 [ @force_reinit_subscription =] *force_reinit_subscription*  
 启用或禁用使订阅重新初始化的功能。 *force_reinit_subscription* 是一位，默认值为0。  
  
 0 指定对项目所做的更改不会导致重新初始化订阅。  
  
 1 指定对项目的更改可能导致订阅重新初始化，如果发生这种情况，值 1 提供了重新初始化订阅所需的权限。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="permissions"></a>权限  
 只有发布服务器上的 sysadmin 固定服务器角色的成员、发布数据库中的 db_owner 和 db_ddladmin 固定数据库角色的成员可以执行 sp_repldropcolumn。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 复制中不推荐使用的功能](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
