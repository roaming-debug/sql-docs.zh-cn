---
description: sysmail_delete_profile_sp (Transact-SQL)
title: sysmail_delete_profile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_profile_sp
- sysmail_delete_profile_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_profile_sp
ms.assetid: 71998653-4a02-446d-b6f7-50646a29e8a2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7484e1c1f31d8cebc7b2ce60c95820e1e7ff19ef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182007"
---
# <a name="sysmail_delete_profile_sp-transact-sql"></a>sysmail_delete_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除数据库邮件使用的邮件配置文件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_delete_profile_sp  { [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' }  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_id = ] profile_id` 要删除的配置文件的配置文件 id。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要删除的配置文件的名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 删除配置文件不会删除配置文件使用的帐户。  
  
 此存储过程删除配置文件时，不考虑用户是否有权访问该配置文件。 删除用户的默认专用配置文件或 **msdb** 数据库的默认公共配置文件时，请小心。 如果没有可用的默认配置文件， **sp_send_dbmail** 需要配置文件的名称作为参数。 因此，删除默认配置文件可能会导致对 **sp_send_dbmail** 的调用失败。 有关详细信息，请参阅 [&#40;transact-sql&#41;sp_send_dbmail ](../../relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql.md)。  
  
 存储过程 **sysmail_delete_profile_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例将删除名为 `AdventureWorks Administrator` 的配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_delete_profile_sp  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
