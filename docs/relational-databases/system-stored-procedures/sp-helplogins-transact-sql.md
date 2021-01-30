---
description: sp_helplogins (Transact-SQL)
title: sp_helplogins (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplogins_TSQL
- sp_helplogins
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplogins
ms.assetid: f9ad3767-5b9f-420d-8922-b637811404f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b58e74a30ddd86fc2edbfc8db553ddf9449181db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183119"
---
# <a name="sp_helplogins-transact-sql"></a>sp_helplogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  提供有关每个数据库中的登录名以及与其相关的用户的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_helplogins [ [ @LoginNamePattern = ] 'login' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @LoginNamePattern = ] 'login'` 是登录名。 login 的数据类型为 sysname，默认值为 NULL。 如果指定，则必须存在 *登录名*。 如果未指定 *login* ，则返回有关所有登录名的信息。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 第一个报告包含有关指定的每个登录的信息，如下表所示。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|登录名。|  
|**SID**|**varbinary(85)**|登录安全标识符 (SID)。|  
|**DefDBName**|**sysname**|**LoginName** 在连接到实例时使用的默认数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**DefLangName**|**sysname**|**LoginName** 使用的默认语言。|  
|**Auser**|**char (5)**|Yes = **LoginName** 在数据库中具有关联的用户名。<br /><br /> No = **LoginName** 没有关联的用户名。|  
|**ARemote**|**char (7)**|Yes = **LoginName** 具有关联的远程登录名。<br /><br /> No = **LoginName** 没有关联的登录名。|  
  
 第二个报告包含有关映射到每个登录的用户的信息以及登录的角色成员身份，如下表所示。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|登录名。|  
|**DBName**|**sysname**|**LoginName** 在连接到实例时使用的默认数据库 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**UserName**|**sysname**|**LoginName** 在 **dbname** 中映射到的用户帐户，以及 **LoginName** 是 **dbname** 中的成员的角色。|  
|**UserOrAlias**|**char (8)**|MemberOf = **用户名** 是一个角色。<br /><br /> User = **UserName** 是用户帐户。|  
  
## <a name="remarks"></a>备注  
 删除登录名之前，请使用 **sp_helplogins** 标识映射到该登录名的用户帐户。  
  
## <a name="permissions"></a>权限  
 要求具有 **securityadmin** 固定服务器角色的成员身份。  
  
 若要确定映射到给定登录名的所有用户帐户， **sp_helplogins** 必须检查服务器中的所有数据库。 因此，对于服务器中的每个数据库，至少应满足下列条件之一：  
  
-   正在执行 **sp_helplogins** 的用户有权访问数据库。  
  
-   在数据库中启用了 **guest** 用户帐户。  
  
 如果 **sp_helplogins** 无法访问数据库， **sp_helplogins** 将返回尽可能多的信息，并显示错误消息15622。  
  
## <a name="examples"></a>示例  
 以下示例报告有关登录 `John` 的信息。  
  
```  
EXEC sp_helplogins 'John';  
GO  
  
LoginName SID                        DefDBName DefLangName AUser ARemote   
--------- -------------------------- --------- ----------- ----- -------   
John      0x23B348613497D11190C100C  master    us_english  yes   no  
  
(1 row(s) affected)  
  
LoginName   DBName   UserName   UserOrAlias   
---------   ------   --------   -----------   
John        pubs     John       User          
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_helpdb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpuser &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
