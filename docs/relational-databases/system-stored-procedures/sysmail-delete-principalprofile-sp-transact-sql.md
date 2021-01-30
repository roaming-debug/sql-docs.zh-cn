---
description: sysmail_delete_principalprofile_sp (Transact-SQL)
title: sysmail_delete_principalprofile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_principalprofile_sp_TSQL
- sysmail_delete_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_principalprofile_sp
ms.assetid: 8fc14700-e17a-4073-9a96-7fc23e775c69
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a80485aca8c7f0adb6a539f57540952ccd758a31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182020"
---
# <a name="sysmail_delete_principalprofile_sp-transact-sql"></a>sysmail_delete_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除数据库用户或角色使用公共或专用数据库邮件配置文件的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_delete_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
```  
  
## <a name="arguments"></a>参数  
`[ @principal_id = ] principal_id` 要删除的关联的 **msdb** 数据库中数据库用户或角色的 ID。 *principal_id* 的值为 **int**，默认值为 NULL。 若要将公共配置文件设置为专用配置文件，请提供主体 ID **0** 或主体名称 **"public"**。 必须指定 *principal_id* 或 *principal_name* 。  
  
`[ @principal_name = ] 'principal_name'` 要删除的关联的 **msdb** 数据库中数据库用户或角色的名称。 *principal_name* 的默认值为 **sysname**，默认值为 NULL。 若要将公共配置文件设置为专用配置文件，请提供主体 ID **0** 或主体名称 **"public"**。 必须指定 *principal_id* 或 *principal_name* 。  
  
`[ @profile_id = ] profile_id` 要删除的关联的配置文件的 ID。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要删除的关联的配置文件的名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 若要将公共配置文件设置为专用配置文件，请为主体名称提供 **"public"** ，为主体 id 提供 **0** 。  
  
 删除用户的默认专用配置文件的权限或默认公共配置文件的权限时，请谨慎操作。 如果没有可用的默认配置文件， **sp_send_dbmail** 需要配置文件的名称作为参数。 因此，删除默认配置文件可能会导致对 **sp_send_dbmail** 的调用失败。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_send_dbmail ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md)。  
  
 存储过程 **sysmail_delete_principalprofile_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何删除 **msdb** 数据库中的配置文件 **AdventureWorks 管理员** 和登录名 **ApplicationUser** 之间的关联。  
  
```  
EXECUTE msdb.dbo.sysmail_delete_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
