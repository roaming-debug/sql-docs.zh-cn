---
description: sysmail_update_principalprofile_sp (Transact-SQL)
title: sysmail_update_principalprofile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_principalprofile_sp
- sysmail_update_principalprofile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_principalprofile_sp
ms.assetid: 9fe96e9a-4758-4e4a-baee-3e1217c4426c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 85bab8892a9459ee5de954596ffea2be2a5aed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181868"
---
# <a name="sysmail_update_principalprofile_sp-transact-sql"></a>sysmail_update_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新主体和配置文件之间的关联的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_update_principalprofile_sp { @principal_id = principal_id | @principal_name = 'principal_name' } ,  
    { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    [ @is_default = ] 'is_default'  
```  
  
## <a name="arguments"></a>参数  
`[ @principal_id = ] principal_id` 要更改的关联的 **msdb** 数据库中数据库用户或角色的 ID。 *principal_id* 的值为 **int**，默认值为 NULL。 必须指定 *principal_id* 或 *principal_name* 。  
  
`[ @principal_name = ] 'principal_name'` 要更新的关联的 **msdb** 数据库中数据库用户或角色的名称。 *principal_name* 的默认值为 **sysname**，默认值为 NULL。 可以指定 *principal_id* 或 *principal_name* 。  
  
`[ @profile_id = ] profile_id` 要更改的关联的配置文件的 id。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要更改的关联的配置文件的名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @is_default = ] 'is_default'` 此配置文件是否为数据库用户的默认配置文件。 数据库用户只能有一个默认的配置文件。 *is_default* 是 **bit**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 此存储过程会更改指定的配置文件是否为数据库用户的默认配置文件。 一个数据库用户只能有一个默认专用配置文件。  
  
 如果关联的主体名称是 **公共** 的或关联的主体 id 为 **0**，则此存储过程将更改公共配置文件。 只能有一个默认的公共配置文件。  
  
 当 **\@ is_default** 为 "**1**" 并且主体与多个配置文件关联时，指定的配置文件将成为主体的默认配置文件。 以前的默认配置文件仍与主体数据库关联，但不再是默认配置文件。  
  
 存储过程 **sysmail_update_principalprofile_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 **A. 将一个配置文件设置为数据库的默认公共配置文件**  
  
 下面的示例将配置文件设置 `General Use Profile` 为 **msdb** 数据库中的用户的默认公共配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'public',  
    @profile_name = 'General Use Profile',  
    @is_default = '1';  
```  
  
 **B. 将一个配置文件设置为用户的默认专用配置文件**  
  
 下面的示例将配置文件设置 `AdventureWorks Administrator` 为 `ApplicationUser` **msdb** 数据库中主体的默认配置文件。 该配置文件必须已与主体关联。 以前的默认配置文件仍与主体数据库关联，但不再是默认配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_update_principalprofile_sp  
    @principal_name = 'ApplicationUser',  
    @profile_name = 'AdventureWorks Administrator',  
    @is_default = '1' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
