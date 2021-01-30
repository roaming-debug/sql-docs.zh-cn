---
description: sp_clean_db_free_space (Transact-SQL)
title: sp_clean_db_free_space (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_clean_db_free_space_TSQL
- sp_clean_db_free_space
dev_langs:
- TSQL
helpviewer_keywords:
- sp_clean_db_free_space
- ghost records
ms.assetid: faa96f7e-be92-47b1-8bc5-4dbba5331655
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1779f07e6c8923f56bb4597623a9b079679b3779
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206530"
---
# <a name="sp_clean_db_free_space-transact-sql"></a>sp_clean_db_free_space (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  删除因 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据修改例程而留在数据库页上的残留信息。 sp_clean_db_free_space 清除数据库中所有文件的所有页。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
sp_clean_db_free_space   
  [ @dbname = ] 'database_name'   
  [ , [ @cleaning_delay = ] 'delay_in_seconds' ] [;]  
```  
  
## <a name="arguments"></a>参数  
 @dbname = '*database_name*'  
 要清理的数据库的名称。 *dbname* 为 **sysname** ，且不能为 NULL。  
  
 @cleaning_delay = '*delay_in_seconds*'  
 指定各次页清理之间的延迟间隔。 这会有助于减轻对 I/O 系统的影响。 *delay_in_seconds* 的 **整数为 int** ，默认值为0。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="remarks"></a>备注  
 删除表的操作或更新操作导致行移动可以立即通过删除对该行的引用来释放页上的空间。 但是，在某些情况下，该行仍然作为虚影记录而保留在数据页上。 后台进程定期清除虚影记录。 不会在响应查询时返回该残留数据 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 但是，在数据或备份文件的物理安全性面临风险的环境中，可以使用 `sp_clean_db_free_space` 清除这些虚影记录。 若要针对每个数据库文件执行此操作，请使用 [ (transact-sql) sp_clean_db_file_free_space ](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)。 
  
 运行 sp_clean_db_free_space 所需的时间依文件大小、磁盘的可用空间和容量而定。 由于运行 `sp_clean_db_free_space` 会显著影响 i/o 活动，因此我们建议您在正常运行时间之外运行此过程。  
  
 在运行之前 `sp_clean_db_free_space` ，我们建议您创建完整数据库备份。  
  
 相关的 [sp_clean_db_file_free_space](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md) 存储过程可以清除单个文件。  
  
## <a name="permissions"></a>权限  
 要求具有数据库角色的成员身份 `db_owner` 。  
  
## <a name="examples"></a>示例  
 下面的示例从 `AdventureWorks2012` 数据库中清除所有残留信息。  
  
```sql  
USE master;  
GO  
EXEC sp_clean_db_free_space @dbname = N'AdventureWorks2012';  
```  
  
## <a name="see-also"></a>另请参阅  
 [数据库引擎存储过程 &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [虚影清除过程指南](../ghost-record-cleanup-process-guide.md)    
 [sp_clean_db_file_free_space (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-clean-db-file-free-space-transact-sql.md)
  
  
