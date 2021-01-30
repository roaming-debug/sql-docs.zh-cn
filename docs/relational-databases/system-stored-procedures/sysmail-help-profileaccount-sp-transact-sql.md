---
description: sysmail_help_profileaccount_sp (Transact-SQL)
title: sysmail_help_profileaccount_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_profileaccount_sp_TSQL
- sysmail_help_profileaccount_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_profileaccount_sp
ms.assetid: 3ea68271-0a6b-4d77-991c-4757f48f747a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45b542047875527a26d9c4854b223e626efda5ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181935"
---
# <a name="sysmail_help_profileaccount_sp-transact-sql"></a>sysmail_help_profileaccount_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出与一个或多个数据库邮件配置文件相关联的帐户。  
    
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_help_profileaccount_sp  
   {   [ @profile_id = ] profile_id   
      | [ @profile_name = ] 'profile_name' }  
   [ , {   [ @account_id = ] account_id  
         | [ @account_name = ] 'account_name' } ]  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_id = ] profile_id` 要列出的配置文件的配置文件 ID。 *profile_id* 的值为 **int**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要列出的配置文件的配置文件名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 必须指定 *profile_id* 或 *profile_name* 。  
  
`[ @account_id = ] account_id` 要列出的帐户 ID。 *account_id* 的值为 **int**，默认值为 NULL。 当 *account_id* 和 *account_name* 均为 NULL 时，将列出配置文件中的所有帐户。  
  
`[ @account_name = ] 'account_name'` 要列出的帐户的名称。 *account_name* 的默认值为 **sysname**，默认值为 NULL。 当 *account_id* 和 *account_name* 均为 NULL 时，将列出配置文件中的所有帐户。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 返回包含以下列的结果集。  
  
| 列名称 | 数据类型 | 说明 |
| ----------- | --------- | ----------- |
|**profile_id**|**int**|配置文件的配置文件 ID。|  
|**profile_name**|**sysname**|配置文件的名称。|  
|**account_id**|**int**|帐户的帐户 ID。|  
|**account_name**|**sysname**|帐户的名称。|  
|**sequence_number**|**int**|配置文件中的帐户的序号。|  
  
## <a name="remarks"></a>备注  
 如果未指定 *profile_id* 或 *profile_name* ，则此存储过程将返回实例中的每个配置文件的信息。  
  
 存储过程 **sysmail_help_profileaccount_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 **A. 按名称列出特定配置文件的帐户**  
  
 以下示例通过指定配置文件名称以列出 `AdventureWorks Administrator` 配置文件的信息。  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
   @profile_name = 'AdventureWorks Administrator';  
```  
  
 下面是示例结果集，由于行的长度原因而进行了编辑：  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **B. 按配置文件 ID 列出特定配置文件的帐户**  
  
 以下示例通过指定配置文件的配置文件 ID 以列出 `AdventureWorks Administrator` 配置文件的信息。  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp  
    @profile_id = 131 ;  
```  
  
 下面是示例结果集，由于行的长度原因而进行了编辑：  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
```  
  
 **C. 列出所有配置文件的帐户**  
  
 以下示例列出实例中的所有配置文件的帐户。  
  
```  
EXECUTE msdb.dbo.sysmail_help_profileaccount_sp;  
```  
  
 下面是示例结果集，由于行的长度原因而进行了编辑：  
  
```  
profile_id  profile_name                 account_id  account_name         sequence_number  
----------- ---------------------------- ----------- -------------------- ---------------  
131         AdventureWorks Administrator 197         Admin-MainServer     1  
131         AdventureWorks Administrator 198         Admin-BackupServer   2  
106         AdventureWorks Operator      210         Operator-MainServer  1  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
