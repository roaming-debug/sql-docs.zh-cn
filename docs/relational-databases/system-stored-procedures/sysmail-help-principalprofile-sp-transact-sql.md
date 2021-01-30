---
description: sysmail_help_principalprofile_sp (Transact-SQL)
title: sysmail_help_principalprofile_sp (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: efb4b5cd655bf4a401d530e4c9bd52cbc9660a65
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181961"
---
# <a name="sysmail_help_principalprofile_sp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出有关数据库邮件配置文件和数据库主体之间的关联的信息。  
  
 
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>参数  
`[ @principal_id = ] principal_id` 关联到列表的 **msdb** 数据库中数据库用户或角色的 ID。 *principal_id* 的值为 **int**，默认值为 NULL。 可以指定 *principal_id* 或 *principal_name* 。  
  
`[ @principal_name = ] 'principal_name'` 要列出关联的 **msdb** 数据库中数据库用户或角色的名称。 *principal_name* 的默认值为 **sysname**，默认值为 NULL。 可以指定 *principal_id* 或 *principal_name* 。  
  
`[ @profile_id = ] profile_id` 要列出的关联的配置文件的 ID。 *profile_id* 的值为 **int**，默认值为 NULL。 可以指定 *profile_id* 或 *profile_name* 。  
  
`[ @profile_name = ] 'profile_name'` 要列出的关联的配置文件的名称。 *profile_name* 的默认值为 **sysname**，默认值为 NULL。 可以指定 *profile_id* 或 *profile_name* 。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-sets"></a>结果集  
 返回一个结果集，其中包含下表中列出的列。  
  
| 列名称 | 数据类型 | 说明 |
| ----------- | --------- | ----------- |
|principal_id|**int**|数据库用户的 ID。|  
|principal_name|**sysname**|数据库用户的名称。|  
|**profile_id**|**int**|数据库邮件配置文件的 ID 号。|  
|**profile_name**|**sysname**|数据库邮件配置文件的名称。|  
|**is_default**|**bit**|声明配置文件是否为用户的默认配置文件的标志。|  
  
## <a name="remarks"></a>备注  
 如果调用不带参数的 **sysmail_help_principalprofile_sp** ，则返回的结果集将列出实例中的所有关联 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 否则，结果集将包含与所提供参数相匹配的关联的信息。 例如，提供配置文件名称时，该过程会列出配置文件的所有关联。  
  
 **sysmail_help_principalprofile_sp** 在 **msdb** 数据库中，并且由 **dbo** 架构拥有。 如果当前数据库不是 **msdb**，则必须使用由三部分组成的名称来执行该过程。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-listing-information-for-a-specific-association"></a>A. 列出特定关联的信息  
 以下示例显示如何列出 `AdventureWorks Administrator` 数据库中 `ApplicationLogin` 配置文件和 `msdb` 主体之间的所有关联的信息。  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 下面是行长度经过调整的示例结果集。  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>B. 列出所有关联的信息  
 以下示例显示如何列出该实例中所有关联的信息。  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 下面是行长度经过调整的示例结果集。  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库邮件](../../relational-databases/database-mail/database-mail.md)   
 [数据库邮件存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
