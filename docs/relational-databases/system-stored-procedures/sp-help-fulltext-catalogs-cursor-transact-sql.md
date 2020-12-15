---
description: sp_help_fulltext_catalogs_cursor (Transact-SQL)
title: sp_help_fulltext_catalogs_cursor (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_help_fulltext_catalogs_cursor
- sp_help_fulltext_catalogs_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_fulltext_catalogs_cursor
ms.assetid: d44478d1-0cc4-415e-9d1a-6dccb64674fa
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2a999b80932f4b6fb81307614d2d03bbb301e260
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468408"
---
# <a name="sp_help_fulltext_catalogs_cursor-transact-sql"></a>sp_help_fulltext_catalogs_cursor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  使用游标返回指定的全文目录的 ID、名称、根目录、状态和全文索引表的数量。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) 目录视图。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_help_fulltext_catalogs_cursor [ @cursor_return= ] @cursor_variable OUTPUT ,   
     [ @fulltext_catalog_name = ] 'fulltext_catalog_name'   
```  
  
## <a name="arguments"></a>自变量  
`[ @cursor_return = ] @cursor_variable OUTPUT` 类型为 **cursor** 的输出变量。 游标是只读的可滚动动态游标。  
  
`[ @fulltext_catalog_name = ] 'fulltext_catalog_name'` 全文目录的名称。 *fulltext_catalog_name* **sysname**。 如果省略该参数或该参数值为 NULL，则返回与当前数据库关联的所有全文目录的信息。  
  
## <a name="return-code-values"></a>返回代码值  
 0（成功）或 1（失败）  
  
## <a name="result-sets"></a>结果集  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**fulltext_catalog_id**|**smallint**|全文目录的标识符。|  
|**NAME**|**sysname**|全文目录的名称。|  
|PATH|**nvarchar(260)**|从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，此子句没有任何作用。|  
|**状态**|**int**|目录的全文索引填充状态：<br /><br /> 0 = 空闲<br /><br /> 1 = 正在进行完全填充<br /><br /> 2 = 已暂停<br /><br /> 3 = 已中止<br /><br /> 4 = Recovering<br /><br /> 5 = 关闭<br /><br /> 6 = 正在进行增量填充<br /><br /> 7 = 正在生成索引<br /><br /> 8 = 磁盘已满。 已暂停<br /><br /> 9 = 更改跟踪|  
|**NUMBER_FULLTEXT_TABLES**|**int**|与目录关联的全文索引表的数量。|  
  
## <a name="permissions"></a>权限  
 Execute 权限默认授予 **public** 角色。  
  
## <a name="examples"></a>示例  
 下面的示例返回有关 `Cat_Desc` 全文目录的信息。  
  
```  
USE AdventureWorks2012;  
GO  
DECLARE @mycursor CURSOR;  
EXEC sp_help_fulltext_catalogs_cursor @mycursor OUTPUT, 'Cat_Desc';  
FETCH NEXT FROM @mycursor;  
WHILE (@@FETCH_STATUS <> -1)  
   BEGIN  
      FETCH NEXT FROM @mycursor;  
   END  
CLOSE @mycursor;  
DEALLOCATE @mycursor;  
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [FULLTEXTCATALOGPROPERTY (Transact-SQL)](../../t-sql/functions/fulltextcatalogproperty-transact-sql.md)   
 [sp_fulltext_catalog &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)   
 [sp_help_fulltext_catalogs &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
