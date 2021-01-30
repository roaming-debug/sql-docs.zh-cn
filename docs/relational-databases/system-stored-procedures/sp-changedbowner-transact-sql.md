---
description: sp_changedbowner (Transact-SQL)
title: sp_changedbowner (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changedbowner
- sp_changedbowner_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changedbowner
ms.assetid: 516ef311-e83b-45c9-b9cd-0e0641774c04
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c11630e73b50eae7c1f972ac7bff1e6d809a464f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159023"
---
# <a name="sp_changedbowner-transact-sql"></a>sp_changedbowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改当前数据库的所有者。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改为使用 [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_changedbowner [ @loginame = ] 'login'  
     [ , [ @map = ] remap_alias_flag ]  
```  
  
## <a name="arguments"></a>参数  
 [ @loginame =] "*login*"  
 当前数据库的新所有者的登录 ID。 *login* 的 **sysname** 为，无默认值。 *登录名* 必须是现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 Windows 用户。 如果用户已通过数据库内现有的用户安全帐户访问了数据库，则该 *登录名* 不能成为当前数据库的所有者。 为了避免发生上述情况，请首先删除当前数据库内的用户。  
  
 [ @map =] *remap_alias_flag*  
 *Remap_alias_flag* 参数已弃用，因为已从中删除登录别名 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 使用 *remap_alias_flag* 参数不会导致错误，但不会产生任何影响。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 执行 sp_changedbowner 之后，新所有者称为数据库中的 dbo 用户。 dbo 拥有执行数据库中所有活动的暗示性权限。  
  
 不能更改 master、model 或 tempdb 系统数据库的所有者。  
  
 若要显示有效 *登录* 值的列表，请执行 sp_helplogins 存储过程。  
  
 仅通过 *登录* 参数执行 sp_changedbowner 会将数据库所有权更改为 *登录名*。  
  
 使用 ALTER AUTHORIZATION 语句可以更该任意安全对象的所用者。 有关详细信息，请参阅 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求具有对数据库的 TAKE OWNERSHIP 权限。 如果新所有者在数据库中具有对应的用户，则要求具有对登录名的 IMPERSONATE 权限，否则要求具有对服务器的 CONTROL SERVER 权限。  
  
## <a name="examples"></a>示例  
 以下示例将登录名 `Albert` 设为当前数据库的所有者。  
  
```  
EXEC sp_changedbowner 'Albert';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-transact-sql.md)   
 [sp_dropalias &#40;Transact-sql&#41;](./system-stored-procedures-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helpdb &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helplogins &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helplogins-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
