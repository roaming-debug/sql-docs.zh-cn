---
description: sp_getapplock (Transact-SQL)
title: sp_getapplock (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_getapplock_TSQL
- sp_getapplock
dev_langs:
- TSQL
helpviewer_keywords:
- application locks
- sp_getapplock
ms.assetid: e1e85908-9f31-47cf-8af6-88c77e6f24c9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 82c0c343d76c3fb8fe6275c2450b62d50fd11b34
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185764"
---
# <a name="sp_getapplock-transact-sql"></a>sp_getapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对应用程序资源设置锁。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_getapplock [ @Resource = ] 'resource_name' ,  
     [ @LockMode = ] 'lock_mode'   
     [ , [ @LockOwner = ] 'lock_owner' ]   
     [ , [ @LockTimeout = ] 'value' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>参数  
 [ @Resource =] "*resource_name*"  
 指定标识锁资源的名称的字符串。 应用程序必须确保该资源名称是唯一的。 指定的名称经过内部哈希运算后成为可以存储在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁管理器中的值。 *resource_name* 为 **nvarchar (255)** ，无默认值。 如果资源字符串的长度超过 **nvarchar (255)**，则它将被截断为 **nvarchar (255)**。  
  
 比较了二进制 *resource_name* ，因此无论当前数据库的排序规则设置如何，都是区分大小写的。  
  
> [!NOTE]  
>  一旦获取应用程序锁之后，则只能检索纯文本中的前 32 个字符；对剩余的字符执行哈希运算。  
  
 [ @LockMode =] "*lock_mode*"  
 要为特定资源获取的锁模式。 *lock_mode* 是 **varchar (32)** ，并且没有默认值。 该值可以是下列任意值： **Shared**、 **Update**、 **IntentShared**、 **IntentExclusive** 或 **Exclusive**。 有关详细信息，请参阅 [锁模式](../sql-server-transaction-locking-and-row-versioning-guide.md#lock_modes)。
  
 [ @LockOwner =] "*lock_owner*"  
 锁的所有者，它是请求锁时所指定的 lock_owner 值。 *lock_owner* 是 **varchar (32)**。 该值可以是 Transaction（默认值）或 Session。 默认情况下， *lock_owner* 值为 **transaction** 时，sp_getapplock 必须从事务内执行。  
  
 [ @LockTimeout =] "*value*"  
 锁超时值（毫秒）。 默认值与 @ 返回的值相同 @LOCK_TIMEOUT 。 若要指示锁请求应返回的返回代码为-1，而不是在不能立即授予请求时等待锁，请指定0。  
  
 [ @DbPrincipal =] "*database_principal*"  
 对数据库中的对象具有权限的用户、角色或应用程序角色。 函数的调用方必须是 *database_principal*、dbo 或 db_owner 固定数据库角色的成员，才能成功调用该函数。 默认值为 public。  
  
## <a name="return-code-values"></a>返回代码值  
 \>= 0 (成功) ，或 < 0 (失败)   
  
|值|结果|  
|-----------|------------|  
|0|锁已同时成功授予。|  
|1|在等待释放其他不兼容锁后成功授予锁。|  
|-1|锁请求超时。|  
|-2|锁请求被取消。|  
|-3|选择锁请求作为死锁牺牲品。|  
|-999|指示参数验证或其他调用错误。|  
  
## <a name="remarks"></a>备注  
 对资源设置的锁与当前事务或当前会话相关联。 当事务提交或回滚时，将释放与当前事务相关联的锁。 当会话注销时，将释放与会话关联的锁。当服务器出于任何原因而关闭时，将释放所有锁定。  
  
 sp_getapplock 创建的锁资源在会话的当前数据库中创建。 每个锁资源都由下列值的组合值进行标识：  
  
-   包含锁资源的数据库的数据库 ID。  
  
-   @DbPrincipal 参数中指定的数据库主体。  
  
-   @Resource 参数中指定的锁名。  
  
 只有 @DbPrincipal 参数中指定的数据库主体成员才能获取指定该主体的应用程序锁。 dbo 和 db_owner 角色成员被隐式视为所有角色成员。  
  
 可以使用 sp_releaseapplock 显式释放锁。 如果某个应用程序为同一锁资源多次调用 sp_getapplock，则必须调用 sp_releaseapplock 同样次数才能释放锁。  当使用锁所有者打开锁时 `Transaction` ，在提交或回滚事务时将释放该锁。
  
 如果为同一锁资源多次调用 sp_getapplock，但是在所有请求中指定的锁模式与现有模式不同，则对资源的影响将是两个锁模式的联合。 多数情况下，这意味着将锁模式提升为现有模式或新请求模式中更强的模式。 在最终释放锁之前，即使出现锁释放调用，也会一直保持这一更强的模式。 例如，在以下调用顺序中，将以 `Exclusive` 模式而非 `Shared` 模式控制资源。  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
EXEC @result = sp_releaseapplock @Resource = 'Form1';  
COMMIT TRANSACTION;  
GO  
```  
  
 如果应用程序锁发生死锁，则该死锁不会回滚请求此应用程序锁的事务。 必须手动完成任何可能需要作为返回值结果的回滚。 因此，建议在代码中使用错误检查，如果返回某些值（例如 -3），则启动 ROLLBACK TRANSACTION 或替代操作。  
  
 以下是示例：  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
IF @result = -3  
BEGIN  
    ROLLBACK TRANSACTION;  
END  
ELSE  
BEGIN  
    EXEC @result = sp_releaseapplock @Resource = 'Form1';  
    COMMIT TRANSACTION;  
END;  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用当前数据库 ID 来限定资源。 因此，如果在不同数据库中执行 sp_getapplock，即便使用了相同的参数值，结果仍然是对单独资源使用单独锁。  
  
 使用 sys.dm_tran_locks 动态管理视图或 sp_lock 系统存储过程检查锁信息，或使用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 监视锁。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例对 `Form1` 数据库中的资源 `AdventureWorks2012` 设置共享锁，使其与当前事务相关联。  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
 以下示例指定 `dbo` 作为主体数据库。  
  
```  
BEGIN TRAN;  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'AdventureWorks2012',   
     @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [APPLOCK_MODE &#40;Transact-sql&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-sql&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_releaseapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)  
  
  
