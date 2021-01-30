---
description: sys.sp_xtp_unbind_db_resource_pool (Transact-SQL)
title: sys.sp_xtp_unbind_db_resource_pool (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_xtp_unbind_db_resource_pool_TSQL
- sp_xtp_unbind_db_resource_pool
- sys.sp_xtp_unbind_db_resource_pool_TSQL
- sys.sp_xtp_unbind_db_resource_pool
dev_langs:
- TSQL
helpviewer_keywords:
- sp_xtp_unbind_db_resource_pool
- sys.sp_xtp_unbind_db_resource_pool
ms.assetid: 695a796d-087e-4bc8-99d0-ddc342604c75
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ec549ca71802d8c028ef12ef8293d178dff19f4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102675"
---
# <a name="syssp_xtp_unbind_db_resource_pool-transact-sql"></a>sys.sp_xtp_unbind_db_resource_pool (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  此系统过程会删除数据库与资源池之间的现有绑定，以便跟踪 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 内存使用量。  如果当前没有任何池绑定到指定数据库，则返回成功。 若数据库未绑定，则之前为内存优化对象分配的内存仍分配给上一个资源池。 您需要重新启动数据库才能释放分配的内存。 一旦数据库与资源池解除绑定，该绑定就会使用 DEFAULT 资源池。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```sql  
sys.sp_xtp_unbind_db_resource_pool 'database_name'  
```  
  
## <a name="arguments"></a>参数  
 database_name  
 启用了 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 的现有数据库的名称。  
  
#### <a name="parameters"></a>参数  
  
## <a name="messages"></a>消息  
 如果数据库绑定到指定资源池，则该过程成功返回。 但是，您必须重新启动该数据库，绑定才能生效。  
 如果未指定该数据库的现有绑定，则 `sp_xtp_unbind_db_resource_pool` 返回成功，但会显示以下信息性消息：  
  
```  
Msg 41374, Level 16, State 1, Procedure sp_xtp_unbind_db_resource_pool_internal, Line 140.  
Database 'Hekaton_DB' does not have a binding to a resource pool.  
  
```  
  
## <a name="example"></a>示例  
 以下代码将数据库 Hekaton_DB 从它所绑定到的 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 资源池解除绑定。  如果 Hekaton_DB 当前未绑定到 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 资源池，则会显示一条消息。 必须重新启动该数据库，取消绑定才会生效。  
  
```sql  
sys.sp_xtp_unbind_db_resource_pool 'Hekaton_DB'  
```  
  
## <a name="requirements"></a>要求  
  
-   由 `database_name` 指定的数据库必须绑定至 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] 资源池。  
  
-   需要 CONTROL SERVER 权限。  
  
## <a name="see-also"></a>另请参阅  
 [数据库与资源池绑定的指南，请参阅主题](../../relational-databases/in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md)   
 [sys.sp_xtp_bind_db_resource_pool (Transact-SQL)](../../relational-databases/system-stored-procedures/sys-sp-xtp-bind-db-resource-pool-transact-sql.md)  
  
  
