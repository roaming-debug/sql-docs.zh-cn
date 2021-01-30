---
description: sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
title: sys.sp_xtp_merge_checkpoint_files (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/28/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sp_xtp_merge_checkpoint_files_TSQL
- sys.sp_xtp_merge_checkpoint_files
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_xtp_merge_checkpoint_files
ms.assetid: da04df2a-f7a1-41e7-a1ef-2d5d68919892
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 85ecf2fbec4e3e35940862e9b6a71460fab781e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102798"
---
# <a name="syssp_xtp_merge_checkpoint_files-transact-sql"></a>sys.sp_xtp_merge_checkpoint_files (Transact-SQL)
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  **sys.sp_xtp_merge_checkpoint_files** 合并指定事务范围内的所有数据和差异文件。  
  
 有关详细信息，请参阅 [创建和管理 Memory-Optimized 对象的存储](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
||  
|-|  
|**注意**：此存储过程在中已弃用 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 。 此方法不再需要，因此无法使用 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 。|  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.sp_xtp_merge_checkpoint_files database_name, @transaction_lower_bound, @transaction_upper_bound  
[;]  
```  
  
## <a name="arguments"></a>参数  
 *database_name*  
 数据库的名称，将对该数据库调用合并。 如果数据库没有内存中表，则此过程将返回用户错误。 如果数据库处于离线状态，则它恢复错误。  
  
 *lower_bound_Tid*  
  (bigint) 数据文件的事务的下限，如 [sys.dm_db_xtp_checkpoint_files &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md) 对应于合并的开始检查点文件中所示。 对于无效的 transactonId 值将生成错误。  
  
 *upper_bound_Tid*  
  (bigint) 数据文件的事务的上限，如 [sys.dm_db_xtp_checkpoint_files &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-checkpoint-files-transact-sql.md)中所示。 对于无效的 transactonId 值将生成错误。  
  
## <a name="return-code-values"></a>返回代码值  
 无  
  
## <a name="cursors-returned"></a>返回的游标  
 无  
  
## <a name="permissions"></a>权限  
 需要 sysadmin 固定服务器角色和 db_owner 固定数据库角色。  
  
## <a name="remarks"></a>备注  
 将有效范围内的所有数据和差异文件合并，以生成单个数据文件和差异文件。 此过程不支持合并策略。  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [内存中 OLTP（内存中优化）](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
