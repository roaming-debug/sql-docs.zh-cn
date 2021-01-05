---
description: DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)
title: DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)
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
ms.openlocfilehash: b3cfbc2117d6c3dd3599443a6f5b6e71a2d3186e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637379"
---
# <a name="dbcc-showresultcachespaceused-transact-sql"></a>DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

显示 Azure [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 数据库的结果集缓存所用的存储空间。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DBCC SHOWRESULTCACHESPACEUSED  
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="remarks"></a>备注

`DBCC SHOWRESULTCACHESPACEUSED` 命令不使用任何参数，并且会返回运行此命令的数据库所使用的空间。

## <a name="permissions"></a>权限

需要 VIEW SERVER STATE 权限。
  
## <a name="result-sets"></a>结果集  
  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|reserved_space|bigint|用于数据库的总空间，单位为 KB。 该数字在缓存的结果集增加时会改变。|  
|data_space|bigint|用于数据的空间，单位为 KB。|  
|index_space|bigint|用于索引的空间，单位为 KB。|  
|unused_space|bigint|保留空间中未使用的空间，单位为 KB。|  

## <a name="see-also"></a>另请参阅

[通过结果集缓存进行性能优化](/azure/sql-data-warehouse/performance-tuning-result-set-caching)</br>
[ALTER DATABASE SET 选项 (Transact-SQL)](../statements/alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)</br>
[ALTER DATABASE (Transact-SQL)](../statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)</br>
[SET RESULT SET CACHING &#40;Transact-SQL&#41;](../statements/set-result-set-caching-transact-sql.md)</br>
[DBCC DROPRESULTSETCACHE &#40;Transact-SQL&#41;](./dbcc-dropresultsetcache-transact-sql.md)