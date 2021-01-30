---
description: sp_dropremotelogin (Transact-SQL)
title: sp_dropremotelogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropremotelogin
- sp_dropremotelogin_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropremotelogin
ms.assetid: 9f097652-a286-40b2-be73-568d77ada698
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: 7b468ff83bffdd581695b9553c636b06fbe6c784
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187038"
---
# <a name="sp_dropremotelogin-transact-sql"></a>sp_dropremotelogin (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除映射到本地登录名的远程登录名，该登录名用于针对运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地服务器执行远程存储过程。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 请改用链接服务器和链接服务器存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
sp_dropremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @remoteserver = ] 'remoteserver'` 远程服务器的名称，该远程服务器映射到要删除的远程登录名。 *remoteserver* 的值为 **sysname**，无默认值。 *remoteserver* 必须已存在。  
  
`[ @loginame = ] 'login'` 与远程服务器相关联的本地服务器上的可选登录名。 login 的数据类型为 sysname，默认值为 NULL。 如果指定，则必须已经存在 *登录名*。  
  
`[ @remotename = ] 'remote_name'` 在从远程服务器登录时映射到 *登录名* 的远程登录名的可选名称。 *remote_name* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 如果仅指定了 *remoteserver* ，则将从本地服务器中删除该远程服务器的所有远程登录名。 如果还指定了 *login* ，则从本地服务器中删除映射到该特定本地登录名的 *remoteserver* 中的所有远程登录名。 如果同时指定了 *remote_name* ，则将仅从本地 *服务器中删除* 该远程用户的远程登录名。  
  
 若要添加本地服务器用户，请使用 **sp_addlogin**。 若要删除本地服务器用户，请使用 **sp_droplogin**。  
  
 仅当使用早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时才需要远程登录。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 及更高版本改用链接服务器登录。 使用 **sp_addlinkedsrvlogin** 和 **sp_droplinkedsrvlogin** 添加和删除链接服务器登录名。  
  
 不能在用户定义的事务中执行 **sp_dropremotelogin** 。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 或 **securityadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-all-remote-logins-for-a-remote-server"></a>A. 删除远程服务器的所有远程登录  
 以下示例删除远程服务器 `ACCOUNTS` 的条目，因此将删除本地服务器中的登录和远程服务器中远程登录之间的所有映射。  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-dropping-a-login-mapping"></a>B. 删除登录映射  
 以下示例删除一个映射的条目，该条目将来自远程服务器 `ACCOUNTS` 的远程登录映射到本地登录 `Albert`。  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-dropping-a-remote-user"></a>C. 删除远程用户  
 以下示例删除远程服务器 `Chris` 上的远程登录 `ACCOUNTS`。该远程服务器被映射到本地登录 `salesmgr`。  
  
```sql
EXEC sp_dropremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>另请参阅  
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_addserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [sp_droplogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
