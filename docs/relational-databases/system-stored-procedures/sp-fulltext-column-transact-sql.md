---
description: sp_fulltext_column (Transact-SQL)
title: sp_fulltext_column (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_fulltext_column_TSQL
- sp_fulltext_column
dev_langs:
- TSQL
helpviewer_keywords:
- sp_fulltext_column
ms.assetid: a84cc45d-1b50-44af-85df-2ea033b8a6a9
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4203f4ce8d603bd8715047b8a3f0ffb93e6b1e8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185403"
---
# <a name="sp_fulltext_column-transact-sql"></a>sp_fulltext_column (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-xxx-md.md)]

  指定表的某个特定列是否参与全文索引。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER 全文索引](../../t-sql/statements/alter-fulltext-index-transact-sql.md) 。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_fulltext_column [ @tabname= ] 'qualified_table_name' ,   
     [ @colname= ] 'column_name' ,   
     [ @action= ] 'action'   
     [ , [ @language= ] 'language_term' ]   
     [ , [ @type_colname= ] 'type_column_name' ]  
```  
  
## <a name="arguments"></a>参数  
`[ @tabname = ] 'qualified_table_name'` 为一个或两个部分组成的表名。 表必须在当前数据库中。 表必须具有全文索引。 *qualified_table_name* 为 **nvarchar (517)**，没有默认值。  
  
`[ @colname = ] 'column_name'`*Qualified_table_name* 中的列的名称。 列必须是字符、 **varbinary (max)** 或 **image** 列，不能是计算列。 *column_name* **sysname**，无默认值。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以为 **(max)** 或 **image** 数据类型的列中存储的文本数据创建全文索引。 不对图像和图片进行索引。  
  
`[ @action = ] 'action'` 要执行的操作。 *操作* 是 **varchar (20)**，无默认值，可以是下列值之一。  
  
|值|说明|  
|-----------|-----------------|  
|**add**|将 *qualified_table_name* *column_name* 添加到表的不活动全文索引中。 此操作可启用列以进行全文索引。|  
|**击落**|从表的不活动全文索引中移除 *qualified_table_name* *column_name* 。|  
  
`[ @language = ] 'language_term'` 列中存储的数据的语言。 有关中包含的语言的列表 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，请参阅 [&#40;transact-sql&#41;sys.fulltext_languages ](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)。  
  
> [!NOTE]  
>  如果列包含的数据使用了多种语言或不支持的语言，则使用“非特定语言”。 默认值通过配置选项“默认全文语言”指定。  
  
`[ @type_colname = ] 'type_column_name'`*Qualified_table_name* 中保存 *column_name* 的文档类型的列的名称。 此列必须是 **char**、 **nchar**、 **varchar** 或 **nvarchar**。 仅当 *column_name* 的数据类型为 **varbinary (max)** 或 **image** 类型时，才使用此方法。 *type_column_name* **sysname**，无默认值。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
 无  
  
## <a name="remarks"></a>备注  
 如果全文索引处于活动状态，则将停止所有正在进行的填充。 而且，如果一个具有活动全文索引的表启用了更改跟踪，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会确保索引是当前索引。 例如，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 停止对表的所有当前填充，删除现有索引，并启动新填充。  
  
 如果启用了更改跟踪，并且需要在保留索引的同时从全文索引中添加或删除列，则应停用表并添加或删除所需的列。 这些操作将冻结索引。 如果以后可以开始进行填充了，则可以重新激活表。  
  
## <a name="permissions"></a>权限  
 用户必须是 **db_ddladmin** 固定数据库角色的成员，或者是 **db_owner** 固定数据库角色的成员或表所有者。  
  
## <a name="examples"></a>示例  
 以下示例将 `DocumentSummary` 表的 `Document` 列添加到表的全文索引中。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_fulltext_column 'Production.Document', DocumentSummary, 'add';  
GO  
```  
  
 以下示例假定已对名为 `spanishTbl` 的表创建了一个全文索引。 若要将 `spanishCol` 列添加到全文索引，可执行以下存储过程：  
  
```  
EXEC sp_fulltext_column 'spanishTbl', 'spanishCol', 'add', 0xC0A;  
GO  
```  
  
 运行此查询时：  
  
```  
SELECT *   
FROM spanishTbl   
WHERE CONTAINS(spanishCol, 'formsof(inflectional, trabajar)')  
```  
  
 结果集将包括内含不同形态的 `trabajar` 一词的行，如 `trabajo`、`trabajamos` 和 `trabajan`。  
  
> [!NOTE]  
>  在单个全文查询函数子句中列出的所有列都必须使用相同的语言。  
  
## <a name="see-also"></a>另请参阅  
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_help_fulltext_columns &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)   
 [sp_help_fulltext_columns_cursor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [sp_help_fulltext_tables_cursor &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的全文搜索和语义搜索存储过程 ](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)  
  
  
