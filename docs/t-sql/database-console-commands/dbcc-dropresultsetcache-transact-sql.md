---
description: DBCC DROPRESULTSETCACHE (Transact-SQL)
title: DBCC DROPRESULTSETCACHE (Transact-SQL)
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 82762e132f3e8478a69eefff20820270c560b342
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440328"
---
# <a name="dbcc-dropresultsetcache--transact-sql"></a>DBCC DROPRESULTSETCACHE (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

从 Azure [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 数据库中删除所有结果集缓存条目。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC DROPRESULTSETCACHE
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="permissions"></a>权限

要求具有 DB_OWNER 固定服务器角色中的成员资格。

## <a name="remarks"></a>备注

- 此命令将清空所有查询的结果集缓存。  

- 关闭数据库的结果集缓存功能也会删除所有缓存的结果。  

- 暂停启用了结果集缓存的数据库不会删除缓存的结果。  

## <a name="see-also"></a>另请参阅

- [通过结果集缓存进行性能优化](/azure/sql-data-warehouse/performance-tuning-result-set-caching)</br>
- [ALTER DATABASE SET 选项 (Transact-SQL)](../statements/alter-database-transact-sql-set-options.md?view=azure-sqldw-latest)</br>
- [ALTER DATABASE (Transact-SQL)](../statements/alter-database-transact-sql.md?view=azure-sqldw-latest)</br>
- [SET RESULT SET CACHING &#40;Transact-SQL&#41;](../statements/set-result-set-caching-transact-sql.md)</br>
- [DBCC SHOWRESULTCACHESPACEUSED &#40;Transact-SQL&#41;](./dbcc-showresultcachespaceused-transact-sql.md)