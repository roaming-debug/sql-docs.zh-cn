---
description: sys.sp_xtp_control_query_exec_stats (Transact-SQL)
title: sys.sp_xtp_control_query_exec_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/13/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_control_query_exec_stats_TSQL
- sys.sp_xtp_control_query_exec_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_control_query_exec_stats
ms.assetid: 4838125d-ad1e-479e-b7d2-42655e8f4f02
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a1047918fd6d337d863dbf43d7058d1e3571e1cb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180215"
---
# <a name="syssp_xtp_control_query_exec_stats-transact-sql"></a>sys.sp_xtp_control_query_exec_stats (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  对实例的所有本机编译存储过程或特定的本机编译存储过程启用按查询的统计信息收集。  
  
 启用统计信息收集时性能下降。 如果只需要对一个或几个本机编译存储过程进行故障排除，则可仅对这些本机编译存储过程启用统计信息收集。  
  
 若要对所有本机编译的存储过程在过程级别启用统计信息收集，请参阅 [&#40;transact-sql&#41;sys.sp_xtp_control_proc_exec_stats ](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md)。  
  
## <a name="syntax"></a>语法  
  
```  
sp_xtp_control_query_exec_stats [ [ @new_collection_value = ] collection_value ],  
[ [ @database_id = ] database_id   
[ , [ @xtp_object_id = ] procedure_id ] ,   
[ @old_collection_value] ]  
```  
  
## <a name="arguments"></a>参数  
 @new_collection_value = *值*  
 决定打开 (1) 还是关闭 (0) 过程级统计信息收集。  
  
 @new_collection_value 当启动时，设置为零 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 @database_id = = *database_id*， @xtp_object_id = *procedure_id*  
 本机编译存储过程的数据库 ID 和对象 ID。 如果为实例启用了统计信息收集 ([sys.sp_xtp_control_proc_exec_stats &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md)) ，则会收集本机编译的存储过程的统计信息。 对实例关闭统计信息收集不会关闭对个别本机编译存储过程的统计信息收集。  
  
 使用 [sys. database &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)， [sys.databases &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)， [DB_ID &#40;transact-sql&#41;](../../t-sql/functions/db-id-transact-sql.md)，或 OBJECT_ID &#40;[transact-sql&#41;](../../t-sql/functions/object-id-transact-sql.md) 获取数据库和存储过程的 id。  
  
 @old_collection_value = *值*  
 返回当前状态。  
  
## <a name="return-code"></a>返回代码  
 0 表示成功。 非零表示失败。  
  
## <a name="permissions"></a>权限  
 要求用户为固定 sysadmin 角色的成员。  
  
## <a name="code-sample"></a>代码示例  
 以下代码示例展示如何对实例的所有本机编译存储过程启用统计信息收集，然后对特定的本机编译存储过程启用统计信息收集。  
  
```sql   
DECLARE @c bit  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1;  
  
EXEC sp_xtp_control_query_exec_stats @old_collection_value=@c output;  
SELECT @c AS 'collection status';  
  
EXEC [sys].[sp_xtp_control_query_exec_stats] @new_collection_value = 1,   
@database_id = 5, @xtp_object_id = 341576255;  
  
EXEC sp_xtp_control_query_exec_stats @database_id = 5,   
@xtp_object_id = 341576255, @old_collection_value=@c output;  
  
SELECT @c AS 'collection status';  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
