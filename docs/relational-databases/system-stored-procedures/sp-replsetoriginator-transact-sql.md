---
description: sp_replsetoriginator (Transact-SQL)
title: sp_replsetoriginator (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replsetoriginator
- sp_replsetoriginator_TSQL
helpviewer_keywords:
- sp_replsetoriginator
ms.assetid: 030e5226-0585-439f-b8cd-36f48367d86d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c26248551ad949bcf27433d28898ba4ec98dc68b
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2021
ms.locfileid: "99211636"
---
# <a name="sp_replsetoriginator-transact-sql"></a>sp_replsetoriginator (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  用于在双向事务复制中调用环回检测和处理。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_replsetoriginator [ @server_name= ] 'server_name'   
    , [ @database_name= ] 'database_name'  
```  
  
## <a name="arguments"></a>参数  
`[ @server_name = ] 'server_name'` 应用事务的服务器的名称。 *originating_server* **sysname**，无默认值。  
  
`[ @database_name = ] 'database_name'` 应用事务的数据库的名称。 *originating_db* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_replsetoriginator** 由分发代理执行，以记录由复制应用的事务的源。 该信息用于为具有环回属性集的双向事务订阅调用环回检测。  
  
## <a name="permissions"></a>权限  
 只有发布服务器上 **sysadmin** 固定服务器角色的成员、发布数据库上 **db_owner** 固定数据库角色的成员或发布访问列表中的用户 (PAL) 可以执行 **sp_replsetoriginator**。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
