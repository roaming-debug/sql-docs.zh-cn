---
description: sp_denylogin (Transact-SQL)
title: sp_denylogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_denylogin_TSQL
- sp_denylogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_denylogin
ms.assetid: db80f152-e8af-4303-95b6-3a3a7b664374
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 7006aaa4a8c7ddf0603264d1c241d09a8e327f5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178099"
---
# <a name="sp_denylogin-transact-sql"></a>sp_denylogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  防止 Windows 用户或 Windows 组连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_denylogin [ @loginame = ] 'login'   
```  
  
## <a name="arguments"></a>参数  
`[ @loginame = ] 'login_ '` Windows 用户或组的名称。 *login* 的 **sysname** 为，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **sp_denylogin** 拒绝将 SQL 权限连接到映射到指定 windows 用户或 windows 组的服务器级别主体。 如果服务器主体不存在，将创建它。 新主体将在 [sys.server_principals &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中可见。  
  
 不能在用户定义的事务中执行 **sp_denylogin** 。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何使用 **sp_denylogin** 阻止 Windows 用户 `CORPORATE\GeorgeV` 连接到服务器。  
  
```  
EXEC sp_denylogin 'CORPORATE\GeorgeV';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
