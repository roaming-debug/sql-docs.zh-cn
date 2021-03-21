---
description: 查询
title: 查询 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 5ff02a32-e8d3-479c-ae8b-07581e41f5f8
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8652e15ae1116fda061bc357e3c3feac492ef79d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754477"
---
# <a name="queries"></a>查询

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  数据操作语言 (DML) 是用于检索和使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 和 SQL 数据库中的数据的词汇。 大部分也可在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中使用（请查看各语句了解详细信息）。 使用这些语句可以从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库添加、修改、查询或删除数据。  
  
## <a name="in-this-section"></a>本节内容  
 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的 DML 语句。  

:::row:::
    :::column:::
        [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)
    :::column-end:::
    :::column:::
        [UPDATETEXT (Transact-SQL)](../../t-sql/queries/updatetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [MERGE (Transact-SQL)](../../t-sql/statements/merge-transact-sql.md)
    :::column-end:::
    :::column:::
        [WRITETEXT (Transact-SQL)](../../t-sql/queries/writetext-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [READTEXT (Transact-SQL)](../../t-sql/queries/readtext-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

&nbsp;

 下表列出了在多个 DML 语句或子句中使用的子句。  
  
|子句|可用于的语句|  
|------------|-------------------------------------|  
|[FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)|DELETE、SELECT、UPDATE|  
|[提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)|DELETE、INSERT、SELECT、UPDATE|  
|[OPTION 子句 (Transact-SQL)](../../t-sql/queries/option-clause-transact-sql.md)|DELETE、SELECT、UPDATE|  
|[OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)|DELETE、INSERT、MERGE、UPDATE|  
|[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)|DELETE、MERGE、SELECT、UPDATE|  
|[表值构造函数 (Transact-SQL)](../../t-sql/queries/table-value-constructor-transact-sql.md)|FROM、INSERT、MERGE|  
|[TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)|DELETE、INSERT、MERGE、SELECT、UPDATE|  
|[WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)|DELETE、SELECT、UPDATE、MATCH|  
|[WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)|DELETE、INSERT、MERGE、SELECT、UPDATE|  
| &nbsp; | &nbsp; |
