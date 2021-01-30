---
description: sp_addremotelogin (Transact-SQL)
title: sp_addremotelogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addremotelogin_TSQL
- sp_addremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addremotelogin
ms.assetid: 71b7cd36-a17d-4b12-b102-10aeb0f9268b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 13b935d91772d70d58b38f9f995341d81e04388f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165191"
---
# <a name="sp_addremotelogin-transact-sql"></a>sp_addremotelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在本地服务器上添加新的远程登录 ID。 这样，远程服务器就能够连接并执行远程过程调用。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] 请改用链接服务器和链接服务器存储过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_addremotelogin [ @remoteserver = ] 'remoteserver'   
     [ , [ @loginame = ] 'login' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>参数  
 [ @remoteserver **=** ] **\ "**_remoteserver_**"**  
 远程登录名所适用的远程服务器的名称。 *remoteserver* 的值为 **sysname**，无默认值。 如果仅指定了 *remoteserver* ，则 *remoteserver* 上的所有用户将映射到本地服务器上具有相同名称的现有登录名。 对于本地服务器而言，远程服务器必须是已知的。 可以使用 sp_addserver 来添加。 当 *remoteserver* 上的用户连接到运行的本地服务器 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以执行远程存储过程时，它们将作为本地登录名，该登录名与其在 *remoteserver* 上的登录名匹配。 *remoteserver* 是启动远程过程调用的服务器。  
  
 [ @loginame **=** ] **"**_login_**"**  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本地实例上的用户的登录 ID。 login 的数据类型为 sysname，默认值为 NULL。 *登录名* 必须在本地实例上已经存在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果指定 *login* ，则 *remoteserver* 上的所有用户将映射到该特定本地登录名。 当 *remoteserver* 上的用户连接到的本地实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以执行远程存储过程时，它们将作为 *登录名* 进行连接。  
  
 [ @remotename **=** ] **'**_remote_name_**'**  
 远程服务器上的用户的登录 ID。 *remote_name* 的默认值为 **sysname**，默认值为 NULL。 *remoteserver* 上必须存在 *remote_name* 。 如果指定 *remote_name* ，则特定用户 *remote_name* 会映射到本地服务器上的 *登录名* 。 当 *remoteserver* 上的 *remote_name* 连接到的本地实例 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以执行远程存储过程时，它将作为 *登录名* 进行连接。 *Remote_name* 的登录 id 可以不同于远程服务器上的登录 id *登录名*。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 若要执行分布式查询，请使用 sp_addlinkedsrvlogin。  
  
 不能在用户定义的事务中使用 sp_addremotelogin。  
  
## <a name="permissions"></a>权限  
 只有 sysadmin 和 securityadmin 固定服务器角色的成员才能执行 sp_addremotelogin。  
  
## <a name="examples"></a>示例  
  
### <a name="a-mapping-one-to-one"></a>A. 一对一映射  
 以下示例在远程服务器 `ACCOUNTS` 和本地服务器具有相同的用户登录名时将远程名称映射到本地名称。  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS';  
```  
  
### <a name="b-mapping-many-to-one"></a>B. 多对一映射  
 以下示例创建一个条目，该条目将来自远程服务器 `ACCOUNTS` 的所有用户都映射到本地登录 ID `Albert`。  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'Albert';  
```  
  
### <a name="c-using-explicit-one-to-one-mapping"></a>C. 使用显式一对一映射  
 以下示例将来自远程服务器 `Chris` 上的远程用户 `ACCOUNTS` 的远程登录映射到本地用户 `salesmgr`。  
  
```  
EXEC sp_addremotelogin 'ACCOUNTS', 'salesmgr', 'Chris';  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_addserver &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)   
 [sp_dropremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_helpremotelogin &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpremotelogin-transact-sql.md)   
 [sp_helpserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [sp_revokelogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-revokelogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
