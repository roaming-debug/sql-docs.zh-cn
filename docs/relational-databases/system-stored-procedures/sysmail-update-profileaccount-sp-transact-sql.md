---
description: sysmail_update_profileaccount_sp (Transact-SQL)
title: sysmail_update_profileaccount_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_update_profileaccount_sp_TSQL
- sysmail_update_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_update_profileaccount_sp
ms.assetid: 92ca7488-29db-414e-8e36-08b0a8f542bb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 140256e73adee3593c620b17f81ac10c163030f3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181843"
---
# <a name="sysmail_update_profileaccount_sp-transact-sql"></a>sysmail_update_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新数据库邮件配置文件中帐户的序列号。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_update_profileaccount_sp  { [ @profile_id = ] profile_id   
| [ @profile_name = ] 'profile_name' } ,  
    { [ @account_id = ] account_id | [ @account_name = ] 'account_name' } ,  
    [ @sequence_number = ] sequence_number  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_id = ] profile_id` 要更新的配置文件的配置文件 ID。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要更新的配置文件的配置文件名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @account_id = ] account_id` 要更新的帐户 ID。 *account_id* 的值为 **int**，默认值为 NULL。 必须指定 *account_id* 或 *account_name* 。  
  
`[ @account_name = ] 'account_name'` 要更新的帐户的名称。 *account_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *account_id* 或 *account_name* 。  
  
`[ @sequence_number = ] sequence_number` 帐户的新序列号。 *sequence_number* 为 **int**，没有默认值。 序列号可以确定帐户在配置文件中的使用顺序。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果指定的帐户与指定的配置文件不相关，则会返回错误。  
  
 序列号可以确定数据库邮件使用配置文件中帐户的顺序。 对于新的电子邮件，数据库邮件将从序列号最小的帐户开始。 如果该帐户失败，数据库邮件就使用具有下一个序列号较大的帐户，依此类推，直到数据库邮件成功发送邮件，或者序列号最大的帐户也失败为止。 如果具有最高序列号的帐户失败，则电子邮件发送失败。  
  
 如果存在具有相同序列号的多个帐户，则数据库邮件将仅使用其中一个帐户发送给定的电子邮件。 在此情况下，数据库邮件不能保证使用具有该序列号的特定帐户，也不能保证使用同一个帐户发送各个邮件。  
  
 存储过程 **sysmail_update_profileaccount_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例在 `Admin-BackupServer` msdb 数据库的配置文件中更改帐户的序列号 `AdventureWorks Administrator` 。  执行此代码后，该帐户的序列号为 `3`，指示前两个帐户失败后将尝试使用此帐户。  
  
```  
EXECUTE msdb.dbo.sysmail_update_profileaccount_sp  
    @profile_name = 'AdventureWorks Administrator'  
    ,@account_name = 'Admin-BackupServer',  
    ,@sequence_number = 3;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
