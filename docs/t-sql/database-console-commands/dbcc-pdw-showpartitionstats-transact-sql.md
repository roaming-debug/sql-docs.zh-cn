---
description: DBCC PDW_SHOWPARTITIONSTATS (Transact-SQL)
title: DBCC PDW_SHOWPARTITIONSTATS (Transact-SQL)
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: pmasl
ms.author: umajay
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a4b3316c85c7f4e1cd0843f81d1e33b5a1e084fd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476748"
---
# <a name="dbcc-pdw_showpartitionstats-transact-sql"></a>DBCC PDW_SHOWPARTITIONSTATS (Transact-SQL)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

显示 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 数据库中表格每个分区的大小和行数。
  
![项目链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
--Show the partition stats for a table  
DBCC PDW_SHOWPARTITIONSTATS ( " [ database_name . [ schema_name ] . ] | [ schema_name.] table_name  ")  
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>参数  
 `[ database_name . [ schema_name ] . | schema_name . ] table_name`  
 要显示的表格的名称（具有一、二、三个部分）。  对于具有二或三个部分的名称，名称必须使用双引号 ("") 括起来。 可选择是否使用引号将具有一个部分的表格名称括起来。  
  
## <a name="permissions"></a>权限
需要 VIEW SERVER STATE 权限  。
  
## <a name="result-sets"></a>结果集  
此集是 DBCC PDW_SHOWPARTITIONSTATS 命令的结果。
  
|列名|数据类型|说明|  
|-----------------|---------------|-----------------|  
|partition_number|int|分区号。|  
|used_page_count|bigint|用于数据的页数。|  
|reserved_page_count|bigint|为分区保留的页数。|  
|row_count|bigint|分区中的行数。|  
|pdw_node_id|int|数据的计算节点。|  
|distribution_id|int|数据的分发标识符。|  
  
## <a name="examples-sssdw-and-sspdw"></a>示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="a-dbcc-pdw_showpartitionstats-basic-syntax-examples"></a>A. DBCC PDW_SHOWPARTITIONSTATS 基本语法示例  
以下示例显示 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 数据库中 FactInternetSales 表格按分区计算的空间和行数。
  
```sql
DBCC PDW_SHOWPARTITIONSTATS ("ssawPDW.dbo.FactInternetSales");  
DBCC PDW_SHOWPARTITIONSTATS ("dbo.FactInternetSales");  
DBCC PDW_SHOWPARTITIONSTATS (FactInternetSales);  
```  

## <a name="see-also"></a>另请参阅

- [DBCC PDW_SHOWEXECUTIONPLAN (Transact-SQL)](dbcc-pdw-showexecutionplan-transact-sql.md)  
- [DBCC PDW_SHOWSPACEUSED (Transact-SQL)](dbcc-pdw-showspaceused-transact-sql.md)  
