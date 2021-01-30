---
description: sp_get_redirected_publisher (Transact-SQL)
title: sp_get_redirected_publisher (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_get_redirected_publisher_TSQL
- sp_get_redirected_publisher
ms.assetid: d47a9ab5-f2cc-42a8-8be9-a33895ce44f0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: fa1a1b8f7cffe3f98435ebd48a404fb76df34c8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204756"
---
# <a name="sp_get_redirected_publisher-transact-sql"></a>sp_get_redirected_publisher (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  由复制代理用于查询分发服务器，以确定是否已重定向原始发布服务器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_get_redirected_publisher   
    [ @original_publisher = ] 'original_publisher',  
    [ @publisher_db = ] 'database_name',   
    [ @bypass_publisher_validation = ] [0 | 1 ]  
```  
  
## <a name="arguments"></a>参数  
`[ @original_publisher = ] 'original_publisher'` 最初发布数据库的 SQL Server 实例的名称。 *original_publisher* **sysname**，无默认值。
  
`[ @publisher_db = ] 'publisher_db'` 要发布的数据库的名称。 *publisher_db* **sysname**，无默认值。  
  
`[ @bypass_publisher_validation = ] [0 | 1 ]` 用于绕过重定向的发布服务器的验证。 如果为0，则执行验证。 如果为 1，则不执行验证。 *bypass_publisher_validation* 为 **bit**，默认值为0。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**redirected_publisher**|**sysname**|重定向后的发布服务器的名称。|  
|error_number|**int**|验证错误的错误号。|  
|**error_severity**|**int**|验证错误的严重性。|  
|**error_message**|**nvarchar(4000)**|验证错误消息的文本。|  
  
## <a name="remarks"></a>备注  
 *redirected_publisher* 返回当前发布服务器的名称。 如果尚未使用 **sp_redirect_publisher** 重定向发布服务器和发布数据库，则返回 null。  
  
 如果未请求验证，或发布服务器和发布数据库没有条目，则 *error_number* 和 *error_severity* 返回0， *error_message* 返回 null。  
  
 如果请求验证，则会调用验证存储过程 [sp_validate_redirected_publisher &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md) ，以验证重定向的目标是否为发布数据库的合适主机。 如果验证成功，则 **sp_get_redirected_publisher** 返回 *error_number* 和 *error_severity* 列的重定向发布服务器名称，0返回 *error_message* 列中的 null。  
  
 如果验证请求失败，则会返回重定向的发布服务器的名称以及错误信息。  
  
## <a name="permissions"></a>权限  
 调用方必须是 **sysadmin** 固定服务器角色的成员、分发数据库 **db_owner** 固定数据库角色的成员，或者是与发布服务器数据库相关联的已定义发布的发布访问列表的成员。  
  
## <a name="see-also"></a>另请参阅  
 [复制存储过程 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [sp_validate_redirected_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-redirected-publisher-transact-sql.md)   
 [sp_redirect_publisher &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-redirect-publisher-transact-sql.md)   
 [sp_validate_replica_hosts_as_publishers &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-validate-replica-hosts-as-publishers-transact-sql.md)  
  
  
