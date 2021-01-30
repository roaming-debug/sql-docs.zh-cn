---
description: sp_redirect_publisher (Transact-SQL)
title: sp_redirect_publisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_redirect_publisher_TSQL
- sp_redirect_publisher
helpviewer_keywords:
- sp_redirect_publisher
ms.assetid: af45e2b2-57fb-4bcd-a58b-e61401fb3b26
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d7fef5e51b8a41e6e21d570a398775810dd23155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185747"
---
# <a name="sp_redirect_publisher-transact-sql"></a>sp_redirect_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为现有发布服务器/数据库对指定重定向的发布服务器。 如果发布服务器数据库属于某个 Always On 可用性组，则重定向的发布服务器就是与该可用性组关联的可用性组侦听器名称。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_redirect_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name'   
    [ , [ @redirected_publisher = ] 'new_publisher' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @original_publisher = ] 'original_publisher'` 最初发布数据库的实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *original_publisher* **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 要发布的数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @redirected_publisher = ] 'redirected_publisher'` 与将成为新发布服务器的可用性组关联的可用性组侦听器名称。 *redirected_publisher* **sysname**，无默认值。 将可用性组侦听器配置到非默认端口时，请随侦听器名称一并指定端口号，如 `'Listenername,51433'`  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 **sp_redirect_publisher** 用于通过将发布服务器/数据库对与可用性组的侦听器相关联，使复制发布服务器重定向到 Always On 可用性组的当前主副本。 为包含已发布数据库的可用性组配置 AG 侦听器后，执行 **sp_redirect_publisher** 。  
  
 如果在主副本上从可用性组中删除原始发布服务器上的发布数据库，请在不指定 *\@ redirected_publisher* 参数的值的情况下执行 **sp_redirect_publisher** ，以删除发布服务器/数据库对的重定向。 有关在时重定向发布服务器的详细信息，请参阅 [维护 AlwaysOn 发布数据库 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)。  
  
## <a name="permissions"></a>权限  
 调用方必须是 **sysadmin** 固定服务器角色的成员、分发数据库 **db_owner** 固定数据库角色的成员，或者是与发布服务器数据库相关联的已定义发布的发布访问列表的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
