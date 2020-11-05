---
title: 'sp_removedbreplication (T-sql) '
description: 描述用于删除发布数据库上的所有复制对象以进行 SQL Server 复制的 sp_removedbreplication 存储过程。
ms.custom: seo-lt-2019
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_removedbreplication
- sp_removedbreplication_TSQL
helpviewer_keywords:
- sp_removedbreplication
ms.assetid: cb98d571-d1eb-467b-91f7-a6e091009672
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b8b8918bf659e6965fed1f9af0342f8295947509
ms.sourcegitcommit: b3a711a673baebb2ff10d7142b209982b46973ae
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2020
ms.locfileid: "93364833"
---
# <a name="sp_removedbreplication-transact-sql"></a>sp_removedbreplication (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  此存储过程将删除 SQL Server 发布服务器实例的发布数据库上或 SQL Server 订阅服务器实例的订阅数据库上的所有复制对象。 在相应的数据库中执行，或如果是在另一个数据库上下文的同一个实例上执行，请指定应删除复制对象的数据库。 此过程不会从其他数据库（例如，分发数据库）中删除对象。  
  
> [!NOTE]  
>  只有当其他删除复制对象的方法都失败后，才应当使用此过程。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_removedbreplication [ [ @dbname = ] 'dbname' ]  
    [ , [ @type = ] type ]   
```  
  
## <a name="arguments"></a>自变量  
`[ @dbname = ] 'dbname'` 数据库的名称。 *dbname* 的数据类型为 **sysname** ，默认值为 NULL。 此参数值为 NULL 时，将使用当前数据库。  
  
`[ @type = ] type` 要为其删除数据库对象的复制的类型。 *类型* 为 **nvarchar (5)** ，可以为以下值之一。  
  
|||  
|-|-|  
|**梁**|删除事务复制发布对象。|  
|**merge**|删除合并复制发布对象。|  
|**(默认** 值) |删除所有复制发布对象。|  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 **sp_removedbreplication** 在所有类型的复制中使用。  
  
 如果要还原的复制数据库没有需要还原的复制对象， **sp_removedbreplication** 会很有用。  
  
 **sp_removedbreplication** 不能用于标记为只读的数据库。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员才能 **sp_removedbreplication** 执行。  
  
## <a name="examples"></a>示例

### <a name="a-remove-replication-objects-adventureworks2012replica-subscription-database"></a>A. 删除复制对象，AdventureWorks2012Replica 订阅数据库
 [!code-sql[HowTo#sp_removedbreplication](../../relational-databases/replication/codesnippet/tsql/sp-removedbreplication-t_1.sql)]  
  
### <a name="b-remove-replication-objects-adventureworksreplica-subscription-database"></a>B. 删除复制对象，AdventureWorksReplica 订阅数据库
  
```sql
-- Remove replication objects from the subscription database on MYSUB.  
DECLARE @subscriptionDB AS sysname  
SET @subscriptionDB = N'AdventureWorksReplica'  
  
-- Remove replication objects from a subscription database (if necessary).  
USE master  
EXEC sp_removedbreplication @subscriptionDB  
GO  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [禁用发布和分发](../../relational-databases/replication/disable-publishing-and-distribution.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
