---
description: DBCC FREESYSTEMCACHE (Transact-SQL)
title: DBCC FREESYSTEMCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- FREESYSTEMCACHE_TSQL
- DBCC_FREESYSTEMCACHE_TSQL
- DBCC FREESYSTEMCACHE
- FREESYSTEMCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- clearing unused cache entries
- DBCC FREESYSTEMCACHE statement
- unused cache entries
- releasing unused cache entries
- freeing unused cache entries
- cleaning unused cache entries
ms.assetid: 4b5c460b-e4ad-404a-b4ca-d65aba38ebbb
author: pmasl
ms.author: umajay
ms.openlocfilehash: f99d6e50aed43273dbcaa659f95a8bb8a1fe73d3
ms.sourcegitcommit: 544706f6725ec6cdca59da3a0ead12b99accb2cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92638980"
---
# <a name="dbcc-freesystemcache-transact-sql"></a>DBCC FREESYSTEMCACHE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

从所有缓存中释放所有未使用的缓存条目。 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]会事先在后台清理未使用的缓存条目，以使内存可用于当前条目。 但是，可以使用此命令从每个缓存中或者从指定的 Resource Governor 池缓存中手动删除未使用的条目。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
```syntaxsql
DBCC FREESYSTEMCACHE   
    ( 'ALL' [, pool_name ] )   
    [WITH   
    { [ MARK_IN_USE_FOR_REMOVAL ] , [ NO_INFOMSGS ]  }  
    ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
( 'ALL' [, _pool\_name_ ] )  
ALL 指定所有受支持的缓存。  
_pool\_name_ 指定 Resource Governor 池缓存。 只释放与此池关联的条目。  
  
MARK_IN_USE_FOR_REMOVAL  
当不再使用当前使用的条目后，将它们分别从其各自所属的缓存中进行异步释放。 在 DBCC FREESYSTEMCACHE WITH MARK_IN_USE_FOR_REMOVAL 运行后，缓存中新建的条目不受影响。  
  
NO_INFOMSGS  
取消显示所有信息性消息。  
  
## <a name="remarks"></a>备注  
运行 DBCC FREESYSTEMCACHE 可清除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的计划缓存。 清除计划缓存将导致对所有即将到来的执行计划进行重新编译，并可能导致查询性能暂时性地突然降低。 对于计划缓存中每个已清除的缓存存储区，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志包含以下信息性消息： 

>`SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to 'DBCC FREEPROCCACHE' or 'DBCC FREESYSTEMCACHE' operations.`

 每隔五分钟，只要缓存在这段时间间隔内得到刷新，此消息就记录一次。

## <a name="result-sets"></a>结果集  
DBCC FREESYSTEMCACHE 返回：“DBCC 执行完毕。 如果 DBCC 输出了错误消息，请与系统管理员联系。”
  
## <a name="permissions"></a>权限  
需要对服务器的 ALTER SERVER STATE 权限。
  
## <a name="examples"></a>示例  
  
### <a name="a-releasing-unused-cache-entries-from-a-resource-governor-pool-cache"></a>A. 从资源调控器池缓存释放未使用的缓存条目  
下面的示例说明如何清除专属于某个指定资源调控器资源池的缓存。
  
```sql
-- Clean all the caches with entries specific to the resource pool named "default".  
DBCC FREESYSTEMCACHE ('ALL', default);  
```  
  
### <a name="b-releasing-entries-from-their-respective-caches-after-they-become-unused"></a>B. 当不再使用条目后，将它们分别从其各自所属的缓存中释放  
下面的示例使用 MARK_IN_USE_FOR_REMOVAL 子句，在不再使用条目后将它们从所有当前缓存中释放。
  
```sql
DBCC FREESYSTEMCACHE ('ALL') WITH MARK_IN_USE_FOR_REMOVAL;  
```  
  
## <a name="see-also"></a>另请参阅  
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC FREEPROCCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[DBCC FREESESSIONCACHE (Transact-SQL)](../../t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[资源调控器](../../relational-databases/resource-governor/resource-governor.md)
  
  
