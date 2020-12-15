---
description: sp_help_fulltext_tables_cursor (Transact-SQL)
title: sp_help_fulltext_tables_cursor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_fulltext_tables_cursor
- sp_help_fulltext_tables_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_tables_cursor
ms.assetid: 155791eb-8832-4596-8487-7fc70dfba5b9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca07485062d39e2fa547e524e2b0368b19e9b577
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468398"
---
# <a name="sp_help_fulltext_tables_cursor-transact-sql"></a>sp_help_fulltext_tables_cursor (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  使用游标返回为全文索引注册的表的列表。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 改为使用新的 **sys.fulltext_indexes** 目录视图。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.fulltext_indexes ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_fulltext_tables_cursor [ @cursor_return = ] @cursor_variable OUTPUT   
     [ , [ @fulltext_catalog_name = ] 'fulltext_catalog_name' ]   
     [ , [ @table_name = ] 'table_name' ]  
```  
  
## <a name="arguments"></a>自变量  
`[ @cursor_return = ] @cursor_variable OUTPUT` 类型为 **cursor** 的输出变量。 游标是只读的可滚动动态游标。  
  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` 全文目录的名称。 *fulltext_catalog_name* 的默认值为 **sysname**，默认值为 NULL。 如果省略 *fulltext_catalog_name* 或为 NULL，则返回与数据库关联的所有全文索引表。 如果指定了 *fulltext_catalog_name* ，但省略了 *TABLE_NAME* 或为 NULL，则将为与此目录关联的每个全文索引表检索全文索引信息。 如果同时指定 *fulltext_catalog_name* 和 *table_name* ，则在 *table_name* 与 *fulltext_catalog_name* 关联时返回一行;否则，将引发错误。  
  
`[ @table_name = ] 'table_name'` 请求全文元数据的一个或两个部分组成的表名。 *table_name* 为 **nvarchar (517)**，默认值为 NULL。 如果仅指定 *table_name* ，则仅返回与 *table_name* 相关的行。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**TABLE_OWNER**|**sysname**|表所有者。 这是创建该表的数据库用户的名称。|  
|**TABLE_NAME**|**sysname**|表名。|  
|**FULLTEXT_KEY_INDEX_NAME**|**sysname**|对于指定为唯一键列的列施加 UNIQUE 约束的索引。|  
|**FULLTEXT_KEY_COLID**|**int**|以 FULLTEXT_KEY_NAME 标识的唯一索引的列 ID。|  
|**FULLTEXT_INDEX_ACTIVE**|**int**|指定该表中为全文索引标记的列是否适于查询：<br /><br /> 0 = 非活动<br /><br /> 1 = 活动|  
|**FULLTEXT_CATALOG_NAME**|**sysname**|全文索引数据所在的全文目录。|  
  
## <a name="permissions"></a>权限  
 Execute 权限默认授予 **public** 角色的成员。  
  
## <a name="examples"></a>示例  
 以下示例返回与 `Cat_Desc` 全文目录相关联的全文索引表的名称。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_tables_cursor @mycursor OUTPUT, 'Cat_Desc';  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END;  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [INDEXPROPERTY (Transact-SQL)](../../t-sql/functions/indexproperty-transact-sql.md)   
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [sp_fulltext_table &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)   
 [sp_help_fulltext_tables &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
