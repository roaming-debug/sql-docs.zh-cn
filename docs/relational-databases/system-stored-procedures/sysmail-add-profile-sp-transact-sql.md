---
description: sysmail_add_profile_sp (Transact-SQL)
title: sysmail_add_profile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_add_profile_sp_TSQL
- sysmail_add_profile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_add_profile_sp
ms.assetid: a828e55c-633a-41cf-9769-a0698b446e6c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4eeb85dee2493a07b1476a25fc90f23c5ad42aeb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207977"
---
# <a name="sysmail_add_profile_sp-transact-sql"></a>sysmail_add_profile_sp (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  创建新的数据库邮件配置文件。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_add_profile_sp [ @profile_name = ] 'profile_name'  
    [ , [ @description = ] 'description' ]  
    [ , [ @profile_id = ] new_profile_id OUTPUT ]  
```  
  
## <a name="arguments"></a>参数  
`[ @profile_name = ] 'profile\_name'` 新配置文件的名称。 *profile_name* **sysname**，无默认值。  
 
   > [!NOTE]
   > 必须调用使用 Azure SQL 托管实例 SQL 代理的配置文件名称 **AzureManagedInstance_dbmail_profile**
  
`[ @description = ] 'description'` 新配置文件的可选说明。 *描述* 为 **nvarchar (256)**，无默认值。  
  
`[ @profile_id = ] _new\_profile\_id OUTPUT` 返回新配置文件的 ID。 *new_profile_id* 的值为 **int**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 数据库邮件配置文件可以保存任意数目的数据库邮件帐户。 数据库邮件存储过程可以通过配置文件名称或此过程生成的配置文件 ID 来引用配置文件。 有关将帐户添加到配置文件的详细信息，请参阅 [&#40;transact-sql&#41;sysmail_add_profileaccount_sp ](../../relational-databases/system-stored-procedures/sysmail-add-profileaccount-sp-transact-sql.md)。  
  
 配置文件的名称和描述可以通过存储过程进行更改 **sysmail_update_profile_sp**，而配置文件 id 在配置文件的生存期内保持不变。  
  
 配置文件名称对于 Microsoft [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 必须是唯一的，否则存储过程将返回一个错误。  
  
 存储过程 **sysmail_add_profile_sp** 在 **msdb** 数据库中，由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 此过程的执行权限默认授予 **sysadmin** 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 **A. 创建一个新配置文件**  
  
 以下示例将创建一个名为 `AdventureWorks Administrator` 的新数据库邮件配置文件。  
  
```  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
       @profile_name = 'AdventureWorks Administrator',  
       @description = 'Profile used for administrative mail.' ;  
```  
  
 **B. 创建一个新配置文件，并将配置文件 ID 保存在一个变量中**  
  
 以下示例将创建一个名为 `AdventureWorks Administrator` 的新数据库邮件配置文件。 该示例将配置文件 ID 号存储在变量 `@profileId` 中，并返回一个包含新配置文件的配置文件 ID 号的结果集。  
  
```  
DECLARE @profileId INT ;  
  
EXECUTE msdb.dbo.sysmail_add_profile_sp  
       @profile_name = 'AdventureWorks Administrator',  
       @description = 'Profile used for administrative mail.',  
       @profile_id = @profileId OUTPUT ;  
  
SELECT @profileId ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [创建数据库邮件帐户](../../relational-databases/database-mail/create-a-database-mail-account.md)   
 [数据库邮件配置对象](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
