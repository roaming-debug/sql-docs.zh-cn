---
description: sp_releaseapplock (Transact-SQL)
title: sp_releaseapplock (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_releaseapplock_TSQL
- sp_releaseapplock
dev_langs:
- TSQL
helpviewer_keywords:
- sp_releaseapplock
ms.assetid: 51b03c2f-0d54-40f5-9172-e747942d4a46
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 020a2964a4f73872669a4f9623a8c24aa2a88fd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185653"
---
# <a name="sp_releaseapplock-transact-sql"></a>sp_releaseapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  为应用程序资源释放锁。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_releaseapplock [ @Resource = ] 'resource_name'   
     [ , [ @LockOwner = ] 'lock_owner' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>参数  
 [ @Resource =] "*resource_name*"  
 由客户端应用程序指定的锁资源名称。 应用程序必须确保该资源是唯一的。 指定的名称经过内部哈希运算后成为可以存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁管理器中的值。 *resource_name* 为 **nvarchar (255)** ，无默认值。 比较了二进制 *resource_name* ，因此无论当前数据库的排序规则设置如何，都是区分大小写的。  
  
 [ @LockOwner =] "*lock_owner*"  
 锁的所有者，它是请求锁时所指定的 lock_owner 值。 lock_owner 是 nvarchar(32)。 该值可以是 Transaction（默认值）或 Session。 默认情况下， *lock_owner* 值为 **transaction** 时，sp_getapplock 必须从事务内执行。  
  
 [ @DbPrincipal =] "*database_principal*"  
 对数据库中的对象具有权限的用户、角色或应用程序角色。 函数的调用方必须是 *database_principal*、dbo 或 db_owner 固定数据库角色的成员，才能成功调用该函数。 默认值为 public。  
  
## <a name="return-code-values"></a>返回代码值  
 \>= 0 (成功) ，或 < 0 (失败)   
  
|值|结果|  
|-----------|------------|  
|0|成功释放锁。|  
|-999|指示参数验证或其他调用错误。|  
  
## <a name="remarks"></a>备注  
 如果某个应用程序为同一锁资源多次调用 sp_getapplock，则必须调用 sp_releaseapplock 同样次数才能释放锁。  
  
 服务器因任何原因而关闭时都将释放锁。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例将释放与 `Form1` 数据库中资源 `AdventureWorks2012` 的当前事务关联的锁。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'Form1',   
     @LockMode = 'Shared';  
EXEC sp_releaseapplock @DbPrincipal = 'dbo', @Resource = 'Form1';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [APPLOCK_MODE &#40;Transact-sql&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-sql&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_getapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
  
  
