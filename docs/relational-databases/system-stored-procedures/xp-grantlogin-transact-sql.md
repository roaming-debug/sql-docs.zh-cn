---
description: xp_grantlogin (Transact-SQL)
title: xp_grantlogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xp_grantlogin
- xp_grantlogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- xp_grantlogin
ms.assetid: c851c1ab-3b29-4b99-9902-78c2665a844b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d31a966e6b882a7901f156b150ce7ba13c9680c2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171683"
---
# <a name="xp_grantlogin-transact-sql"></a>xp_grantlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予 Windows 组或用户对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的访问权限。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
xp_grantlogin {[@loginame = ] 'login'} [,[@logintype = ] 'logintype']  
```  
  
## <a name="arguments"></a>参数  
`[ @loginame = ] 'login'` 要添加的 Windows 用户或组的名称。 Windows 用户或组必须使用 *域* 用户的 windows 域名进行限定 \\ 。 *login* 的 **sysname** 为，无默认值。  
  
`[ @logintype = ] 'logintype'` 被授予访问权限的登录名的安全级别。 *logintype* 是 **varchar (5)**，默认值为 NULL。 只能指定 **管理员** 。 如果指定 **admin** ，则授予 *登录名* 的访问权限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，并将其添加为 **sysadmin** 固定服务器角色的成员。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 **xp_grantlogin** 现在是一个系统存储过程，而不是扩展存储过程。 **xp_grantlogin** 调用 **sp_grantlogin** 和 **sp_addsrvrolemember**。  
  
## <a name="permissions"></a>权限  
 要求具有 **securityadmin** 固定服务器角色的成员身份。 更改 *logintype* 时，需要 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [sp_denylogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的常规扩展存储过程 ](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md)   
 [xp_enumgroups &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/xp-enumgroups-transact-sql.md)   
 [xp_loginconfig &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/xp-loginconfig-transact-sql.md)   
 [xp_logininfo &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/xp-logininfo-transact-sql.md)   
 [sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)  
  
  
