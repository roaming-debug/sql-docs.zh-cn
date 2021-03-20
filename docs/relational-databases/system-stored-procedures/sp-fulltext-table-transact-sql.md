---
description: sp_fulltext_table (Transact-SQL)
title: sp_fulltext_table (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_table_TSQL
- sp_fulltext_table
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_table
ms.assetid: a765f311-07fc-4af3-b74c-e9a027fbecce
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7184d4d9fae78821c0558ae193e7a830f6f28f28
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749957"
---
# <a name="sp_fulltext_table-transact-sql"></a>sp_fulltext_table (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  标记或取消标记要编制全文索引的表。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 " [创建全文索引](../../t-sql/statements/create-fulltext-index-transact-sql.md)"、" [更改全文](../../t-sql/statements/alter-fulltext-index-transact-sql.md)索引" 和 " [删除全文索引](../../t-sql/statements/drop-fulltext-index-transact-sql.md) "。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_fulltext_table   
   [ @tabname= ] 'qualified_table_name'           
   , [ @action= ] 'action'   
   [   
   , [ @ftcat= ] 'fulltext_catalog_name'           
   , [ @keyname= ] 'unique_index_name'   
   ]  
```  
  
## <a name="arguments"></a>参数  
`[ @tabname = ] 'qualified_table_name'` 为一个或两个部分组成的表名。 表必须在当前数据库中。 *qualified_table_name* 为 **nvarchar (517)**，无默认值。  
  
`[ @action = ] 'action'` 要执行的操作。 *操作* 为 **nvarchar (50)**，无默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**创建**|为 *qualified_table_name* 引用的表创建全文索引的元数据，并指定此表的全文索引数据应驻留在 *fulltext_catalog_name* 中。 此操作还将 *unique_index_name* 的用法指定为全文键列。 这个唯一的索引必须已经存在，并且必须已在表的某个列上定义。<br /><br /> 完成了全文目录填充后，才能对该表执行全文检索。|  
|**击落**|删除 *qualified_table_name* 的全文索引的元数据。 如果全文索引是活动的，则将自动停用该全文索引，然后将其删除。 在删除全文索引之前，不必删除列。|  
|**激活**|激活在停用后，为 *qualified_table_name* 收集全文索引数据的功能。 在激活全文索引之前，应该至少有一列参与了全文索引。<br /><br /> 只要添加了第一个要编制索引的列，便会自动激活全文索引（进行填充）。 如果从索引中删除最后一个列，该索引便成为非活动索引。 如果启用了更改跟踪，则激活非活动索引时将启动一个新填充。<br /><br /> 请注意，这实际上并不是填充全文索引，而只是将表注册到文件系统中的全文目录中，以便可以在下一次全文索引填充期间检索 *qualified_table_name* 中的行。|  
|**停用**|停用 *qualified_table_name* 的全文索引，以便无法再为 *qualified_table_name* 收集全文索引数据。 全文索引元数据依然保留，该表可以被重新激活。<br /><br /> 如果启用了更改跟踪，停用一个活动索引将冻结索引的状态：停止正在进行的所有填充，不再向索引传播更改。|  
|**start_change_tracking**|启动全文索引的增量填充。 如果表没有时间戳，则启动全文索引的完全填充。 开始跟踪对表所做的更改。<br /><br /> 全文更改跟踪不跟踪对类型为 **image**、 **text** 或 **ntext** 的全文索引列执行的任何 WRITETEXT 或 UPDATETEXT 操作。|  
|**stop_change_tracking**|停止对表更改的跟踪。|  
|**update_index**|将当前的一组跟踪的更改传播给全文索引。|  
|**start_background_updateindex**|一旦发生更改，就启动将被跟踪的更改传播给全文索引。|  
|**stop_background_updateindex**|一旦发生更改，就停止将被跟踪的更改传播给全文索引。|  
|**start_full**|启动表的全文索引的完全填充。|  
|**start_incremental**|启动表的全文索引的增量填充。|  
|**停止**|停止完全填充或增量填充。|  
  
`[ @ftcat = ] 'fulltext_catalog_name'` 是一个有效的现有全文目录名称，用于 **创建** 操作。 对于其他所有操作，此参数必须为 NULL。 *fulltext_catalog_name* 的默认值为 **sysname**，默认值为 NULL。  
  
`[ @keyname = ] 'unique_index_name'`**创建** 操作 *qualified_table_name* 的有效单一键列唯一不可 null 索引。 对于其他所有操作，此参数必须为 NULL。 *unique_index_name* 的默认值为 **sysname**，默认值为 NULL。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 为特定表停用全文索引后，现有的全文索引将保持不变，直到下一次完全填充;但是，不使用此索引，因为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 阻止对停用的表的查询。  
  
 如果重新激活该表，但不重新填充索引，则仍可使用旧索引对剩余的启用了全文索引的非新列进行查询。 在指定了完全全文列搜索的查询中，将匹配已删除的列中的数据。  
  
 为全文索引定义了表后，将全文唯一键列从一种数据类型切换到另一种数据类型，方法是：将该列的数据类型更改为另一列，或将全文唯一键从一列更改为另一列，而不进行完全重新填充可能会导致在后续查询中出现失败，并返回错误消息： " *data_type* 全文搜索键值 *key_value* 的转换为类型失败。 若要防止出现这种情况，请使用 **sp_fulltext_table** 的 **删除** 操作删除此表的全文定义，并使用 **sp_fulltext_table** 和 **sp_fulltext_column** 对其重新定义。  
  
 必须将全文键列定义为 900 字节或更少。 考虑到性能原因，建议尽量使用较小的键列。  
  
## <a name="permissions"></a>权限  
 只有 **sysadmin** 固定服务器角色的成员、 **db_owner** 和 **db_ddladmin** 固定数据库角色的成员，或者具有全文目录的 reference 权限的用户才能 **sp_fulltext_table** 执行。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enabling-a-table-for-full-text-indexing"></a>A. 启用要进行全文索引的表  
 以下示例将为 `Document` 数据库的 `AdventureWorks` 表创建全文索引元数据。 `Cat_Desc` 是一个全文目录。 `PK_Document_DocumentID` 是针对 `Document` 的唯一单列索引。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'create', 'Cat_Desc', 'PK_Document_DocumentID';  
--Add some columns  
EXEC sp_fulltext_column 'Production.Document','DocumentSummary','add';  
-- Activate the full-text index  
EXEC sp_fulltext_table 'Production.Document','activate';  
GO  
```  
  
### <a name="b-activating-and-propagating-track-changes"></a>B. 激活并传播跟踪更改  
 下例将在更改发生时激活跟踪更改并开始将跟踪的更改传播给全文索引。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'Start_change_tracking';  
EXEC sp_fulltext_table 'Production.Document', 'Start_background_updateindex';  
GO  
```  
  
### <a name="c-removing-a-full-text-index"></a>C. 删除全文索引  
 以下示例将删除 `Document` 数据库的 `AdventureWorks` 表的全文索引元数据。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_table 'Production.Document', 'drop';  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [sp_helpindex &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的全文搜索和语义搜索存储过程 ](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
