---
description: sp_kill_filestream_non_transacted_handles (Transact-SQL)
title: sp_kill_filestream_non_transacted_handles (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_kill_filestream_non_transacted_handles_TSQL
- sp_kill_filestream_non_transacted_handles
dev_langs:
- TSQL
helpviewer_keywords:
- sp_kill_filestream_non_transacted_handles
ms.assetid: 7188353e-ab29-49a0-8f25-7fb8ab122589
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dd3ba3d653b8f1626f89c7bb979580c666dd830a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165238"
---
# <a name="sp_kill_filestream_non_transacted_handles-transact-sql"></a>sp_kill_filestream_non_transacted_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  关闭 FileTable 数据的非事务性文件句柄。  
  
## <a name="syntax"></a>语法  
  
```sql  
sp_kill_filestream_non_transacted_handles [[ @table_name = ] 'table_name', [[ @handle_id = ] @handle_id]]  
```  
  
## <a name="arguments"></a>参数  
 *table_name*  
 要关闭其中的非事务性句柄的表的名称。  
  
 可以不 *handle_id* 传递 *Table_name* 来关闭 FileTable 的所有打开的非事务性句柄。  
  
 您可以为 *table_name* 的值传递 NULL，以关闭当前数据库中所有 filetable 的所有打开的非事务性句柄。 NULL 是默认值。  
  
 *handle_id*  
 要关闭的各个句柄的可选 ID。 可以从 [sys.dm_filestream_non_transacted_handles &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)动态管理视图中获取 *handle_id* 。 每个 ID 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中都是唯一的。 如果指定 *handle_id*，则还必须为 *table_name* 提供一个值。  
  
 可以为 *handle_id* 的值传递 NULL，以关闭 *Table_name* 指定的 FileTable 的所有打开的非事务性句柄。 NULL 是默认值。  
  
## <a name="return-code-value"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="result-set"></a>结果集  
 无。  
  
## <a name="general-remarks"></a>一般备注  
 **Sp_kill_filestream_non_transacted_handles** 所需的 *handle_id* 与其他 **kill** 命令中使用的 session_id 或工作单元无关。  
  
 有关详细信息，请参阅 [管理 FileTables](../../relational-databases/blob/manage-filetables.md)。  
  
## <a name="metadata"></a>元数据  
 有关开放式非事务性文件句柄的信息，请查询动态管理视图 [sys.dm_filestream_non_transacted_handles &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-filestream-non-transacted-handles-transact-sql.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 您必须具有 " **查看数据库状态** " 权限才能从 **sys.dm_FILESTREAM_non_transacted_handles** 动态管理视图获取文件句柄并运行 **sp_kill_filestream_non_transacted_handles**。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何调用 **sp_kill_filestream_non_transacted_handles** 关闭 FileTable 数据的非事务性文件句柄。  
  
```sql  
-- Close all open handles in the current database.  
sp_kill_filestream_non_transacted_handles  
  
-- Close all open handles in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable'  
  
-- Close a specific handle in myFileTable.  
sp_kill_filestream_non_transacted_handles @table_name = 'myFileTable', @handle_id = 0xFFFAAADD  
```  
  
 下面的示例演示如何使用脚本获取 *handle_id* 并将其关闭。  
  
```sql  
DECLARE @handle_id varbinary(16);  
DECLARE @table_name sysname;  
  
SELECT TOP 1 @handle_id = handle_id, @table_name = Object_name(table_id)  
FROM sys.dm_FILESTREAM_non_transacted_handles;  
  
EXEC sp_kill_filestream_non_transacted_handles @dbname, @table_name, @handle_id;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [管理 FileTable](../../relational-databases/blob/manage-filetables.md)  
 [Filestream 和 FileTable 动态管理视图 (Transact-sql) ](../system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)
 <br>[Filestream 和 FileTable 目录视图 (Transact-sql) ](../system-catalog-views/filestream-and-filetable-catalog-views-transact-sql.md)
 <br>[sp_filestream_force_garbage_collection (Transact-SQL)](filestream-and-filetable-sp-filestream-force-garbage-collection.md)
  
