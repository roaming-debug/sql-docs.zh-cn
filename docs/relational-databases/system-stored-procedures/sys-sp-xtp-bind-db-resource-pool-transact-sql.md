---
description: sys.sp_xtp_bind_db_resource_pool (Transact-SQL)
title: sys.sp_xtp_bind_db_resource_pool (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/03/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xtp_bind_db_resource_pool_TSQL
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool_TSQL
- sys.sp_xtp_bind_db_resource_pool
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xtp_bind_db_resource_pool
- sys.sp_xtp_bind_db_resource_pool
ms.assetid: c2a78073-626b-4159-996e-1808f6bfb6d2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 73312fdb49f223f8a275ade2a13bf2d988649404
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205083"
---
# <a name="syssp_xtp_bind_db_resource_pool-transact-sql"></a>sys.sp_xtp_bind_db_resource_pool (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  将指定的 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 数据库绑定到指定资源池。 在执行 `sys.sp_xtp_bind_db_resource_pool` 之前，该数据库和资源池必须都存在。  
  
 此系统过程在由 resource_pool_name 标识的资源调控器池与由 database_name 标识的数据库之间创建绑定。 不要求该数据库在绑定时包含任何内存优化对象。 在缺少内存优化对象的情况下，将不会从资源池获取内存。 此绑定将由资源调控器使用，以管理通过下面所述的 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 分配器分配的内存。  
  
 如果给定数据库已有绑定，则该过程返回错误。  数据库任何时候都不能具有一个以上的活动绑定。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
  
## <a name="syntax"></a>语法  
  
```sql  
sys.sp_xtp_bind_db_resource_pool 'database_name', 'resource_pool_name'  
```  
  
## <a name="arguments"></a>参数  
 database_name  
 启用了 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 的现有数据库的名称。  
  
 resource_pool_name  
 现有资源池的名称。  
  
## <a name="messages"></a>消息  
 发生错误时，`sp_xtp_bind_db_resource_pool` 返回以下消息之一。  
  
 **数据库不存在**  
 Database_name 必须引用现有数据库。 如果没有具有指定 ID 的数据库，则返回以下消息：   
*数据库 ID% d 不存在。 请为此绑定使用有效的数据库 ID。*  
  
```  
Msg 911, Level 16, State 18, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB213' does not exist. Make sure that the name is entered correctly.  
```  
  
**数据库是系统数据库**  
 无法在系统数据库中创建 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 表。  因此，为这种数据库创建 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 内存绑定是无效的。  返回以下错误：  
*Database_name% s 引用系统数据库。 资源池只能绑定到用户数据库。*  
  
```  
Msg 41371, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Binding to a resource pool is not supported for system database 'master'. This operation can only be performed on a user database.  
```  
  
**资源池不存在**  
 在执行 `sp_xtp_bind_db_resource_pool` 之前，由 resource_pool_name 标识的资源池必须存在。  如果没有具有指定 ID 的池，则返回以下错误：  
*资源池% s 不存在。 请输入有效的资源池名称。*  
  
```  
Msg 41370, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Resource pool 'Pool_Hekaton' does not exist or resource governor has not been reconfigured.  
```  
  
**Pool_name 引用了保留系统池**  
 池名称 "内部" 和 "默认" 保留用于系统池。  将数据库显式绑定至两者之一将是无效的。  如果输入了系统池名称，则返回以下错误：  
*资源池% s 是系统资源池。 不能使用此过程将系统资源池显式绑定到数据库。*  
  
```  
Msg 41373, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 51  
Database 'Hekaton_DB' cannot be explicitly bound to the resource pool 'internal'. A database can only be bound only to a user resource pool.  
```  
  
**数据库已绑定至其他资源池**  
 在任何时候，数据库都只能绑定至一个资源池。 必须显式删除与资源池的数据库绑定，才能将数据库绑定至其他池。 请参阅 [&#40;transact-sql&#41;sys.sp_xtp_unbind_db_resource_pool ](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)。  
*数据库% s 已绑定到资源池% s。 必须先取消绑定，然后才能创建新绑定。*  
  
```  
Msg 41372, Level 16, State 1, Procedure sp_xtp_bind_db_resource_pool_internal, Line 54  
Database 'Hekaton_DB' is currently bound to a resource pool. A database must be unbound before creating a new binding.  
```  
  
 若成功，则 `sp_xtp_bind_db_resource_pool` 返回以下消息。  
  
**绑定成功**  
 若成功，则该函数返回以下成功消息，此消息记录在 SQL ERRORLOG 中  
*已在具有 ID %d 的数据库与具有 ID %d 的资源池之间成功创建资源绑定。*  
  
## <a name="examples"></a>示例  
A.  以下代码示例将数据库 Hekaton_DB 绑定至资源池 Pool_Hekaton。  
  
```sql  
sys.sp_xtp_bind_db_resource_pool N'Hekaton_DB', N'Pool_Hekaton'  
```  
 
 下次该数据库联机时，此绑定生效。  
 
 B. 上面示例的扩展示例，其中包括一些基本检查。  [!INCLUDE[tsql](../../includes/tsql-md.md)]在中执行以下[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]\:
 
```sql
DECLARE @resourcePool sysname = N'Pool_Hekaton';
DECLARE @database sysname = N'Hekaton_DB';

-- Check whether resource pool exists
IF NOT EXISTS (
    SELECT * FROM sys.resource_governor_resource_pools WHERE name = @resourcePool
    )
BEGIN
    SELECT N'Resource pool "' + @resourcePool + N'" does not exist or resource governor has not been reconfigured.';
END
-- Check whether database is already bound to a resource pool
ELSE IF EXISTS (
    SELECT p.name
    FROM sys.databases d
    JOIN sys.resource_governor_resource_pools p
    ON d.resource_pool_id = p.pool_id
    WHERE d.name = @database
    )
BEGIN
    SELECT N'Database "' + @database + N'" is currently bound to resource pool "' + @resourcePool  + N'". A database must be unbound before creating a new binding.';
END
-- Bind resource pool to database.
ELSE BEGIN
    EXEC sp_xtp_bind_db_resource_pool @database, @resourcePool; 
END 
``` 
  
## <a name="requirements"></a>要求  
  
-   绑定之前，由 `database_name` 指定的数据库以及由 `resource_pool_name` 指定的资源池必须存在。  
  
-   需要 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>另请参阅  
 [数据库与资源池绑定的指南，请参阅主题](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [sys.sp_xtp_unbind_db_resource_pool (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-unbind-db-resource-pool-transact-sql.md)  
  
  
