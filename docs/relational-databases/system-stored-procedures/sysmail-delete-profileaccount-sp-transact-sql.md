---
description: sysmail_delete_profileaccount_sp (Transact-SQL)
title: sysmail_delete_profileaccount_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_delete_profileaccount_sp
- sysmail_delete_profileaccount_sp_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_delete_profileaccount_sp
ms.assetid: b58d06f2-d6c9-4c8e-95bd-027c50f4621a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: d364dfb4de4408da2f424185e6cad4f62b0c54cf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181984"
---
# <a name="sysmail_delete_profileaccount_sp-transact-sql"></a>sysmail_delete_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  从数据库邮件配置文件中删除帐户。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_delete_profileaccount_sp  {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ,  
    {   [ @account_id = ] account_id | [ @account_name = ] 'account_name' }  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_id = ] profile_id` 要删除的配置文件的配置文件 ID。 *profile_id* 的值为 **int**，默认值为 NULL。 可以指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要删除的配置文件的配置文件名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 可以指定 *profile_id* 或 *profile_name* 。  
  
`[ @account_id = ] account_id` 要删除的帐户 ID。 *account_id* 的值为 **int**，默认值为 NULL。 可以指定 *account_id* 或 *account_name* 。  
  
`[ @account_name = ] 'account_name'` 要删除的帐户的名称。 *account_name* 的默认值为 **sysname**，默认值为 NULL。 可以指定 *account_id* 或 *account_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果指定的帐户与指定的配置文件不相关，则会返回错误。  
  
 如果指定了帐户但没有指定配置文件，此存储过程会从所有配置文件中删除指定的帐户。 例如，如果您准备关闭现有的 SMTP 服务器，您将会从所有配置文件中删除使用该 SMTP 服务器的帐户，而不是从各个配置文件中删除每一个帐户。  
  
 如果指定了配置文件但没有指定帐户，此存储过程将从指定的配置文件中删除所有帐户。 例如，如果您正在更改配置文件使用的 SMTP 服务器，则可方便地从配置文件中删除所有帐户，然后根据需要添加新帐户。  
  
 存储过程 **sysmail_delete_profileaccount_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例显示了如何从配置文件 `Audit Account` 中删除帐户 `AdventureWorks Administrator`。  
  
```  
EXECUTE msdb.dbo.sysmail_delete_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator',  
    @account_name = 'Audit Account' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
