---
description: sp_changeobjectowner (Transact-SQL)
title: sp_changeobjectowner (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 482fcbdb2d7c1b79eb0a8b5ed88d1295785b994b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159831"
---
# <a name="sp_changeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改当前数据库中对象的所有者。  
  
> [!IMPORTANT]
>  此存储过程仅适用于中可用的对象 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER SCHEMA](../../t-sql/statements/alter-schema-transact-sql.md) 或 [alter AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) 。 **sp_changeobjectowner** 同时更改架构和所有者。 若要保持与早期版本 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的兼容性，如果当前所有者和新所有者拥有的架构名称与它们的数据库用户名相同，则此存储过程将只更改对象所有者。  
> 
> [!IMPORTANT]
>  已经将新的权限要求添加到此存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>参数  
`[ @objname = ] 'object'` 当前数据库中现有的表、视图、用户定义函数或存储过程的名称。 *对象* 是一个 **nvarchar (776)**，无默认值。 *对象* 可以用现有对象的所有者限定，格式为 _existing_owner_**。** 如果架构及其所有者具有相同的名称，则为 _对象_。  
  
`[ @newowner = ] 'owner_ '` 将成为对象的新所有者的安全帐户的名称。 *所有者* 为 **sysname**，无默认值。 *所有者* 必须是有权访问当前数据库的有效数据库用户、服务器角色、 [!INCLUDE[msCoName](../../includes/msconame-md.md)] windows 登录名或 windows 组。 如果新所有者是没有对应数据库级主体的 Windows 用户或 Windows 组，则将创建数据库用户。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_changeobjectowner** 从对象中删除所有现有权限。 在运行 **sp_changeobjectowner** 后，您必须重新应用想要保留的任何权限。 因此，建议您在运行 **sp_changeobjectowner** 之前编写现有权限的脚本。 更改了对象的所有权之后，便可使用该脚本重新应用权限。 在运行该脚本之前必须在权限脚本中修改对象所有者。  
  
 若要更改安全对象的所有者，请使用 ALTER AUTHORIZATION. 若要更改架构，请使用 ALTER SCHEMA。  
  
## <a name="permissions"></a>权限  
 需要 **db_owner** 固定数据库角色的成员身份，或者 **db_ddladmin** 固定数据库角色和 **db_securityadmin** 固定数据库角色的成员身份，同时还要求对对象拥有 CONTROL 权限。  
  
## <a name="examples"></a>示例  
 下面的示例将表的所有者更改 `authors` 为 `Corporate\GeorgeW` 。  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SCHEMA (Transact-SQL)](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
