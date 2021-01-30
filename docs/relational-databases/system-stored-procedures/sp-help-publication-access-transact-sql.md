---
description: sp_help_publication_access (Transact-SQL)
title: sp_help_publication_access (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_help_publication_access
- sp_help_publication_access_TSQL
helpviewer_keywords:
- sp_help_publication_access
ms.assetid: 9408fa13-54a0-4cb1-8fb0-845e5536ef50
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b33b79c6e632eef28feaa5fa8b3f605018b09953
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199593"
---
# <a name="sp_help_publication_access-transact-sql"></a>sp_help_publication_access (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  返回发布的所有授权登录的列表。 此存储过程在发布服务器上对发布数据库执行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_publication_access [ @publication = ] 'publication'  
    [ , [ @return_granted = ] 'return_granted' ]   
    [ , [ @login = ] 'login' ]  
    [ , [ @initial_list = ] initial_list ]  
```  
  
## <a name="arguments"></a>参数  
`[ @publication = ] 'publication'` 要访问的发布的名称。 *发布* 为 **sysname**，无默认值。  
  
`[ @return_granted = ] 'return_granted'` 登录 ID。 *return_granted* 为 **bit**，默认值为1。 如果指定 **0** 并 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用身份验证，则返回出现在发布服务器而不是分发服务器上的可用登录名。 如果指定 **0** 并使用 Windows 身份验证，则返回在发布服务器或分发服务器上不被明确拒绝访问的登录名。  
  
`[ @login = ] 'login'` 标准安全登录 ID。 *login* 的值为 **sysname**，默认值为 **%** 。  
  
`[ @initial_list = ] initial_list` 指定是返回具有发布访问权的所有成员，还是只返回那些在新成员添加到列表之前具有访问权限的成员。 *initial_list* 为 bit，默认值为 **0**。  
  
 **1** 返回 **sysadmin** 固定服务器角色的所有成员的信息，这些成员具有在创建发布时存在的分发服务器上的有效登录名，以及当前登录名。  
  
 **0** 返回 **sysadmin** 固定服务器角色的所有成员的信息，这些成员具有在创建发布时存在的分发服务器上的有效登录名，以及不属于 **sysadmin** 固定服务器角色的发布访问列表中的所有用户。  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**Loginname**|**nvarchar(256)**|实际登录名。|  
|**Isntname**|**int**|**0** = 登录名不是 Windows 用户。<br /><br /> **1** = 登录名是 Windows 用户。|  
|**Isntgroup**|**int**|**0** = 登录不是 Windows 组。<br /><br /> **1** = 登录名是 Windows 组。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_help_publication_access** 在所有类型的复制中使用。  
  
 如果结果集中的 **Isntname** 和 **Isntgroup** 均为 **0**，则假定该登录名是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员或 **db_owner** 固定数据库角色的成员才能执行 **sp_help_publication_access**。  
  
## <a name="see-also"></a>另请参阅  
 [sp_grant_publication_access &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-grant-publication-access-transact-sql.md)   
 [sp_revoke_publication_access &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-revoke-publication-access-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
