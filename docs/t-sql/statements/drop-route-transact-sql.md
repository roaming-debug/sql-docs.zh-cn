---
description: DROP ROUTE (Transact-SQL)
title: DROP ROUTE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP ROUTE
- DROP_ROUTE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- dropping routes
- DROP ROUTE statement
- deleting routes
- routes [Service Broker], removing
- removing routes
ms.assetid: d8fab0bc-d54a-46ca-9437-552db7477d40
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0a699415666b17a53ebbedf596a16ebfe8fae4e7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080993"
---
# <a name="drop-route-transact-sql"></a>DROP ROUTE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除路由，并从当前数据库的路由表中删除该路由的信息。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP ROUTE route_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 route_name   
 要删除的路由名称。 不能指定服务器、数据库和架构名称。  
  
## <a name="remarks"></a>注解  
 存储路由的路由表是一个元数据表，可通过目录视图 sys.routes 来读取。 路由表只能通过 CREATE ROUTE、ALTER ROUTE 和 DROP ROUTE 语句进行更新。  
  
 对于一个路由，无论是否有会话使用它，都可以删除它。 不过，如果没有到远程服务的其他路由，则这些会话的消息将保留在传输队列中，直到创建了到远程服务的路由或会话超时为止。  
  
## <a name="permissions"></a>权限  
 删除路由的权限默认授予该路由的所有者、db_ddladmin 或 db_owner 固定数据库角色的成员和 sysadmin 固定服务器角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例将删除 `ExpenseRoute` 路由。  
  
```sql  
DROP ROUTE ExpenseRoute ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER ROUTE (Transact-SQL)](../../t-sql/statements/alter-route-transact-sql.md)   
 [CREATE ROUTE (Transact SQL)](../../t-sql/statements/create-route-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.routes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-routes-transact-sql.md)  
  
  
