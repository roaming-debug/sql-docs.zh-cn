---
description: CUME_DIST (Transact-SQL)
title: CUME_DIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CUME_DIST
- CUME_DIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CUME_DIST function
- analytic functions, CUME_DIST
ms.assetid: 491b07f3-9ffd-4cdd-93e5-5abb636fc5ef
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4252c237e234fdc380ca0f4b18cfcf55f6bf9ae4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093716"
---
# <a name="cume_dist-transact-sql"></a>CUME_DIST (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，此函数会计算某个值在某个值组内的累积分布。 换言之，`CUME_DIST` 计算某指定值在一组值中的相对位置。 假定采用升序，行 _r_ 中 `CUME_DIST` 的值定义为低于或等于行 _r_ 的值的行数除以在分区或查询结果集中求出的行数。 `CUME_DIST` 类似于 `PERCENT_RANK` 函数。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CUME_DIST( )  
    OVER ( [ partition_by_clause ] order_by_clause )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_)  

_partition\_by\_clause_ 将 FROM 子句结果集划分为要应用函数的分区。 如果未指定 _partition\_by\_clause_ 参数，则 `CUME_DIST` 将查询结果集的所有行视为单个组。 _order\_by\_clause_ 确定操作发生的逻辑顺序。 `CUME_DIST` 需要 _order\_by\_clause_。 `CUME_DIST` 不接受 OVER 语法的 \<rows or range clause>。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。
  
## <a name="return-types"></a>返回类型
**float(53)**
  
## <a name="remarks"></a>备注  
`CUME_DIST` 返回一系列大于 0 且小于或等于 1 的值。 关联值始终计算为相同的累积分布值。 `CUME_DIST` 默认包含 NULL 值，且将这些值视为最低的可能值。
  
`CUME_DIST` 具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。
  
## <a name="examples"></a>示例  
此示例使用 `CUME_DIST` 函数计算给定部门内每个雇员的薪金百分比。 `CUME_DIST` 返回表示薪金低于或等于同一个部门中当前雇员的雇员百分比的值。 `PERCENT_RANK` 函数计算雇员的薪金在部门内的百分比排名。 为了按部门对结果集行进行分区，示例指定 _partition\_by\_clause_ 值。 OVER 子句中的 ORDER BY 子句在逻辑上对每个分区中的行进行排序。 SELECT 语句中的 ORDER BY 子句确定结果集的显示顺序。
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Department, LastName, Rate,   
       CUME_DIST () OVER (PARTITION BY Department ORDER BY Rate) AS CumeDist,   
       PERCENT_RANK() OVER (PARTITION BY Department ORDER BY Rate ) AS PctRank  
FROM HumanResources.vEmployeeDepartmentHistory AS edh  
    INNER JOIN HumanResources.EmployeePayHistory AS e    
    ON e.BusinessEntityID = edh.BusinessEntityID  
WHERE Department IN (N'Information Services',N'Document Control')   
ORDER BY Department, Rate DESC;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Department             LastName               Rate                  CumeDist               PctRank  
---------------------- ---------------------- --------------------- ---------------------- ----------------------  
Document Control       Arifin                 17.7885               1                      1  
Document Control       Norred                 16.8269               0.8                    0.5  
Document Control       Kharatishvili          16.8269               0.8                    0.5  
Document Control       Chai                   10.25                 0.4                    0  
Document Control       Berge                  10.25                 0.4                    0  
Information Services   Trenary                50.4808               1                      1  
Information Services   Conroy                 39.6635               0.9                    0.888888888888889  
Information Services   Ajenstat               38.4615               0.8                    0.666666666666667  
Information Services   Wilson                 38.4615               0.8                    0.666666666666667  
Information Services   Sharma                 32.4519               0.6                    0.444444444444444  
Information Services   Connelly               32.4519               0.6                    0.444444444444444  
Information Services   Berg                   27.4038               0.4                    0  
Information Services   Meyyappan              27.4038               0.4                    0  
Information Services   Bacon                  27.4038               0.4                    0  
Information Services   Bueno                  27.4038               0.4                    0  
(15 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅
[PERCENT_RANK (Transact-SQL)](../../t-sql/functions/percent-rank-transact-sql.md)
  
  
