---
description: sp_defaultdb (Transact-SQL)
title: sp_defaultdb (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_defaultdb_TSQL
- sp_defaultdb
dev_langs:
- TSQL
helpviewer_keywords:
- sp_defaultdb
ms.assetid: 663b859f-c6da-4942-95a6-60b93d05654e
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a4c644fed26ea2ce80f0f827eb7b3b0f6b000d66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196081"
---
# <a name="sp_defaultdb-transact-sql"></a>sp_defaultdb (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更改登录名的默认数据库 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_defaultdb [ @loginame = ] 'login', [ @defdb = ] 'database'   
```  
  
## <a name="arguments"></a>参数  
`[ @loginame = ] 'login'` 登录名。 *login* 的 **sysname** 为，无默认值。 *登录名* 可以是现有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，也可以是 Windows 用户或组。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不存在 Windows 用户或用户组的登录名，则会自动添加该登录名。  
  
`[ @defdb = ] 'database'` 新的默认数据库的名称。 *数据库* 为 **sysname**，无默认值。 *数据库* 必须已存在。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_defaultdb** 调用 ALTER LOGIN。 此语句支持附加选项。 有关更改默认数据库的信息，请参阅 [ALTER LOGIN &#40;transact-sql&#41;](../../t-sql/statements/alter-login-transact-sql.md)。  
  
 不能在用户定义的事务中执行 **sp_defaultdb** 。  
  
## <a name="permissions"></a>权限  
 需要 ALTER ANY LOGIN 权限。  
  
## <a name="examples"></a>示例  
 以下示例将 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 设置为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Victoria` 的默认数据库。  
  
```  
EXEC sp_defaultdb 'Victoria', 'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_droplogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_grantdbaccess (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
