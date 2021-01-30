---
description: sp_validate_redirected_publisher (Transact-SQL)
title: sp_validate_redirected_publisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validate_redirected_publisher
- sp_validate_redirected_publisher_TSQL
helpviewer_keywords:
- sp_validate_redirected_publisher
ms.assetid: 2b7fdbad-17e4-4442-b0b2-9b5e8f84b91d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd0e7a98d9f9a5886d829c5567d91795ca388b6a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211316"
---
# <a name="sp_validate_redirected_publisher-transact-sql"></a>sp_validate_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  验证要发布数据库的当前主机是否有能力支持复制。 必须从分发数据库运行。 此过程由 **sp_get_redirected_publisher** 调用。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
      sp_validate_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @redirected_publisher = ] 'new_publisher' output  
```  
  
## <a name="arguments"></a>参数  
`[ @original_publisher = ] 'original_publisher'` 最初发布数据库的实例的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *original_publisher* **sysname**，无默认值。  
  
`[ @publisher_db = ] 'publisher_db'` 要发布的数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @redirected_publisher = ] 'redirected_publisher'` 为发布服务器/数据库对调用 **sp_redirect_publisher** 时指定的重定向的目标。 *redirected_publisher* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无。  
  
## <a name="remarks"></a>备注  
 如果发布服务器和发布数据库中不存在任何项， **sp_validate_redirected_publisher** 将在输出参数 *\@ redirected_publisher* 中返回 null。 如果存在条目，则在成功和失败的情况下都会在输出参数中返回条目。  
  
 如果验证成功， **sp_validate_redirected_publisher** 将返回成功指示。  
  
 如果验证失败，则会引发描述失败的错误。  
  
## <a name="permissions"></a>权限  
 调用方必须是 **sysadmin** 固定服务器角色的成员、分发数据库 **db_owner** 固定数据库角色的成员，或者是与发布服务器数据库相关联的已定义发布的发布访问列表的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_get_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-get-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
