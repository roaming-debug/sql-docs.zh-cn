---
description: sp_droplinkedsrvlogin (Transact-SQL)
title: sp_droplinkedsrvlogin (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_droplinkedsrvlogin_TSQL
- sp_droplinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_droplinkedsrvlogin
ms.assetid: 75a4a040-72d5-4d29-8304-de0aa481ad4b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c6e67dc1b33fb5c5655992b11367ab60639c8654
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209321"
---
# <a name="sp_droplinkedsrvlogin-transact-sql"></a>sp_droplinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的本地服务器上的登录与链接服务器上的登录之间的现有映射。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_droplinkedsrvlogin [ @rmtsrvname= ] 'rmtsrvname' ,   
   [ @locallogin= ] 'locallogin'  
```  
  
## <a name="arguments"></a>参数  
`[ @rmtsrvname = ] 'rmtsrvname'` 应用登录映射的链接服务器的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 *rmtsrvname* 的值为 **sysname**，无默认值。 *rmtsrvname* 必须已存在。  
  
`[ @locallogin = ] 'locallogin'`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]本地服务器上的登录名，它具有到链接服务器的映射 *rmtsrvname*。 *locallogin* 的值为 **sysname**，无默认值。 必须已经存在 *locallogin* 到 *rmtsrvname* 的映射。 如果为 NULL，则会删除 **sp_addlinkedserver** 创建的默认映射，该映射将本地服务器上的所有登录名映射到链接服务器上的登录名。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 删除登录名的现有映射后，本地服务器将使用 **sp_addlinkedserver** 创建的默认映射，该映射代表该登录名连接到链接服务器。 若要更改默认映射，请使用 **sp_addlinkedsrvlogin**。  
  
 如果还删除了默认映射，则只有通过使用 **sp_addlinkedsrvlogin** 显式给定了到链接服务器的登录名映射的登录名可以访问链接服务器。  
  
 不能在用户定义的事务中执行 **sp_droplinkedsrvlogin** 。  
  
## <a name="permissions"></a>权限  
 要求对服务器拥有 ALTER ANY LOGIN 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-removing-the-login-mapping-for-an-existing-user"></a>A. 为现有用户删除登录映射  
 以下示例删除从本地服务器到链接服务器 `Mary` 的登录 `Accounts` 的映射。 因此，登录 `Mary` 使用默认登录映射。  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', 'Mary';  
```  
  
### <a name="b-removing-the-default-login-mapping"></a>B. 删除默认的登录映射  
 以下示例删除最初通过在链接服务器 `sp_addlinkedserver` 上执行 `Accounts` 而创建的默认登录映射。  
  
```  
EXEC sp_droplinkedsrvlogin 'Accounts', NULL;  
```  
  
## <a name="see-also"></a>另请参阅  
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
