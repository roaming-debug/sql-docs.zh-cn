---
description: sysmail_add_principalprofile_sp (Transact-SQL)
title: sysmail_add_principalprofile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_principalprofile_sp_TSQL
- sysmail_add_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_principalprofile_sp
ms.assetid: b2a0b313-abb9-4c23-8511-db77ca8172b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 0252e299f072144999e3c2db1b2404de9aa9b6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183032"
---
# <a name="sysmail_add_principalprofile_sp-transact-sql"></a>sysmail_add_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予 msdb 数据库主体使用数据库邮件配置文件的权限。 数据库主体必须映射到 SQL Server authentication 用户、Windows 用户或 Windows 组。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_add_principalprofile_sp  { [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
    [ , [ @is_default ] = 'is_default' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @principal_id = ] principal_id` 关联的 **msdb** 数据库中数据库用户或角色的 ID。 *principal_id* 的值为 **int**，默认值为 NULL。 必须指定 *principal_id* 或 *principal_name* 。 *Principal_id* **0** 使此配置文件成为公共配置文件，并授予对数据库中所有主体的访问权限。  
  
`[ @principal_name = ] 'principal_name'` 关联的 **msdb** 数据库中数据库用户或角色的名称。 *principal_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *principal_id* 或 *principal_name* 。 *Principal_name* **"public"** 使此配置文件成为公共配置文件，并授予对数据库中所有主体的访问权限。  
  
`[ @profile_id = ] profile_id` 关联的配置文件的 id。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 关联的配置文件的名称。 *profile_name* **sysname**，无默认值。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @is_default = ] is_default` 指定此配置文件是否为主体的默认配置文件。 主体必须且只能有一个默认配置文件。 *is_default* 是 **bit**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 若要将配置文件设置为公共配置文件，请指定 **\@ principal_id** **0** 或 **public** **\@ principal_name** 。 公共配置文件可供 **msdb** 数据库中的所有用户使用，但用户还必须是 **DatabaseMailUserRole** 的成员才能执行 **sp_send_dbmail**。  
  
 数据库用户只能有一个默认的配置文件。 当 **\@ is_default** 为 "**1**" 并且用户已与一个或多个配置文件关联时，指定的配置文件将成为该用户的默认配置文件。 以前的默认配置文件仍与该用户关联，但不再是默认配置文件。  
  
 如果 **\@ is_default** 为 "**0**"，并且不存在其他关联，则存储过程将返回错误。  
  
 存储过程 **sysmail_add_principalprofile_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 **A. 创建一个关联，设置默认配置文件**  
  
 以下示例在名为的配置文件 `AdventureWorks Administrator Profile` 与 **msdb** 数据库用户之间创建关联 `ApplicationUser` 。 此配置文件是该用户的默认配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator Profile',  
    @is_default = 1 ;  
```  
  
 **B. 将配置文件设置为默认的公共配置文件**  
  
 下面的示例将配置文件 `AdventureWorks Public Profile` 设置为 **msdb** 数据库中用户的默认公共配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_add_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'AdventureWorks Public Profile',  
    @is_default = 1 ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
