---
description: APPLOCK_TEST (Transact-SQL)
title: APPLOCK_TEST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- APPLOCK_TEST_TSQL
- APPLOCK_TEST
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], applications
- APPLOCK_TEST function
- applications [SQL Server], locks
- sessions [SQL Server], application locks
- testing application locks
ms.assetid: 4ea33d04-f8e9-46ff-ae61-985bd3eaca2c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4babfd8fba4184e255b366b8a949fe195df466de
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98089699"
---
# <a name="applock_test-transact-sql"></a>APPLOCK_TEST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函数返回信息指示是否可以为指定锁所有者授予对某种资源的锁，而不必获取锁。 作为应用程序锁函数，APPLOCK_TEST 对当前数据库执行操作。 数据库是应用程序锁的作用域。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
APPLOCK_TEST ( 'database_principal' , 'resource_name' , 'lock_mode' , 'lock_owner' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**'** *database_principal* **'**  
可将对数据库中对象的权限授予它们的用户、角色或应用程序角色。 若要成功调用函数，该函数调用方必须是 database_principal、dbo 或 db_owner 固定数据库角色的成员。
  
**'** *resource_name* **'**  
由客户端应用程序指定的锁资源名称。 应用程序必须确保唯一的资源名称。 指定的名称经过内部哈希运算后成为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁管理器可内部存储的值。  resource_name 为 nvarchar(255)，无默认值。 resource_name 使用二进制比较并区分大小写，无论当前数据库的排序规则设置为何。
  
**'** *lock_mode* **'**  
要为特定资源获取的锁模式。 lock_mode 为 nvarchar(32)，无默认值。 lock_mode 可具有任何以下值：Shared、Update、IntentShared、IntentExclusive、Exclusive。
  
**'** *lock_owner* **'**  
锁的所有者，它是请求锁时所指定的 lock_owner 值。 lock_owner 为 nvarchar(32)，值可以是 Transaction（默认值）或 Session。 如果显式指定默认值或 Transaction，则必须从事务中执行 APPLOCK_TEST。
  
## <a name="return-types"></a>返回类型
**smallint**
  
## <a name="return-value"></a>返回值
如果无法将锁授予指定的所有者，则为 0；如果可以授予锁，则为 1。
  
## <a name="function-properties"></a>函数属性
**不具有确定性**
  
**Nonindexable**
  
**Nonparallelizable**
  
## <a name="examples"></a>示例  
具有不同会话的两个用户（用户 A 和用户 B）按以下顺序运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。
  
用户 A 运行：
  
```sql
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result INT;  
EXEC @result=sp_getapplock  
    @DbPrincipal='public',  
    @Resource='Form1',  
    @LockMode='Shared',  
    @LockOwner='Transaction';  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
GO  
```  
  
然后用户 B 运行：
  
```sql
Use AdventureWorks2012;  
GO  
BEGIN TRAN;  
SELECT APPLOCK_MODE('public', 'Form1', 'Transaction');  
--Result set: NoLock  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Shared', 'Transaction');  
--Result set: 1 (Lock is grantable.)  
  
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: 0 (Lock is not grantable.)  
GO  
```  
  
然后用户 A 运行：
  
```sql
EXEC sp_releaseapplock @Resource='Form1', @DbPrincipal='public';  
GO  
```  
  
然后用户 B 运行：
  
```sql
SELECT APPLOCK_TEST('public', 'Form1', 'Exclusive', 'Transaction');  
--Result set: '1' (The lock is grantable.)  
GO  
```  
  
然后用户 A 和用户 B 同时运行：
  
```sql
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>另请参阅
[APPLOCK_MODE (Transact-SQL)](../../t-sql/functions/applock-mode-transact-sql.md)  
[sp_getapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-getapplock-transact-sql.md)  
[sp_releaseapplock (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)
